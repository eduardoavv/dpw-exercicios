# E00.5 — Roteiro de diagnóstico

## 1. Roteiro de diagnóstico

| Passo | Comando | Se a saída for X | Então |
|---|---|---|---|
| 1 | `Get-Location` | A pasta não é `C:\dev\dpw-exercicios` | Entrar na pasta correta com `cd C:\dev\dpw-exercicios`. |
| 2 | `Test-Path .\package.json` | `False` | O projeto não está nessa pasta; localizar a raiz correta antes de continuar. |
| 3 | `Test-Path .\node_modules` | `False` | As dependências não estão instaladas; executar `pnpm install` ou o comando de verificação do projeto. |
| 4 | `pnpm list --depth 0` | O pacote esperado não aparece ou ocorre erro | Verificar o `package.json` e reinstalar as dependências com `pnpm install`. |
| 5 | `pnpm verificar` | O comando termina sem erro | O ambiente básico está funcionando; se o `import` continuar falhando, investigar o nome do pacote, caminho do import e export utilizado. |

A ordem elimina primeiro hipóteses simples e baratas: localização, existência do projeto, presença das dependências e instalação dos pacotes. Só depois parte para a execução e para uma investigação específica do `import`.

## 2. Demonstração com falha provocada

### Falha provocada

**Comando:**

```powershell
Remove-Item -Recurse -Force .\node_modules
```

**Verificação:**

```powershell
Test-Path .\node_modules
```

**Saída:**

```text
False
```

Isso confirmou que `node_modules` havia sido removido.

### Execução do diagnóstico

**Comando:**

```powershell
pnpm verificar
```

**Saída:**

```text
✓ Lockfile passes supply-chain policies (verified 1h ago)
Lockfile is up to date, resolution step is skipped
Packages: +1
+
Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: C:\Users\Pichau\AppData\Local\pnpm\store\v11
  Virtual store is at:             node_modules/.pnpm
Progress: resolved 1, reused 1, downloaded 0, added 1, done

devDependencies:
+ prettier 3.9.6

Done in 591ms using pnpm v11.24.0
$ node --version && pnpm --version
v24.19.0
11.24.0
```

O comando de verificação detectou a ausência das dependências e restaurou o `prettier`.

### Passo 1

**Comando:**

```powershell
Get-Location
```

**Saída:**

```text
Path
----
C:\dev\dpw-exercicios
```

A pasta está correta, então a localização não era a causa.

### Passo 2

**Comando:**

```powershell
Test-Path .\package.json
```

**Saída:**

```text
True
```

O `package.json` existe, confirmando que estamos na raiz do projeto.

### Passo 3

**Comando:**

```powershell
Test-Path .\node_modules
```

**Saída inicial:**

```text
False
```

Isso confirmou a causa provocada: as dependências não estavam instaladas.

Após executar `pnpm verificar`, o ambiente foi restaurado.

### Passo 4

**Comando:**

```powershell
pnpm list --depth 0
```

**Saída:**

```text
Legend: production dependency, optional only, dev only

dpw-exercicios@1.0.0 C:\dev\dpw-exercicios (PRIVATE)
│
│   devDependencies:
└── prettier@3.9.6

1 package
```

O `prettier` está instalado como dependência de desenvolvimento.

### Passo 5

**Comando:**

```powershell
pnpm verificar
```

**Saída:**

```text
$ node --version && pnpm --version
v24.19.0
11.24.0
```

O ambiente voltou a funcionar corretamente.

### Conclusão

A falha provocada foi a remoção de `node_modules`. O diagnóstico confirmou que a pasta do projeto estava correta e que o `package.json` existia. A ausência das dependências foi identificada e o `pnpm verificar` restaurou o ambiente, eliminando a causa do problema.
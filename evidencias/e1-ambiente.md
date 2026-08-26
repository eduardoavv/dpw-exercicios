# E00.1 — Ambiente reprodutível

## Prova de reprodutibilidade

### 1. Remoção do node_modules

Comando:

Remove-Item -Recurse -Force node_modules

Saída:

PS C:\dev\dpw-exercicios> Remove-Item -Recurse -Force node_modules
PS C:\dev\dpw-exercicios>

### 2. Instalação com lockfile congelado

Comando:

pnpm install --frozen-lockfile

Saída:

✓ Lockfile passes supply-chain policies (verified 3m ago)
Lockfile is up to date, resolution step is skipped
Packages: +1

- Packages are hard linked from the content-addressable store to the virtual store.
  Content-addressable store is at: C:\Users\Pichau\AppData\Local\pnpm\store\v11
  Virtual store is at: node_modules/.pnpm
  Progress: resolved 1, reused 1, downloaded 0, added 1, done

devDependencies:

- prettier 3.9.6

Done in 580ms using pnpm v11.24.0
PS C:\dev\dpw-exercicios>

### 3. Verificação do Git

Comando:

git status --short

Saída:

?? .env.example
?? .gitattributes
?? .gitignore
?? evidencias/
?? package.json
?? pnpm-lock.yaml

O pnpm-lock.yaml não aparece como modificado (M), mostrando que a instalação com --frozen-lockfile não alterou o lockfile.

## Link permanente para o .gitignore

https://github.com/eduardoavv/dpw-exercicios/blob/3cc8585/.gitignore

## Por que o pnpm-lock.yaml é versionado e o node_modules não?

O pnpm-lock.yaml é versionado para garantir que todas as pessoas instalem as mesmas versões exatas das dependências. O node_modules não é versionado porque é uma pasta gerada localmente pelo gerenciador de pacotes e pode ser recriada usando o pnpm install.

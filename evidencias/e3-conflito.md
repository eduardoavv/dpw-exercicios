E00.3 — Conflito de merge, provocado sozinho

## 1. Saída do merge que acusou o conflito

**Comando:**

```powershell
git merge feat/titulo-b
```

**Saída:**

```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```
## 2. Conteúdo do arquivo durante o conflito

**Comando:**

```powershell
Get-Content README.md
```

**Saída:**

```text
<<<<<<< HEAD
# DPW â€” ExercÃ­cios do M00 !!
=======
# DPW â€” ExercÃ­cios do M00 @-@
>>>>>>> feat/titulo-b
```
## 3. Grafo do histórico

**Comando:**

```powershell
git log --graph --oneline --all
```

**Saída:**

```text
*   de04f1a (HEAD -> main) merge: resolve conflito nos titulos
|\
| * 285f94d (feat/titulo-b) docs: altera titulo para versao B
* | e1fd736 (feat/titulo-a) docs: altera titulo para versao A
|/
* afcf377 (origin/main) docs: adiciona evidencia da arqueologia de historico
* b91b526 docs: registra evidencia do ambiente
* 3cc8585 chore: inicializa exercicios do M00
```

## 4. Links permanentes

**Commit de merge:**

https://github.com/eduardoavv/dpw-exercicios/commit/de04f1a

**Página Network:**

https://github.com/eduardoavv/dpw-exercicios/network

## 5. Por que o Git não conseguiu resolver sozinho?

As duas branches alteraram a mesma linha do `README.md`, mas com conteúdos diferentes. O Git não conseguiu determinar automaticamente qual versão deveria permanecer. Por isso, foi necessário resolver o conflito manualmente e criar o commit de merge.
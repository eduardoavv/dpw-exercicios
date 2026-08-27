# E00.4 — Desfazer sem pânico

## 1. Descartar alteração ainda não adicionada

**Comando usado para verificar antes:**

```powershell
git status
```

**Saída antes:**

```text
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

**Comando:**

```powershell
git restore README.md
```

**Saída depois:**

```powershell
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

**Efeito:** `git restore` descartou a alteração do working directory sem alterar o histórico.

---

## 2. Tirar do stage um arquivo adicionado por engano

**Comando:**

```powershell
git restore --staged README.md
```

**Saída depois:**

```powershell
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
        modified:   README.md

no changes added to commit
```

**Efeito:** o arquivo foi retirado do stage, mas a alteração permaneceu no working directory.

---

## 3. Corrigir a mensagem do último commit

**Comando usado para criar o commit com mensagem errada:**

```powershell
git commit -m "docs: mensagem errada"
```

**Saída do log antes:**

```powershell
git log --oneline -3
```

```text
50fcb19 (HEAD -> main) docs: mensagem errada
98d66db (origin/main) docs: adiciona evidencia do conflito de merge
de04f1a merge: resolve conflito nos titulos
```

**Comando:**

```powershell
git commit --amend -m "docs: atualiza README"
```

**Saída do log depois:**

```powershell
git log --oneline -3
```

```text
b1374a3 (HEAD -> main) docs: atualiza README
98d66db (origin/main) docs: adiciona evidencia do conflito de merge
de04f1a merge: resolve conflito nos titulos
```

**Efeito:** o último commit foi substituído por outro com a mensagem corrigida, antes de ser enviado ao remoto.

---

## 4. Desfazer o último commit mantendo as alterações

**Comando:**

```powershell
git reset --soft HEAD~1
```

**Saída do status depois:**

```powershell
git status
```

```text
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
        modified:   README.md
```

**Saída do log depois:**

```powershell
git log --oneline -3
```

```text
98d66db (HEAD -> main, origin/main) docs: adiciona evidencia do conflito de merge
de04f1a merge: resolve conflito nos titulos
285f94d (feat/titulo-b) docs: altera titulo para versao B
```

**Efeito:** o commit foi removido do histórico local, mas suas alterações foram mantidas no stage.

---

## 5. Reverter um commit já enviado ao remoto

**Commit criado e enviado:**

```powershell
git commit -m "docs: teste de revert"
```

**Log antes do revert:**

```text
f917a59 (HEAD -> main) docs: teste de revert
98d66db (origin/main) docs: adiciona evidencia do conflito de merge
de04f1a merge: resolve conflito nos titulos
```

O commit `f917a59` foi enviado ao remoto com `git push`.

**Comando:**

```powershell
git revert HEAD
```

**Resultado:**

```text
Revert "docs: teste de revert"
```

**Log depois:**

```powershell
git log --oneline -3
```

```text
5f85b95 (HEAD -> main) Revert "docs: teste de revert"
f917a59 (origin/main) docs: teste de revert
98d66db docs: adiciona evidencia do conflito de merge
```

**Status depois:**

```powershell
git status
```

```text
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
nothing to commit, working tree clean
```

Após isso, o commit de revert `5f85b95` foi enviado ao remoto com `git push`.

**Efeito:** o commit original permanece no histórico e um novo commit desfaz suas alterações.

**Link permanente para o commit de revert:**

https://github.com/eduardoavv/dpw-exercicios/commit/5f85b95

---

## Reflog final

**Comando:**

```powershell
git reflog -10
```

**Saída:**

```text
5f85b95 (HEAD -> main, origin/main) HEAD@{0}: revert: Revert "docs: teste de revert"
f917a59 HEAD@{1}: commit: docs: teste de revert
98d66db HEAD@{2}: reset: moving to HEAD~1
b1374a3 HEAD@{3}: commit (amend): docs: atualiza README
50fcb19 HEAD@{4}: commit: docs: mensagem errada
98d66db HEAD@{5}: commit: docs: adiciona evidencia do conflito de merge
de04f1a HEAD@{6}: commit (merge): merge: resolve conflito nos titulos
e1fd736 (feat/titulo-a) HEAD@{7}: checkout: moving from feat/titulo-b to main
285f94d (feat/titulo-b) HEAD@{8}: commit: docs: altera titulo para versao B
afcf377 HEAD@{9}: checkout: moving from main to feat/titulo-b
```

## Por que o caso 5 é diferente do caso 4?

No caso 4, `git reset --soft` reescreveu o histórico local e removeu o último commit do histórico, mantendo suas alterações no stage.

No caso 5, o commit já havia sido enviado ao remoto, então não era seguro reescrever o histórico compartilhado.

O `git revert` cria um novo commit que desfaz o anterior, preservando o histórico e evitando problemas para quem já baixou o repositório.
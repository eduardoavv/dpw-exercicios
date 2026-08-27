E00.2 — Arqueologia de histórico
1. Quantos commits o repositório tem?

Comando:

git rev-list --count HEAD

Saída:

21672
2. Qual foi o primeiro commit, e em que data?

Comando:

git log --reverse --format="%H | %ad | %s" --date=iso-strict | Select-Object -First 1

Saída:

f7c8d10fb20943bc7102c73d5ecbe49e6c0b5ea1 | 2017-01-08T15:09:41+01:00 |
3. Quem mais modificou packages/core/injector/injector.ts?

Comando:

git shortlog -sn -- packages/core/injector/injector.ts

Saída:

    90  Kamil Myśliwiec
    12  Jay McDoniel
     6  Kamil Mysliwiec
     4  Jean-Baptiste Pionnier
     4  Livio Brunner
     3  Micael Levi (lab)
     2  Jiri Hajek
     2  Micael Levi L. Cavalcante
     2  mag123c
     1  Elies Lou
     1  Lee Donghyun
     1  Livio
     1  Lutz
     1  Nathan Knight
     1  Sergei Yudin
     1  Tony133
     1  codytseng
     1  cojack
     1  coti-z
     1  jacob87o2
     1  malekelkssas
     1  tooleks
     1  youmoo
4. O que mudou no último commit que tocou esse arquivo?

Comando:

git show --format=fuller 45485b54210e06a517c1ebf86b42b1ea99fc3fe2 -- packages/core/injector/injector.ts

Saída:

commit 45485b54210e06a517c1ebf86b42b1ea99fc3fe2
Author:     Kamil Myśliwiec <mail@kamilmysliwiec.com>
AuthorDate: Tue Aug 25 12:48:22 2026 +0200
Commit:     Kamil Myśliwiec <mail@kamilmysliwiec.com>
CommitDate: Tue Aug 25 13:02:32 2026 +0200

    fix(core): circular durable providers issue #17562

diff --git a/packages/core/injector/injector.ts b/packages/core/injector/injector.ts
index 32b9f2850..aad3a899c 100644
--- a/packages/core/injector/injector.ts
+++ b/packages/core/injector/injector.ts
@@ -581,7 +581,7 @@ export class Injector {
        * that eventual lazily created instance will be merged with the prototype
        * instantiated beforehand.
        */
-      instanceHost.donePromise &&
+      if (instanceHost.donePromise) {
         void instanceHost.donePromise
           .then(() =>
             this.loadProvider(instanceWrapper, moduleRef, resolutionContext),
@@ -589,6 +589,20 @@ export class Injector {
           .catch(err => {
             instanceWrapper.settlementSignal?.error(err);
           });
+      } else {
+        /**
+         * No load has ever been scheduled for this context (e.g., request-scoped
+         * providers are no longer instantiated during static bootstrap, so a fresh
+         * durable/request sub-tree host has no inherited `donePromise`).
+         * Load it now; if a circular dependency is truly in-flight, the nested
+         * lookup will find this host pending and defer through its `donePromise`.
+         */
+        await this.loadProvider(
+          instanceWrapper,
+          instanceWrapper.host ?? moduleRef,
+          resolutionContext,
+        );
+      }
     }
     if (instanceWrapper.async) {
       const host = instanceWrapper.getInstanceByContextId(
5. Quantos commits foram feitos nos últimos 90 dias?

Comando:

git rev-list --count --since="90 days ago" HEAD

Saída:

695
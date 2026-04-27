# Taller de Git: Branques i Merge

Segueix les instruccions en ordre i **espera la indicació del professor/a** abans de passar a cada fase.

# FASE 1 — Branques sense conflicte

**Objectiu:** aprendre el cicle complet `clone → branch → switch → commit → push → pull` i veure com les branques dels companys apareixen al repositori compartit.

### Pas 1.1 — Clonar el repositori

```bash
git clone https://github.com/CE-Python/taller_branques.git
cd taller_branques
```

Comprova on ets:

```bash
git status
git branch
```

Hauries de veure que estàs a la branca `main`.

### Pas 1.2 — Crear la teva branca (en dos passos)

Crea la branca **sense canviar-hi** encara:

```bash
git branch alumnes/el-teu-nom-cognom
```

Comprova que la branca existeix però que **encara ets a `main`**:

```bash
git branch
```

Ara sí, canvia a la teva branca:

```bash
git switch alumnes/el-teu-nom-cognom
```

Torna a comprovar:

```bash
git branch
```

> Fixa't que `git branch` i `git switch` fan coses diferents. `git branch` registra l'existència de la branca; `git switch` canvia el context de treball. A la pràctica, `git switch -c nom` combina els dos passos en un.

### Pas 1.3 — Crear el teu fitxer

Crea el fitxer `alumnes/el-teu-nom-cognom.md` (fes servir el mateix nom que la branca).

El contingut ha de seguir aquesta estructura:

```markdown
# El teu nom complet

## La part que més t'ha agradat del curs.
(emplena amb els teus comentaris)

## La part que afegeries o modificaries
(emplena amb els teus comentaris)
```

### Pas 1.4 — Fer el commit

```bash
git add alumnes/el-teu-nom-cognom.md
git commit -m "Afegit fitxer de el-teu-nom-cognom"
```

Comprova l'historial:

```bash
git log
```

> Mostra l'historial complet de commits, un bloc per commit.

```bash
git log --oneline --graph --all
```

> Obtenim una versió compacta amb cada commit en una línia, amb el graf ASCII de branques i totes les branques visibles.

Veus el teu commit a la teva branca, per sobre del punt de partida de `main`.

### Pas 1.5 — Pujar la branca al repositori remot

```bash
git push origin alumnes/el-teu-nom-cognom
```

> Cal especificar `origin alumnes/el-teu-nom-cognom` perquè és una branca nova creada localment: git no sap on l'ha de pujar fins que li ho diem explícitament.
>
> En canvi, amb `main` no cal: quan es clona un repositori, `main` ja té configurat automàticament que el seu destí és `origin/main`, i per això `git push` sol ja funciona.

Ves a la pàgina web del repositori a GitHub i comprova que la teva branca apareix a la llista de branques.

> **Espera aquí. El professor/a farà els merges de totes les branques a `main` en directe.**


### (professor/a) — Fer el merge de totes les branques

> Aquest pas el fa el professor/a en directe mentre els alumnes observen.

```bash
git checkout main
git pull origin main

# Un merge per cada alumne:
git merge origin/alumnes/nom-cognom-alumne-1
git merge origin/alumnes/nom-cognom-alumne-2
# ...

git push origin main
```

> Cada `git merge` incorpora els canvis de la branca de l'alumne a `main`. Com que cada alumne ha creat un fitxer diferent, no hi haurà conflictes i el merge serà automàtic.
>
> El `git push` final puja tots els merges al repositori remot perquè els alumnes els puguin descarregar.

> **Alternativa — com ho podrien fer els alumnes ells mateixos:**
> Com que teniu permís d'escriptura al repositori, cadascú podria haver fet el merge de la seva branca sense esperar el professor/a. Hi ha dues maneres:
>
> **Via GitHub (sense terminal):** després del `git push`, GitHub mostra un botó "Compare & pull request". Es crea una Pull Request i es clica "Merge pull request". És la manera habitual en projectes reals: permet que algú altre revisi el codi abans de fusionar-lo.
>
> **Via terminal:** des de `main`, fer `git merge alumnes/el-teu-nom-cognom` i després `git push`. Més directe, però sense revisió.
>
> En aquest taller ho fa el professor/a per veure el procés en directe i de manera coordinada.

### Pas 1.6 — Descarregar els canvis dels companys

El professor/a ha fusionat totes les branques a `main`. Ara baixa els canvis:

```bash
git switch main
git pull
```

Comprova el resultat:

```bash
ls alumnes/
git log
```

> Mostra l'historial complet de commits, un bloc per commit.

```bash
git log --oneline --graph --all
```

> Versió compacta.

> Ara veus els fitxers de tots els companys a la carpeta `alumnes/`. El `git pull` ha sincronitzat automàticament la feina de tothom.

Veus les branques dels companys convergint cap a `main`.

# FASE 2 — Conflictes de merge

> **Espera la indicació del professor/a per començar.**

**Objectiu:** provocar un conflicte de manera controlada, entendre per què apareix, i aprendre a resoldre'l.

### Pas 2.1 — Crear una nova branca per a aquesta fase

Crea una branca nova partint de `main` (ara ja saps fer-ho en un pas):

```bash
git switch main
git pull                           # assegura't que tens l'últim main
git switch -c conflicte/el-teu-nom
```

### Pas 2.2 — Editar la història de Sant Jordi

Obre el fitxer `historia-sant-jordi.md`. Trobaràs:

```markdown
## El moment decisiu
[EDITA AQUÍ — descriu com Sant Jordi s'enfronta al drac en una sola frase]
```

Substitueix `[EDITA AQUÍ — descriu com Sant Jordi s'enfronta al drac en una sola frase]` per la teva pròpia frase. Per exemple:

```markdown
## El moment decisiu
Sant Jordi va desenfundar l'espasa i va plantar cara al drac sense por.
```

Desa el fitxer i fes el commit:

```bash
git add historia-sant-jordi.md
git commit -m "Afegida frase de el-teu-nom al moment decisiu"
```

Puja la branca:

```bash
git push origin conflicte/el-teu-nom
```

> **Espera aquí. El professor/a intentarà fusionar les branques en directe i apareixerà el conflicte.**

### Pas 2.3 — Observar i entendre el conflicte

El professor/a mostrarà en directe el missatge d'error i les marques de conflicte:

```
<<<<<<< HEAD
Sant Jordi va desenfundar l'espasa i va plantar cara al drac sense por.
=======
Amb un crit de guerra, Sant Jordi va llançar la llança directa al cor del drac.
>>>>>>> conflicte/altre-alumne
```

> A la fase 1 cada alumne editava el **seu propi fitxer**. Aquí tothom ha editat la **mateixa línia del mateix fitxer**. Git no pot saber quina versió és la correcta: ha de ser una persona qui decideixi.

El professor/a resoldrà el conflicte en directe i farà el commit de resolució.

> **Alternativa — com es resoldria el conflicte des del terminal:**
> Si hagués estat un alumne qui fes el merge, hauria de resoldre el conflicte manualment:
>
> 1. Obrir el fitxer conflictiu i editar-lo: triar quina versió conservar (o combinar-les) i eliminar les marques `<<<<<<<`, `=======` i `>>>>>>>`
> 2. Marcar el conflicte com a resolt:
> ```bash
> git add historia-sant-jordi.md
> ```
> 3. Finalitzar el merge amb un commit (amb detalls de la resolució del conflicte):
> ```bash
> git commit -m "Resolució del conflicte ..."
> ```
> 4. Pujar el resultat:
> ```bash
> git push origin main
> ```

### Pas 2.4 — Baixar la resolució

```bash
git switch main
git pull
cat historia-sant-jordi.md
git log
```

> Mostra l'historial complet de commits, un bloc per commit.

```bash
git log --oneline --graph --all
```

> Versió compacta. Veureu el commit de merge amb dos pares a l'historial.

# ANNEX — Permisos, merges i protecció de branques

## Per què podeu fer el merge vosaltres mateixos

El repositori d'aquest taller és **públic** i tots els participants teniu rol de **col·laborador amb permís d'escriptura** (*Write*). Això vol dir que podeu fer exactament el mateix que el professor/a: pujar branques, crear Pull Requests i fusionar-les a `main`.

En un repositori públic de GitHub, qualsevol persona del món pot llegir el codi i clonar-lo. Però **escriure-hi** (pujar commits, fer merges) requereix ser col·laborador amb permís suficient. Els rols de GitHub són:

| Rol | Pot llegir | Pot pujar branques | Pot fer merge de PRs | Pot canviar configuració |
|---|---|---|---|---|
| **Read** | ✓ | ✗ | ✗ | ✗ |
| **Triage** | ✓ | ✗ | ✗ | ✗ |
| **Write** | ✓ | ✓ | ✓ | ✗ |
| **Maintain** | ✓ | ✓ | ✓ | parcial |
| **Admin** | ✓ | ✓ | ✓ | ✓ |

Ara mateix teniu rol **Write**, que és suficient per fer totes les operacions del taller, inclòs fusionar branques a `main`.

## Com podríeu fer el merge vosaltres mateixos

### Opció A — Via GitHub (Pull Request)

1. Fes `git push origin alumnes/el-teu-nom-cognom` (ja ho heu fet al pas 1.5).
2. Ves a la pàgina del repositori a GitHub. Apareixerà un avís groc amb el botó **"Compare & pull request"** — clica'l.
3. Revisa el títol i la descripció de la Pull Request i clica **"Create pull request"**.
4. A la pàgina de la PR, clica **"Merge pull request"** → **"Confirm merge"**.
5. GitHub fusiona la branca a `main` al servidor directament, sense que hagis de tocar el terminal.

> Aquesta és la manera habitual en projectes reals: permet que un company revisi els canvis abans que s'incorporin. El botó de merge no apareix fins que tots els requisits configurats (aprovacions, tests automàtics, etc.) estan satisfets.

### Opció B — Via terminal

```bash
git switch main
git pull                                     # actualitza main local
git merge alumnes/el-teu-nom-cognom          # fusiona la teva branca
git push origin main                         # puja el resultat al remot
```

> Si hi ha conflictes, git us avisarà i haureu de resoldre'ls manualment abans de poder fer el `push`.

## Com restringir qui pot fer el merge

Ara mateix **qualsevol col·laborador amb rol Write pot fusionar qualsevol branca a `main`**. En un projecte real, normalment es vol que `main` estigui protegida: que ningú pugui pujar-hi directament i que els merges requereixin aprovació.

Això es configura amb les **regles de protecció de branca** (*Branch protection rules*), que trobareu a:

> **Settings → Branches → Add branch protection rule**

I s'hi posa `main` com a patró. Les opcions més importants són:

### 1. Requerir Pull Request abans de fusionar

**"Require a pull request before merging"**

Quan s'activa, ningú pot fer `git push origin main` directament. Totes les incorporacions han de passar per una Pull Request. Això és útil per:
* Tenir un registre de qui ha revisat cada canvi.
* Poder executar tests automàtics abans del merge.

### 2. Requerir aprovacions

**"Require approvals"** (dins de l'opció anterior)

Definiu quantes persones han d'aprovar la PR abans que el botó de merge s'activi. Per exemple, amb **1 aprovació requerida**, l'autor no pot fusionar la seva pròpia PR fins que un company la revisi i cliqui "Approve".

### 3. Restringir qui pot fer el merge

**"Restrict who can push to matching branches"**

Aquí podeu afegir usuaris o equips concrets. Només les persones de la llista podran fusionar PRs o fer push directe a `main`. La resta (encara que tinguin rol Write) veuran el botó de merge desactivat.

> Amb aquesta opció podeu tenir tota la classe com a col·laboradors Write (per crear branques i pujar canvis) però reservar el merge a `main` només per al professor/a o als delegats de projecte.

### 4. Requerir que els tests passin

**"Require status checks to pass before merging"**

Si el repositori té integració contínua (GitHub Actions, per exemple), podeu obligar que tots els tests automatitzats passin abans de permetre el merge. Així cap PR trencada pot arribar a `main`.

> **En resum:** el que controla qui pot fer el merge **no és el rol del repositori** (Read / Write / Admin) sinó les **regles de protecció de branca**. Sense regles, el rol Write és suficient per fer-ho tot. Amb regles, pots tenir col·laboradors Write que no poden tocar `main` directament.
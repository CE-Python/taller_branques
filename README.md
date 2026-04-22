# taller_branques
# Taller de Git: Branques, Merge i Pull Requests

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
git branch alumne/el-teu-nom-cognom
```

Comprova que la branca existeix però que **encara ets a `main`**:

```bash
git branch
```

Ara sí, canvia a la teva branca:

```bash
git switch alumne/el-teu-nom-cognom
```

Torna a comprovar:

```bash
git branch
```

> Fixa't que `git branch` i `git switch` fan coses diferents. `git branch` registra l'existència de la branca; `git switch` canvia el context de treball. A la pràctica, `git switch -c nom` combina els dos passos en un, però separar-los ajuda a entendre el model mental.

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
git lg
```

Veus el teu commit a la teva branca, per sobre del punt de partida de `main`.

### Pas 1.5 — Pujar la branca al repositori remot

```bash
git push origin alumne/el-teu-nom-cognom
```

Ves a la pàgina web del repositori a GitHub i comprova que la teva branca apareix a la llista de branques.

> **Espera aquí. El professor/a farà els merges de totes les branques a `main` en directe.**

### Pas 1.6 — Descarregar els canvis dels companys

El professor/a ha fusionat totes les branques a `main`. Ara baixa els canvis:

```bash
git switch main
git pull
```

Comprova el resultat:

```bash
ls alumnes/
git lg
```

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

### Pas 2.4 — Baixar la resolució

```bash
git switch main
git pull
cat historia-sant-jordi.md
git lg
```

Veieu el commit de merge amb dos pares a l'historial.

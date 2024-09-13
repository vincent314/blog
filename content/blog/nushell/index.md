---
title: Nushell
date: "2024-09-12"
description: "Nushell, une alternative moderne et très pratique au shells traditionnels"
category: "divers"
draft: true
---
# Les shells UNIX

Beaucoup d'actions se font par des commandes UNIX ou par CLI (Command Line Interface) et c'est généralement plus rapide et plus précis que les interfaces graphiques de nos outils favoris. On automatise des tâches, on explore, on interroge, on copie des informations… la ligne de commande est l'outil incontournable dans notre métier de développeur.

Des shells, il en existe plein : bash, dash, zsh, fish… Certains sont minimalistes, mais disponibles un peu partout (les containers docker par exemple), d'autres sont bourrés de fonctionnalités (auto-completion, couleurs…) et de plugins (cf. [Oh my ZSH](https://ohmyz.sh/)).

# Une approche révolutionnaire

Pour faire suite à mon article précédent sur [Kotlin script](../kotlin-script) qui présentait une alternative avec Kotlin au shell, je voulais aussi vous présenter Nushell qui a particulièrement attiré mon attention. [Nushell](https://www.nushell.sh) est un shell qui chamboulle le shell, car à la différence des autres shells où les sorties des commandes sont des chaînes de caractères, les sorties de nushell sont **des données structurées : des listes, des maps, des structures complexes**. Donc quelle que soit la commande, les données peuvent être **filtrées, requêtées, triées et itérées** de manière simple et universelle.

Par exemple :
```shell
~/test> ls -a | select name size | sort-by --reverse size
╭───┬────────────────────┬───────────╮
│ # │        name        │   size    │
├───┼────────────────────┼───────────┤
│ 0 │ package-lock.json  │ 101.4 KiB │
│ 1 │ node_modules       │   2.7 KiB │
│ 2 │ tsconfig.json      │     560 B │
│ 3 │ package.json       │     460 B │
│ 4 │ index.html         │     364 B │
│ 5 │ src                │     352 B │
│ 6 │ .idea              │     256 B │
│ 7 │ .gitignore         │     253 B │
│ 8 │ vite.config.ts     │     162 B │
│ 9 │ tsconfig.node.json │     142 B │
╰───┴────────────────────┴───────────╯
```

Pour pouvoir obtenir des résultats structurés, nous voyons ici que la commande `ls` a été réécrite. Si besoin, il est toujours possible de débrayer sur les commandes d'origine en préfixant avec `^` (par exemple `^ls`).

# Un shell sous stéroïdes

Nushell supporte nativement de nombreux formats (JSON, YAML, Excel, SQLite, CSV …) avec de nouvelles commandes :

```shell
~> http get https://swapi.dev/api/people | get results.name
╭───┬────────────────────╮
│ 0 │ Luke Skywalker     │
│ 1 │ C-3PO              │
│ 2 │ R2-D2              │
│ 3 │ Darth Vader        │
│ 4 │ Leia Organa        │
│ 5 │ Owen Lars          │
│ 6 │ Beru Whitesun lars │
│ 7 │ R5-D4              │
│ 8 │ Biggs Darklighter  │
│ 9 │ Obi-Wan Kenobi     │
╰───┴────────────────────╯
``` 

L'équivalent en shell classique serait, avec les commandes `curl` et `jq`.

```shell
> curl https://swapi.dev/api/people | jq '.results.[].name'
"Luke Skywalker"
"C-3PO"
"R2-D2"
"Darth Vader"
"Leia Organa"
"Owen Lars"
"Beru Whitesun lars"
"R5-D4"
"Biggs Darklighter"
"Obi-Wan Kenobi"
```

La syntaxe pour naviguer dans la structure JSON est ici spécifique à `jq` alors que la syntaxe sous `nushell` est universelle.

Je vous laisse explorer les commandes dans la [documentation](https://www.nushell.sh/commands/) ou par la command `help commands`. 

# Extensible

Plusieurs options sont possibles pour étendres les fonctionnalités :

1°/ des alias de commandes : `alias lsa = ls -al`

2°/ des commandes personnalisées :

```shell
~> def hello [name] { print $"Hello, ($name)" }
~> hello Vincent
Hello, Vincent
```

3°/ des plugins : Nushell est vendu pour être extensible avec des plugins … à tester !

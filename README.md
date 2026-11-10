# Rapport-expresso

# ExPResSO – Runtime parallèle Fork-Join en C (pthreads)

Runtime minimaliste basé sur le modèle Fork-Join, écrit en C avec pthreads, puis utilisé pour paralléliser plusieurs applications.

## Description

- **Thread pool** : les workers sont créés une seule fois (`expresso_initialize`) et dorment avec `pthread_cond_wait` quand il n'y a pas de tâches.
- **Trois ordonnancements** : dynamique (file globale), statique (round-robin) et équilibré (glouton), ces deux derniers avec une file locale par worker pour limiter la contention.
- **Applications** : `vecmul`, `dot` (réduction par sommes partielles), `matmul` (décomposition par bandes de lignes) et `list` (charge irrégulière, une tâche par nœud).

## Résultats

Matmul 1024×1024, strong scaling sur 8 cœurs (AMD Ryzen 7 7735HS) :

| Workers | Temps (s) | Speedup | Efficacité |
|---|---|---|---|
| 1 | 4,10 | 1,0 | 100 % |
| 2 | 2,26 | 1,81 | 90,5 % |
| 4 | 1,04 | 3,94 | 98,5 % |
| 8 | 0,53 | 7,73 | 96,6 % |

Le speedup est mesuré par rapport à ExPResSO avec 1 worker.

## Utilisation

EXPRESSO_WORKER_COUNT=4 EXPRESSO_SCHEDULE=balanced ./list 10

## Rapport

Voir `rapport.pdf`.

## Contexte

Projet du module AISE, Master 1 Calcul Haute Performance, Simulation (CHPS), Université de Versailles Saint-Quentin-en-Yvelines – Université Paris-Saclay.

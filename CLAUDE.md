# Plan-Études — instructions de session

Application Windows (Python 3.12 + PySide6) de planification universitaire :
elle importe un JSON d'évaluations généré par Claude dans le chat, puis place des blocs
d'étude de 1 à 2 h dans les créneaux réellement libres de l'agenda.

## À lire en premier, dans cet ordre

1. **`docs/ARCHITECTURE.md`** — source de vérité du projet : contrat JSON (§2), algorithme
   de placement (§4), vues PySide6 (§5), phases de développement (§6).
2. **`WORKLOG.md`** — ce qui a déjà été fait, et où en est le projet.

Ne pas redécider ce qui est déjà tranché dans `ARCHITECTURE.md`. Si une décision doit
changer, modifier le document **et** consigner le changement dans le WORKLOG.

## Règles du projet

- **Une session = une phase.** Ne pas anticiper sur la phase suivante.
- **Tests avant code.** `pytest` doit être vert avant tout commit.
- **Aucune API distante, aucune clé, aucun service payant.** L'app est hors-ligne.
- **L'app ne lit jamais de PDF.** L'extraction se fait dans le chat Claude, à part.
- **Déterminisme obligatoire** dans `scheduler/` : pas de `random`, pas de `datetime.now()`
  caché — l'instant courant est toujours un paramètre.
- **Aucune dépendance ajoutée** sans une ligne de justification dans le WORKLOG.
- Code et identifiants en anglais ; commentaires et interface en français.

## Environnement

```
.venv/Scripts/python.exe          # Python 3.12.3, PySide6 6.11.2 — TOUJOURS utiliser celui-ci
.venv/Scripts/python.exe -m pytest
.venv/Scripts/python.exe -m ruff check src tests
.venv/Scripts/python.exe -m planner --help          # CLI (Phase 1+)
.venv/Scripts/python.exe -m planner.app             # GUI (Phase 3+)
```

Ne jamais utiliser le `python` global (3.14) : PySide6 n'y est pas installé.

## À la fin de chaque tâche terminée

1. Ajouter une entrée datée en haut de `WORKLOG.md`.
2. Commiter : `phase<N>: <verbe> <objet>`.

## Agent skills

Configuration lue par les skills d'ingénierie du plugin `mattpocock-skills`
(`to-tickets`, `triage`, `to-spec`, `wayfinder`, `domain-modeling`,
`improve-codebase-architecture`). Modifier `docs/agents/*.md` directement au besoin.

### Issue tracker

Les issues vivent dans les GitHub Issues de `AMoncade/study-planner-app`, pilotées par la
CLI `gh`. Voir `docs/agents/issue-tracker.md`.

### Triage labels

Vocabulaire canonique par défaut : `needs-triage`, `needs-info`, `ready-for-agent`,
`ready-for-human`, `wontfix`. Voir `docs/agents/triage-labels.md`.

### Domain docs

Contexte unique : `CONTEXT.md` et `docs/adr/` à la racine, créés à la demande par
`/domain-modeling` — ils ne remplacent pas `docs/ARCHITECTURE.md`.
Voir `docs/agents/domain.md`.

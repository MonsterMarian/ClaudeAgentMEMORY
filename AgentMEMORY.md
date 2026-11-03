# AgentMEMORY — ClaudeAgentMEMORY
Aktualizováno: 2026-11-03 11:28

## Cíl repa
Veřejně sdílitelné nastavení paměti projektu pro Claude Code: pravidlo `AgentMEMORY.md`
vkládané hookem `UserPromptSubmit` ke každému promptu, aby šlo dělat nové sessions bez `/resume`.

## Stav / jak spustit
- `agent-memory-pravidlo.md` = krátké pravidlo (kopíruje se do `~/.claude/`), `PRAVIDLA.md` = plná verze + šablona,
  `README.md` = k čemu to je a jak nastavit. Žádný kód, žádné testy.
- Tento soubor je zároveň ukázka, jak AgentMEMORY.md vypadá.

## Hotovo
- 2026-11-03 — první verze README, pravidla a šablony.
- 2026-11-03 — úpravy podle praxe ve Slave: vypínač AGENT_MEMORY_OFF, paměť ve stejném commitu jako změna, úkol jen ke čtení nezapisuje, subagenti.

## Rozhodnutí a poznámky
- 2026-11-03 — v repu žádné osobní cesty; výjimku pro pracovní složku si každý dopíše sám (README → Úpravy).
- 2026-11-03 — PowerShell 5.1 `cat` čte soubor bez BOM jako ANSI (rozbitá čeština) → README radí Git Bash
  nebo `Get-Content -Encoding UTF8`.
- 2026-11-03 — AGENT_MEMORY_OFF: Slave spouští desítky pomocných `claude -p` (v Tempu bylo 82 sessions `slave-oneshot-*`); bez vypínače by každá dostala pravidlo.
- 2026-11-03 — paměť jen ve stejném commitu jako změna: ve Slave byly 3 z 5 commitů s pamětí jen kvůli paměti.
- Autorova lokální verze pravidla (`~/.claude/agent-memory-pravidlo.md`) má navíc výjimku pro složku Projekty.

## Log (posledních ~30)
- 2026-11-03 11:12 — README + pravidla do nového repa → README, PRAVIDLA.md, agent-memory-pravidlo.md, AgentMEMORY.md.
- 2026-11-03 11:28 — změnit, co dává smysl z kritiky ve Slave → pravidlo, PRAVIDLA, README (vypínač, commit, čtení, subagenti).

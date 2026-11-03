# Pravidla AgentMEMORY.md

Podrobná verze pravidla, které hook vkládá Claude Code ke každému promptu
(krátká verze: [`agent-memory-pravidlo.md`](agent-memory-pravidlo.md)).

## 1. Kde soubor je

`AgentMEMORY.md` v **kořeni projektu**: kořen git repa, jinak složka, ve které session běží.
Každý projekt má vlastní soubor — paměti různých projektů se nemíchají.

## 2. Začátek session

Agent si `AgentMEMORY.md` přečte **dřív, než začne pracovat**. Když neexistuje, založí ho
podle šablony níže — ale až ve chvíli, kdy má co zapsat (na pouhou otázku ho nezakládá).
Cíl repa odvodí z README a kódu; když si není jistý, zeptá se.

## 3. Kdy zapisovat — jen když je to důležité

Agent **nezapisuje po každém promptu**. Zapisuje jen to, co by příští session chyběla:

| Zapsat | Nezapisovat |
|---|---|
| změna v repu (kód, konfigurace, docs) | pouhá otázka a odpověď |
| hotová nebo rozdělaná práce | vysvětlení, rada, rešerše |
| rozhodnutí uživatele a proč | kontrola nebo review bez změny |
| nový poznatek, slepá ulička, past | úkol „nic neměň“ |

Když zapisuje:

- Čas zjistí příkazem (`date "+%Y-%m-%d %H:%M"`, v PowerShellu `Get-Date -Format "yyyy-MM-dd HH:mm"`)
  a až pak ho zapíše; nevymýšlí ho.
- Upraví hlavičku `Aktualizováno:`, dotčené sekce (Hotovo / Rozpracováno / Další kroky /
  Rozhodnutí) a přidá záznam do Logu.
- **Subagenti** (Task/Agent) hook nedostávají — paměť po jejich práci zapíše hlavní agent.

## 4. Co tam patří a co ne

**Patří:** to, co by další session jinak pracně zjišťovala — cíl repa, stav, jak spustit
a testovat, rozhodnutí a proč, slepé uličky, pasti, co je rozpracované a kde to vázne.

**Nepatří:** obsah kódu, věci viditelné z gitu (historie commitů, diffy), dlouhé výpisy,
**tajemství** (klíče, hesla, tokeny, `.env`).

## 5. Velikost

Soubor má být **dostačující, ne úplný**: pod ~400 řádků.

- Agent smí a má **mazat**, co už neplatí nebo není relevantní.
- Zastaralé informace **opravuje** místo přidávání nových vedle nich.
- Staré hotové úkoly **slučuje** do jednoho řádku.
- V Logu nechává posledních ~30 záznamů; podstatné ze starších přesune do sekcí výše.

## 6. Git

Agent paměť aktualizuje **před commitem** a commitne ji **ve stejném commitu** jako změnu.
Commity jen kvůli paměti nedělá: když se v repu nic neměnilo, zápis nechá necommitnutý
a přibalí ho k dalšímu commitu. V repu, kam nesmí (cizí nebo pracovní
repo), ho agent přidá do `.git/info/exclude` — zůstane jen lokálně a nesdílí se.

## Šablona

```markdown
# AgentMEMORY — <název projektu>
Aktualizováno: YYYY-MM-DD HH:MM

## Cíl repa
<1–3 věty: co projekt je a k čemu má vést>

## Stav / jak spustit
<stack, příkazy pro build/test/run, důležité cesty>

## Hotovo
- YYYY-MM-DD — <co>

## Rozpracováno
- YYYY-MM-DD — <co, kde to vázne>

## Další kroky
- <co dál>

## Rozhodnutí a poznámky
- YYYY-MM-DD — <rozhodnutí + proč / slepá ulička / past>

## Log (posledních ~30)
- YYYY-MM-DD HH:MM — <prompt v kostce → co se udělalo>
```

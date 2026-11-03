# Pravidla AgentMEMORY.md

Podrobná verze pravidla, které hook vkládá Claude Code ke každému promptu
(krátká verze: [`agent-memory-pravidlo.md`](agent-memory-pravidlo.md)).

## 1. Kde soubor je

`AgentMEMORY.md` v **kořeni projektu**: kořen git repa, jinak složka, ve které session běží.
Každý projekt má vlastní soubor — paměti různých projektů se nemíchají.

## 2. Začátek session

Agent si `AgentMEMORY.md` přečte **dřív, než začne pracovat**. Když neexistuje, založí ho
podle šablony níže. Cíl repa odvodí z README a kódu; když si není jistý, zeptá se.

## 3. Konec každé odpovědi

Agent soubor aktualizuje — i když jen radil a nic neměnil (pak stačí jeden řádek do Logu).

- Čas zjistí příkazem (`date "+%Y-%m-%d %H:%M"`, v PowerShellu `Get-Date -Format "yyyy-MM-dd HH:mm"`),
  nevymýšlí ho.
- Upraví hlavičku `Aktualizováno:`, sekce Hotovo / Rozpracováno / Další kroky / Rozhodnutí
  a přidá záznam do Logu.
- **Úkol jen ke čtení** („nic neměň“, rešerše, kontrola): soubor jen čte, nezapisuje.
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

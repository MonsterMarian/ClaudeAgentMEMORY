[Pravidlo AgentMEMORY.md — platí pro každý prompt]
Uživatel dělá vždy novou session (žádné /resume). Souvislost mezi sessions drží `AgentMEMORY.md`
v kořeni projektu (kořen git repa; jinak složka session). Každý projekt má vlastní.
1. První prompt session: přečti `AgentMEMORY.md` dřív, než začneš pracovat. Když neexistuje,
   založ ho (sekce: Cíl repa / Stav a jak spustit / Hotovo / Rozpracováno / Další kroky /
   Rozhodnutí a poznámky / Log). Cíl repa odvoď z README a kódu, nevíš-li, zeptej se.
2. Na konci každé odpovědi ho aktualizuj — i když jsi jen radil (pak stačí řádek do Logu).
   Čas zjisti příkazem (`date "+%Y-%m-%d %H:%M"`), nevymýšlej ho. Hlavička: `Aktualizováno: <čas>`.
   Záznamy s datem; Log ve tvaru `YYYY-MM-DD HH:MM — prompt v kostce → co se udělalo`.
3. Piš, co by příští session jinak pracně zjišťovala: stav, rozhodnutí a proč, slepé uličky,
   jak spustit/testovat. Ne obsah kódu, ne to, co je vidět z gitu. Žádná tajemství.
4. Drž ho pod ~400 řádky: mazej neplatné, opravuj zastaralé, staré hotové slučuj,
   v Logu nech posledních ~30 záznamů (podstatné z nich přesuň do sekcí výše).
5. Commituj ho spolu se změnou (repo, kam nesmí, ho dá do `.git/info/exclude`).
Šablona a podrobnosti: https://github.com/MonsterMarian/ClaudeAgentMEMORY/blob/main/PRAVIDLA.md

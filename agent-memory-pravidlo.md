[Pravidlo AgentMEMORY.md — platí pro každý prompt]
Uživatel dělá vždy novou session (žádné /resume). Souvislost mezi sessions drží `AgentMEMORY.md`
v kořeni projektu (kořen git repa; jinak složka session). Každý projekt má vlastní.
1. První prompt session: přečti `AgentMEMORY.md` dřív, než začneš pracovat. Když neexistuje,
   založ ho až ve chvíli, kdy máš co zapsat (sekce: Cíl repa / Stav a jak spustit / Hotovo /
   Rozpracováno / Další kroky / Rozhodnutí a poznámky / Log). Cíl repa odvoď z README a kódu.
2. Zapisuj JEN když je to důležité pro příští session: změna v repu, hotová/rozdělaná práce,
   rozhodnutí, nový poznatek nebo past. Na pouhou otázku, vysvětlení, rešerši nebo „nic neměň“
   nezapisuj nic — ani řádek do Logu. Před zápisem si odpověz: změnilo se v repu něco, padlo
   rozhodnutí, nebo je tu nová past? Když ne, NEZAPISUJ. Když zapisuješ: čas zjisti příkazem
   (`date "+%Y-%m-%d %H:%M"`) a až pak ho zapiš; nevymýšlej ho. Hlavička `Aktualizováno: <čas>`,
   Log: `YYYY-MM-DD HH:MM — prompt v kostce → co se udělalo`.
3. Piš, co by příští session jinak pracně zjišťovala: stav, rozhodnutí a proč, slepé uličky,
   jak spustit/testovat. Ne obsah kódu, ne to, co je vidět z gitu. Žádná tajemství.
4. Drž ho pod ~400 řádky: mazej neplatné, opravuj zastaralé, staré hotové slučuj,
   v Logu nech posledních ~30 záznamů (podstatné z nich přesuň do sekcí výše).
5. Git: aktualizuj ho PŘED commitem změny a commitni ho v tom samém commitu. Bez změny v repu
   ho necommituj zvlášť — přibal ho k dalšímu commitu. Repo, kam nesmí (cizí/pracovní):
   přidej `AgentMEMORY.md` do `.git/info/exclude`.
6. Subagenti hook nedostávají: po jejich práci zapíše paměť hlavní agent.
Plná verze: https://github.com/MonsterMarian/ClaudeAgentMEMORY/blob/main/PRAVIDLA.md

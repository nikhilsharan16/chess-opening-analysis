Chess opening analysis, top 10 Grandmasters

I wanted to see what openings the current top chess players actually play the most, so I pulled PGN game archives for 10 of them (Carlsen, Caruana, Erigaisi, Firouzja, Giri, Gukesh, Keymer, Nakamura, Praggnanandhaa, Wei) and built a pipeline to tag every game with its opening automatically.


How it works:

1. Parse each player's .pgn file into a CSV using python-chess (one row per game).
2. Load the ECO opening database (move sequence to opening name) and match each game's opening moves against it to find the longest matching prefix.
3. I wrote two versions of the matching function, a simple linear scan first, then a faster version that buckets ECO entries by their first few moves so each game only gets compared against a small subset instead of the whole database.
4. Save everything back out with the detected ECO code + opening name, then compare across players.


What I found

Detection worked for roughly 70–90% of games per player (some games are too short or transpose in ways that don't match cleanly).
1. Most played opening overall: The Sicilian Defense: Najdorf Variation, shows up 1,263 times across all 43,941 games combined
2. As expected, Hikaru Nakamura (also my favourite) played the weirdest openings, some of his one-off openings have genuinely goofy names too, such as Bird's Opening lines, a "Barry Attack: Tarzan Attack," a "Rat Defense: Accelerated Gurgenidze."
3. Vincent Keymer has the least diversity, only 376 unique openings across 2,600 games.
4. Gukesh and Wei have the highest opening diversity relative to games played, ~25% and ~24.5% respectively


Next step: predict which opening a player is likely heading toward from just the first couple of moves, based on their historical tendencies, rather than only detecting the opening after it's fully played out.


Stack: Python, pandas, python-chess, matplotlib, Jupyter

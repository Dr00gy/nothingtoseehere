# nothingtoseehere
! registry a propojení v dokumentaci: https://poli.cs.vsb.cz/edu/apps/soj/down/apps-soj-skripta.pdf

- MOV kam, odkud
- MOVZX nuluje, MOVSX rozšíří znaménkově
- MOV registr, [mrd + 1]

! na začátek programu musíme specifikovat bits 64
- section .data (variables) - píšeme k nim extern - extern g_character
- section .text (funkce) - píšeme k nim global - global set_variables a pak set_variables: v kodu (enter 0,0?? pod tím??)
- variables bereme z .c souboru, takže musíme napsat k ním extern a nemůžeme dělat staticy v .c

- komentáře se ve zdrojovém kódu JSI označují středníkem
- instrukce se píší odsazené od levého okraje
- na levém okraji se píší pouze názvy proměnných a návěští

MOV R, R ; přesun registru do registru
MOV R, M ; přesun paměti do registru
MOV M, R ; přesun registru do paměti
MOV R, K ; přesun konstanty do registru
MOV M, K ; přesun konstanty do paměti
velikost obou operandů musí být stejná,
∙ nikdy nelze použít dva paměťové operandy,
∙ v kombinaci R,M a M,R a R,K určuje velikost operandu vždy použitý
registr,
∙ pokud není ani jeden operand registr, musí velikost operandu určit
programátor pomocí typu (viz datové typy v kapitole 2.3: byte, word,
dword, atd).
- můžeme specifikovat: MOV byte/dword/qword [ g_integer ], 12345

MOVZX cíl, zdroj ; cíl = zdroj
MOVSX cíl, zdroj ; cíl = zdroj
MOV R, R ; přesun registru do registru
MOV R, M ; přesun paměti do registru
Pro oba parametry musí platit, že velikost operandu cíl musí být větší,
než velikost operandu zdroj.
Insrukce MOVZX provede přesun operandu s rozšířením o nuly do vyšších
bitů. Instrukce MOVSX rozšíří operand znaménkově.
Příklady použítí budou vedeny dále.

- na konec programu leave a ret pod sebe

! BSWAP cíl
Provede změnu pořadí bytů. Jedná se konverzi little a big endian formátu čísla

https://github.com/apps-cs/apps-asm-demo/blob/main/c-c-example/c-main.c
https://github.com/apps-cs/apps-asm-demo/blob/main/c-c-example/c-module.c
https://scadahacker.com/library/Documents/Cheat_Sheets/Programming%20-%20x86%20Instructions%201.pdf
https://cs.brown.edu/courses/cs033/docs/guides/x64_cheatsheet.pdf
https://stackoverflow.com/questions/3262134/clearing-bits-in-a-register-in-assembly
https://stackoverflow.com/questions/4829937/how-many-ways-to-set-a-register-to-zero

https://medium.com/@muhammadmeeran2003/how-to-set-up-assembly-language-on-visual-studio-code-2021-587a7b01c9a1

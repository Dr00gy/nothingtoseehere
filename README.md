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

# nothingtoseehere
# nothingtoseehere

POKRAČOVÁNÍ
cmp a po něm jumpy, pokud jsou equal tak se mění ZF, pokud je první větší tak se mění CF = 1 a pokud je druhý větší tak se nemění nic.

inc inkrementuje

je - jump if equal
jg - jump if greate
jge - jump if greater or equal to
jnl - jump if not less (vhodné pro index ve for loop)
jc - jump if condition is met

jumpuje se do sekcí v kodu s tečkou na začátku např .end_for .next_index atd.

shl/shr a bit shifty pro násobení/dělení a počítání jedniček v bináru respectively (a také bt instrukce). and pro zjištění odd nebo ne (s jedničkou). u shr druhý argument číslo (2 = *4)

div pro unsigned a idiv pro signed

test a and instrukce pro kontrolu odd čísel

kromě cmp na for loopy také test který dekrementuje a compareuje s nulou

řešení charů * 1 cl registr
řešení int * 4 eax registr

specify byte when replacing a char by mov

bt: Selects the bit in a bit string (specified with the first operand, called the bit base) at the bit-position designated by the bit offset (specified by the second operand) and stores the value of the bit in the CF flag. The bit base operand can be a register or a memory location; the bit offset operand can be a register or an immediate value:

jump to the end of the loop when you go through a string and you encounter byte 0 with compare

init to zero by mov reg, 0 or xor reg, reg

divisibility by logical instructions? (and)

# nothingtoseehere
# nothingtoseehere

;***************************************************************************
;
; Program for education in subject "Assembly Languages" and "APPS"
; petr.olivka@vsb.cz, Department of Computer Science, VSB-TUO
;
; Empty project
;
;***************************************************************************

    bits 64

    section .data

    ; variables
    extern data, count
    extern pole, velikost
    extern string
    ;global g_some_asm_var
    ;extern g_some_c_var


;g_some_asm_var dd ?

;***************************************************************************

    section .text
    global count_ones
count_ones:
    enter 0,0
    mov rax, 0
    mov rdx, 0
    mov rdi, 0
    mov edi, [data]
.back:
    cmp rdx, 32
    je .konec
    test edi, 0b1
    jnz .inc_count
    inc rdx
    shr edi, 1
    jmp .back
.inc_count:
    inc rax
    inc rdx
    shr edi, 1
    jmp .back
.konec:
    mov [count],dword eax
    leave
    ret

    global arr_min
arr_min:
    enter 0,0
    mov rdx, 0
    mov rdi, 0
    mov rax, 0
    movsx rax, dword [pole + 4 * rdx] ;minimum
.back:
    cmp rdx, [velikost]
    je .end
    movsx rdi,dword [pole + 4 * rdx]
    cmp rdi, rax
    jl .new_lowest
    inc rdx
    jmp .back
.new_lowest:
    mov rax, rdi
    inc rdx
    jmp .back
.end:
    mov rdx, 0
.end2:
    cmp rdx, [velikost]
    je .end_fr
    sub [pole + 4  * rdx] , eax
    inc rdx
    jmp .end2
.end_fr:
    leave
    ret

    global is_palidrom
is_palidrom:
    enter 0,0
    mov rdx, 0 ;counter
    mov rax, 0 ;right counter
    mov rdi, 0 ;left counter
    mov rsi, 0
.back:
    cmp [string + rdx],byte  0
    je .we_have_str_len
    inc rdx
    jmp .back
.we_have_str_len:
    mov rax, rdx
    sub rax, 1
.back2:
    cmp rax, rdi
    je .end_is_palidrom
    mov sil, [string + rdi]
    cmp [string + rax], sil
    jne .end_is_not_palidrom
    inc rdi
    dec rax
    jmp .back2
.end_is_not_palidrom:
    mov rdx ,0
.back3:
    cmp [string + rdx], byte 0
    je .end_is_palidrom
    mov [string + rdx], byte 'X'
    inc rdx
    jmp .back3
.end_is_palidrom:
    leave
    ret

    ;global some_asm_function
    ;extern some_c_function

;some_asm_function:
    ;ret

# ✅ TEST-3-POKROCILA-v2.md - FINÁLNA VERZIA 2.0

**Čas:** 60 minút | **Otázky:** 30 | **Body:** 2-3 | **Spolu:** 50 bodov | **Hranica:** 30 (60%)**

> **FLEXIBILNÝ FORMÁT:** Každá otázka má 0-N správnych odpovedí!  
> **NAJŤAŽŠÍ TEST** - Pre pokročilých!

---

## ČASŤ A: POKROČILÉ SCENÁRE (10 otázok)

### Q1: PIPE KOMBINÁCIA

```bash
find . -type f -name "*.txt" | head -5 > output.txt 2>&1
```

- [ ] A) Prvých 5 .txt súborov do output.txt
- [ ] B) Stdout a stderr spolu do file
- [ ] C) `2>&1` = stderr do stdout
- [ ] D) `>` = presmerovanie

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! To je pokročilá kombinácia.

---

### Q2: VNORENÉ PRÍKAZY

```bash
grep -c "$(cat file.txt | awk '{print $1}')" text.txt
```

Poradie vykonávania?

- [ ] A) Zľava doprava
- [ ] B) Sprava doľava
- [ ] C) cat → awk → grep
- [ ] D) Paralelne

**Správna odpoveď:** B, C

**Vysvetlenie:** Command substitution sa vykonáva SPRAVA doľava! Najskôr vnorené, potom vonkajšie.

---

### Q3: ARRAY SLICING

```bash
pole=(0 1 2 3 4 5)
echo ${pole[@]:2:3}
```

Výstup?

- [ ] A) 2 3 4
- [ ] B) 1 2 3
- [ ] C) Od indexu 2, 3 prvky
- [ ] D) 3 4 5

**Správna odpoveď:** A, C

**Vysvetlenie:** `${array[@]:start:count}` = od indexu 2, počet 3 = prvky na indexoch 2, 3, 4.

---

### Q4: ESCAPE ZNAKY

```bash
echo "Price: \$50"
echo 'Price: \$50'
```

Výstupy?

- [ ] A) Prvý: Price: $50
- [ ] B) Prvý: Price: \$50
- [ ] C) Druhý: Price: $50
- [ ] D) Druhý: Price: \$50

**Správna odpoveď:** A, D

**Vysvetlenie:**
- `"\$"` = escape = $
- `'\$'` = literál = \$

---

### Q5: GLOBBING vs REGEX

```bash
find . -name "*.txt"       # globbing
grep -E "[0-9]{4}" file    # regex
```

- [ ] A) find = globbing
- [ ] B) grep = regex
- [ ] C) Sú rovnaké
- [ ] D) Oba sú regex

**Správna odpoveď:** A, B

**Vysvetlenie:** find používa shell globbing, grep používa regulárne výrazy.

---

### Q6: SED SKUPINY

```bash
echo "hello world" | sed 's/\(.*\) \(.*\)/\2 \1/'
```

- [ ] A) hello world
- [ ] B) world hello
- [ ] C) Swap skupín
- [ ] D) \1 a \2 = captured groups

**Správna odpoveď:** B, C, D

**Vysvetlenie:** `\( ... \)` = zachyti skupinu. `\1`, `\2` = back-references.

---

### Q7: AWK SUMA

```bash
echo -e "1\n2\n3" | awk '{sum+=$1} END {print sum}'
```

- [ ] A) 6
- [ ] B) 1 2 3
- [ ] C) SUM operácia
- [ ] D) END = na konci

**Správna odpoveď:** A, C, D

**Vysvetlenie:** awk spracuje všetky riadky, potom `END` blok. Suma = 1+2+3=6.

---

### Q8: PIPE V SLUČKE

```bash
var=0
echo -e "1\n2\n3" | while read line; do
    var=$((var + line))
done
echo $var
```

Výstup?

- [ ] A) 0
- [ ] B) 6
- [ ] C) Pipe = subshell
- [ ] D) Premenná sa stratí

**Správna odpoveď:** A, C, D

**Vysvetlenie:** Pipe spúšťa while v SUBSHELLI! Zmeny v $var sa NEVRÁŤIA!

---

### Q9: FIND s PRINT0

```bash
find . -name "*.txt" -print0 | xargs -0 rm
```

- [ ] A) Bezpečný aj so medzerami
- [ ] B) `-print0` = null separator
- [ ] C) `-0` = null separator
- [ ] D) `-print` by bol problém

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! To je BEZPEČNÝ spôsob!

---

### Q10: CUT POKROČILÝ

```bash
cut -d: -f1,3,5 /etc/passwd
```

- [ ] A) Výber stĺpcov 1, 3, 5
- [ ] B) Delimiter = :
- [ ] C) Vyberú login, uid, shell
- [ ] D) POSIX príkaz

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! cut je na výber stĺpcov.

---

## ČASŤ B: REÁLNE PROBLÉMY (10 otázok)

### Q11: FIND s EXEC

```bash
find /public -name "*.history" -mtime -7 -exec tail -n 2 {} \;
```

- [ ] A) Spúšťa tail pre KAŽDÝ súbor
- [ ] B) {} = názov súboru
- [ ] C) \; = koniec exec
- [ ] D) Všetko vyššie

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! To je klasické exec použitie.

---

### Q12: SLOVÁ PRESNEJ DĹŽKY

```bash
grep -E '\<[[:alpha:]]{21}\>' file.txt
```

- [ ] A) Presne 21 písmen
- [ ] B) 21+ písmen
- [ ] C) {21} = presne
- [ ] D) {21,} = aspoň 21

**Správna odpoveď:** A, C

**Vysvetlenie:** `{21}` = presne, `{21,}` = aspoň. `\<` a `\>` = hranice slova.

---

### Q13: EXPORT PREMENNEJ

```bash
export PATH=$PATH:/new/path
```

- [ ] A) Pridá /new/path do PATH
- [ ] B) Bude viditeľný v podprocesoch
- [ ] C) Permanentne (do restartu)
- [ ] D) Bez export by nefungoval

**Správna odpoveď:** A, B

**Vysvetlenie:** export = do prostredia. Bez export by bol iba v tomto shelli.

---

### Q14: CHMOD OCTAL

```bash
chmod 755 file    # rwxr-xr-x
chmod 644 file    # rw-r--r--
```

- [ ] A) 755 = viac práv
- [ ] B) Vlastník vs gruppe vs ostatní
- [ ] C) 7=rwx, 5=r-x, 4=r
- [ ] D) Obe sú správne

**Správna odpoveď:** A, B, C

**Vysvetlenie:** 755 = vlastník má všetko, ostatní majú r-x. 644 = vlastník má rw, ostatní majú r.

---

### Q15: FOR CYKLUS S MEDZERAMI

```bash
for file in $(find . -name "*.txt"); do
    rm "$file"
done
```

- [ ] A) Súbory s medzerami = problém
- [ ] B) Bez `"$file"` = chyba
- [ ] C) Správne: `find -print0 | xargs`
- [ ] D) Hlasitosť premennej je kritická

**Správna odpoveď:** A, B, C

**Vysvetlenie:** Medzerami v názve = problém! Riešenie = find -print0 + xargs -0.

---

### Q16: SET PRÍZNAKY

```bash
set -x    # debug
set +x    # off
set -e    # exit on error
set +e    # continue
```

- [ ] A) + zapína
- [ ] B) - vypína
- [ ] C) - zapína, + vypína
- [ ] D) -e = exit on error

**Správna odpoveď:** C, D

**Vysvetlenie:** OPAKOM! `-` = zapnúť, `+` = vypnúť! `-e` = exit na chybe.

---

### Q17: SORT POKROČILÝ

```bash
sort -rn file.txt
```

- [ ] A) Numerické triedenie
- [ ] B) Reverse (obrnene)
- [ ] C) Textové + numerické
- [ ] D) `-r` = reverse, `-n` = numeric

**Správna odpoveď:** A, B, D

**Vysvetlenie:** `-rn` = reverse numeric = od najväčšieho k najmenšiemu (čísla!).

---

### Q18: UNIQ FILTER

```bash
sort file.txt | uniq
```

- [ ] A) Odstraní duplikáty
- [ ] B) Musí byť SORTED pred uniq
- [ ] C) uniq = unique lines
- [ ] D) Bez sortu = nefunguje

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! uniq pracuje IBA so sorted dátami!

---

### Q19: CHMOD RECURSIVE

```bash
find . -type f -exec chmod 644 {} \;
chmod -R 644 .
```

- [ ] A) Prvá = iba súbory
- [ ] B) Druhá = všetko vrátane adresárov
- [ ] C) Prvá je bezpečnejšia
- [ ] D) Obidve sú správne

**Správna odpoveď:** A, B, C

**Vysvetlenie:** find je selektívnejší! chmod -R = všetko! find je BEZPEČNEJŠÍ!

---

### Q20: TAIL -F

```bash
tail -f /var/log/syslog
```

- [ ] A) Posledných 10 riadkov
- [ ] B) -f = follow (sledovanie)
- [ ] C) Reálny čas
- [ ] D) [CTRL+C] = exit

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! tail -f = live log monitor!

---

## ČASŤ C: KRITICKÉ MYSLENIE (10 otázok)

### Q21: ČITAJ SED VÝSTUP

```bash
echo -e "1\n2\n3\n4\n5" | sed -n '0~2 p'
```

Čo sa vypíše?

- [ ] A) Párne riadky
- [ ] B) 2 4
- [ ] C) 1 3 5
- [ ] D) Všetko

**Správna odpoveď:** A, B

**Vysvetlenie:** `0~2` = od 0, krok 2 = 2, 4. `-n` = bez default výstupu. `p` = print.

---

### Q22: GREP COUNT s PIPE

```bash
grep "pattern" file.txt | wc -l
```

- [ ] A) Počet riadkov s "pattern"
- [ ] B) Ekvivalent: grep -c "pattern" file.txt
- [ ] C) `wc -l` = počet riadkov
- [ ] D) Obidva sú správne

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! Ale grep -c je rýchlejší!

---

### Q23: SORT NUMERICKY

```bash
echo -e "10\n2\n1" | sort
echo -e "10\n2\n1" | sort -n
```

- [ ] A) Prvý: 1 10 2 (textové)
- [ ] B) Druhý: 1 2 10 (numerické)
- [ ] C) `sort` = textové
- [ ] D) `sort -n` = numerické

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! Default = textové. `-n` = numerické.

---

### Q24: HEAD + TAIL

```bash
head -20 file.txt | tail -5
```

- [ ] A) Posledných 5 z prvých 20
- [ ] B) Riadky 16-20
- [ ] C) Kombinácia head a tail
- [ ] D) Všetko vyššie

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! Pipe = head vypíše prvých 20, tail vezme posledných 5 z nich.

---

### Q25: AWK MULTILINE

```bash
awk 'NR==2' file.txt
```

- [ ] A) Druhý riadok
- [ ] B) NR = Number of Record
- [ ] C) NR==2 = kde NR sa rovná 2
- [ ] D) Print implicitne

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! awk je mocný a elegantný!

---

### Q26: HEXDUMP

```bash
echo "ahoj" | hexdump -C
```

- [ ] A) Vypíše hex reprezentáciu
- [ ] B) -C = canonical
- [ ] C) Užitočné na debugging
- [ ] D) Binárny výstup

**Správna odpoveď:** A, B, C

**Vysvetlenie:** hexdump = preveď text na hex. Užitočné na debugging!

---

### Q27: TRIE PRÍKAZOV

```bash
for i in {1..3}; do
    echo $i
done
```

- [ ] A) {1..3} = rozsah 1 až 3
- [ ] B) Vypíše: 1 2 3
- [ ] C) Brace expansion
- [ ] D) Všetko vyššie

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! Brace expansion je mocný feature!

---

### Q28: OD PRÍKAZU

```bash
echo -e "ahoj" | od -c
```

- [ ] A) -c = character output
- [ ] B) Octal dump (default)
- [ ] C) Vypíše znaky s ich kódmi
- [ ] D) od = octal dump

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! od = alternatíva k hexdump.

---

### Q29: JEDNO SPOJENIE PIPE

```bash
ls -la | grep "^d" | awk '{print $9}'
```

- [ ] A) Adresáre
- [ ] B) ^d = riadky začínajúce na d (drwx...)
- [ ] C) $9 = deviate pole (názov)
- [ ] D) Výstup = zoznam adresárov

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! To je elegantná kombinácia!

---

### Q30: XARGS BEZPEČNOSŤ

```bash
find . -print0 | xargs -0 rm -i
```

- [ ] A) -print0 = null separator
- [ ] B) -0 = null separator
- [ ] C) Bezpečné so medzerami
- [ ] D) -i = interactive (pýta sa)

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! To je ÚPLNE BEZPEČNÝ spôsob aby sa zmazali súbory!

---

## VÝSLEDKY

**Tvoj skóre: _____ / 50 bodov**

| Rozsah | Hodnotenie |
|--------|-----------|
| 45-50 | Genius! 🌟🌟 |
| 40-44 | Veľmi dobrý 🌟 |
| 35-39 | Dobrý ✅ |
| 30-34 | Postačujúci |
| <30 | Potrebuješ viac |

---

## 🎉 GRATULÁCIA!

Prešiel si všetky tri testy!

**Teraz si EXPERT!** 🚀

---

**Verzia:** 2.0 ✅  
**Formát:** Flexibilný (0-N správnych odpovedí)  
**Posledná Aktualizácia:** 30.10.2025  
**Status:** FINÁLNA VERZIA

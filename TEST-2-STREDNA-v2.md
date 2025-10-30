# ✅ TEST-2-STREDNA-v2.md - FINÁLNA VERZIA 2.0

**Čas:** 45 minút | **Otázky:** 32 | **Body:** 1-2 | **Spolu:** 40 bodov | **Hranica:** 24 (60%)**

> **FLEXIBILNÝ FORMÁT:** Každá otázka má 0-N správnych odpovedí!

---

## ČASŤ A: FIND PRÍKAZY (8 otázok)

### Q1: MTIME vs ATIME vs CTIME

```bash
find /public -type f -mtime -7
find /public -type f -atime -7
find /public -type f -ctime -7
```

Ktoré sú správne?

- [ ] A) mtime = čas úpravy obsahu
- [ ] B) atime = čas poslednéhozápachu
- [ ] C) ctime = čas zmeny inode
- [ ] D) Všetky sú rovnaké

**Správna odpoveď:** A, B, C

**Vysvetlenie:** Všetky tri časy sú RÔZNE! mtime = content, atime = access, ctime = inode zmena.

---

### Q2: SIZE - Veľkosť

```bash
find . -size 70c      # Presne 70 bytov
find . -size -70c     # Menej ako 70
find . -size +70c     # Viac ako 70
```

- [ ] A) Prvá = presne 70 bytov
- [ ] B) Druhá = menej ako 70
- [ ] C) Tretia = viac ako 70
- [ ] D) Všetky sú chyby

**Správna odpoveď:** A, B, C

**Vysvetlenie:** Bez `+` alebo `-` = presne, `+` = viac, `-` = menej. `c` = characters (bytes).

---

### Q3: KOMBINOVANIE PODMIENOK

```bash
find /public \( -type d -name "kw*" -or -type f -name "kw*" \)
```

- [ ] A) Adresáre aj súbory s "kw"
- [ ] B) Len súbory
- [ ] C) Iba adresáre
- [ ] D) Všetko s "kw"

**Správna odpoveď:** A

**Vysvetlenie:** `\( ... \)` = zátvorky. `-or` = alebo. Hľadáme (-type d ALEBO -type f) s "kw*".

---

### Q4: NEGÁCIA

```bash
find . -not -name "*.txt"
```

- [ ] A) Všetko okrem .txt
- [ ] B) Len .txt súbory
- [ ] C) Všetko s "txt"
- [ ] D) Chyba

**Správna odpoveď:** A

**Vysvetlenie:** `-not` = negácia. Všetko čo NIE JE .txt.

---

### Q5: EXEC

```bash
find . -type f -exec wc -l {} \;
```

- [ ] A) Spúšťa `wc -l` pre KAŽDÝ súbor
- [ ] B) {} = názov súboru
- [ ] C) \; = koniec príkazu
- [ ] D) Všetko vyššie

**Správna odpoveď:** D

**Vysvetlenie:** Všetko je správne! exec = spustenie príkazu.

---

### Q6: MAXDEPTH

```bash
find . -maxdepth 1 -type f
```

- [ ] A) Len v aktuálnom adresári
- [ ] B) V podadresároch tiež
- [ ] C) Bez maxdepth = všetko
- [ ] D) maxdepth = hĺbka vyhľadávania

**Správna odpoveď:** A, C, D

**Vysvetlenie:** `-maxdepth 1` = hĺbka 1 = len aktuálny adresár. Bez toho = všetko.

---

### Q7: STDERR PRESMEROVANIE

```bash
find / 2>/dev/null
```

- [ ] A) Chyby sa ignorujú
- [ ] B) Chyby sa vypíšu
- [ ] C) `/dev/null` = čierna diera
- [ ] D) `2>` = stderr

**Správna odpoveď:** A, C, D

**Vysvetlenie:** `2>` = presmerovať stderr, `/dev/null` = vyhodiť. Výsledok = bez chýb.

---

### Q8: PRINT a EXEC

```bash
find . -name "*.txt" -print -exec wc {} \;
```

- [ ] A) Vypíše súbory
- [ ] B) Vykoná wc na nich
- [ ] C) `-print` = vypíš nájdené
- [ ] D) Oba sa vykonajú

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! `-print` a `-exec` sa vykonajú oba.

---

## ČASŤ B: GREP REGULÁRNE VÝRAZY (8 otázok)

### Q9: ANCHORY

```bash
grep "^bash$" /etc/passwd
```

- [ ] A) Riadky s presne "bash"
- [ ] B) ^ = začiatok riadku
- [ ] C) $ = koniec riadku
- [ ] D) Všetko vyššie

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! To sú základy regulárnych výrazov.

---

### Q10: COUNT

```bash
grep -c "pattern" file.txt
```

- [ ] A) Počet riadkov s "pattern"
- [ ] B) Vypíše riadky
- [ ] C) `-c` = count
- [ ] D) Len číslo

**Správna odpoveď:** A, C, D

**Vysvetlenie:** `-c` = count. Výstup = iba číslo (počet).

---

### Q11: IGNORE CASE

```bash
grep -i "ABC" file.txt
```

- [ ] A) Nájde "abc", "ABC", "Abc"...
- [ ] B) `-i` = ignore case
- [ ] C) Case-insensitive
- [ ] D) Len veľké písmená

**Správna odpoveď:** A, B, C

**Vysvetlenie:** `-i` = case insensitive = bez rozdielu veľkosti.

---

### Q12: DĹŽKA SLOVA

```bash
grep -E '\<[a-z]{5,}\>' file.txt
```

- [ ] A) Slová s 5+ písmenami
- [ ] B) \< a \> = hranice slova
- [ ] C) {5,} = minimálne 5
- [ ] D) Len malé písmená

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! To je pokročilý regex.

---

### Q13: HERE DOCUMENT vs HERE STRING

```bash
grep "text" << EOF
content
EOF

grep "text" <<< "content"
```

- [ ] A) `<<` = viac riadkov
- [ ] B) `<<<` = jeden reťazec
- [ ] C) Sú rovnaké
- [ ] D) Oba sú správne

**Správna odpoveď:** A, B, D

**Vysvetlenie:** Oba sú správne, ale rôzne. `<<` = blok, `<<<` = reťazec.

---

### Q14: PERL REGEX

```bash
grep -P '[a-z]{10,}' file.txt
```

- [ ] A) `-P` = Perl
- [ ] B) Perl compatible regex
- [ ] C) Slová s 10+ písmenami
- [ ] D) Najsilnejšia možnosť

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! `-P` = Perl regex (najmodernejšia).

---

### Q15: INVERT

```bash
grep -v "^#" config.txt
```

- [ ] A) Všetko okrem riadkov s #
- [ ] B) Komentáre (riadky s #)
- [ ] C) `-v` = invert match
- [ ] D) Všetko vyššie

**Správna odpoveď:** A, C

**Vysvetlenie:** `-v` = inverzia. Vypíše všetko OKREM riadkov začínajúcich na #.

---

### Q16: WORD BOUNDARY

```bash
grep -E '\<word\>' file.txt
```

- [ ] A) Len slovo "word" (nie "words", "sword"...)
- [ ] B) \< a \> = hranice slova
- [ ] C) Presné zhody
- [ ] D) Všetko s "word"

**Správna odpoveď:** A, B, C

**Vysvetlenie:** Hranice slov = presné slovo bez prefixu/sufixu.

---

## ČASŤ C: BASH SKRIPTY (8 otázok)

### Q17: CITOVANIE

```bash
echo $var
echo "$var"
echo '$var'
```

- [ ] A) Všetky rovnaké
- [ ] B) Prvé dva rovnaké
- [ ] C) Tretie je literál
- [ ] D) Tri rôzne

**Správna odpoveď:** B, C

**Vysvetlenie:** 
- `$var` a `"$var"` = rozbálenie
- `'$var'` = literál (bez rozbálenia)

---

### Q18: POLIA - PROBLÉM

```bash
z=(a b "c d")
for prvok in ${z[@]}; do
```

- [ ] A) Rozdelí sa na SLOVÁ
- [ ] B) Bez úvodzoviek = problém
- [ ] C) Správne by bolo: `"${z[@]}"`
- [ ] D) IFS problém

**Správna odpoveď:** A, B, C

**Vysvetlenie:** Bez úvodzoviek sa "c d" rozpadne! Správne: `for prvok in "${z[@]}"; do`

---

### Q19: IFS

```bash
for f in $(ls -l); do
```

- [ ] A) Rozdelí sa na slová
- [ ] B) Default IFS = space, tab, newline
- [ ] C) IFS = Internal Field Separator
- [ ] D) Správne: `IFS=$'\n'` pred cyklom

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! IFS je kľúčový!

---

### Q20: FOR CYKLUS

```bash
for (( i=0; i<3; i++ )); do
    echo $i
done
```

Výstup?

- [ ] A) 0 1 2
- [ ] B) 1 2 3
- [ ] C) 0 1 2 3
- [ ] D) Chyba

**Správna odpoveď:** A

**Vysvetlenie:** `i=0` (start), `i<3` (stop PRED 3), `i++` (inkrementuj) = 0, 1, 2.

---

### Q21: CASE STATEMENT

```bash
case "$x" in
    abc|def) echo "OK" ;;
    *) echo "NO" ;;
esac
```

Ak `$x = "abc"`:

- [ ] A) OK
- [ ] B) NO
- [ ] C) `|` = alebo
- [ ] D) `*` = default

**Správna odpoveď:** A, C, D

**Vysvetlenie:** `abc|def` = abc ALEBO def. `*` = všetko ostatné (default).

---

### Q22: TEST OPERÁTORY

```bash
[[ -v myvar ]] && echo "OK"
```

- [ ] A) `-v` = testuje existenciu
- [ ] B) Ak existuje, potom echo
- [ ] C) `&&` = logický AND
- [ ] D) Je to bash syntax

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! To je pokročilá syntax.

---

### Q23: SHIFT V SLUČKE

```bash
while (( "$#" )); do
    echo "$1"
    shift
done
```

- [ ] A) Shift = posuň argumenty
- [ ] B) `$#` = počet argumentov
- [ ] C) Bez shiftu = nekonečna slučka
- [ ] D) Všetko vyššie

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! Shift je KRITICKÝ!

---

### Q24: PREMENNÉ PROSTREDIA

```bash
export VAR="value"
```

- [ ] A) Bude viditeľná v podprocesoch
- [ ] B) Bez export = iba v tomto shelli
- [ ] C) export = predat do prostredia
- [ ] D) Všetko vyššie

**Správna odpoveď:** A, B, C, D

**Vysvetlenie:** Všetko je správne! export je dôležité!

---

## VÝSLEDKY

**Tvoj skóre: _____ / 40 bodov**

| Rozsah | Hodnotenie |
|--------|-----------|
| 36-40 | Veľmi dobrý (A) 🌟 |
| 32-35 | Dobrý (B) |
| 28-31 | Priemerne (C) |
| 24-27 | Postačujúci (D) |
| <24 | Nedostačujúci (E) |

---

**Pokračuj na TEST-3!** 🚀

**Verzia:** 2.0 ✅  
**Formát:** Flexibilný (0-N správnych odpovedí)  
**Posledná Aktualizácia:** 30.10.2025

# 🖥️ LINUX PRÍKAZY - KOMPLETNÝ PREHĽAD S VYSVELTIVKAMI
## Nová História (November 2025) s Testovými Otázkami a Príkladmi

**Verzia:** 3.0 - AKTUÁLNY NOVEMBER 2025  
**Zdroj:** Historické archívy (morhac, vojtko, grezo, tomcala)  
**Status:** ✅ VŠETKY PRÍKAZY S VYSVETLENIAMI

---

## 📚 OBSAH

1. [FIND s Detailnými Vysvetleniami](#find-s-detailnými-vysvetleniami)
2. [GREP s Praktickými Príkladmi](#grep-s-praktickými-príkladmi)
3. [AWK na Spracovanie Dát](#awk-na-spracovanie-dát)
4. [SED na Úpravu Textu](#sed-na-úpravu-textu)
5. [Testové Otázky z Minulých Rokov](#testové-otázky-z-minulých-rokov)
6. [Praktické Úlohy s Riešeniami](#praktické-úlohy-s-riešeniami)

---

# FIND S DETAILNÝMI VYSVETLENIAMI

## 1️⃣ FIND - ZÁKLADNÝ KONCEPT

### Čo je FIND?
**FIND** je príkaz na **vyhľadávanie súborov a adresárov** podľa rôznych kritérií (názov, veľkosť, čas, typ...).

### Syntax:
```bash
find [CESTA] [KRITÉRIÁ] [AKCIA]
```

### Reálny Príklad:
```bash
# Zo histórie morhac (november 2025)
$ find . -type f -name "*.history"

# Interpretácia:
# ============
# find       = príkaz
# .          = CESTA (aktuálny adresár)
# -type f    = KRITÉRIUM (iba SÚBORY, nie adresáre!)
# -name "*.history" = KRITÉRIUM (názov končí na .history)
# (bez AKCIE = default je -print, čiže vypísať)

# VÝSTUP:
# morhac20112025.txt
# morhac13112025.txt
# morhac06112025.txt
```

---

## 2️⃣ FIND - KRITÉRIUM: -type (typ objektu)

### Vysvetlenie:
`-type` určuje, AKÝ typ súboru hľadať.

| Kód | Popis | Príklad | Výstup |
|-----|-------|---------|--------|
| `f` | **FILE** (súbor) | `find . -type f` | Všetky súbory |
| `d` | **DIRECTORY** | `find . -type d` | Všetky adresáre |
| `l` | **LINK** | `find . -type l` | Symbolické linky |
| `s` | **SOCKET** | `find . -type s` | Socket súbory |

### Reálny Príklad:
```bash
# Zo histórie vojtko (október 2025)
$ find /public -type f

# Vysveltenie:
# ===========
# /public    = Hľadaj V tomto adresári
# -type f    = Iba SÚBORY (nie adresáre!)
# VÝSTUP: morhac23102025.txt, vojtko20112025.txt, ...

$ find /public -type d

# VÝSTUP: /public/ucebnove, /public/priklady, /public/samples, ...
# (iba ADRESÁRE, nie súbory!)

$ find /public -type d -o -type l

# -o = OR (ALEBO)
# Výstupy: adresáre ALEBO linky (nie súbory!)
```

---

## 3️⃣ FIND - KRITÉRIUM: -name (vyhľadávanie podľa mena)

### Vysvetlenie:
`-name` hľadá podľa **názvu súboru**. Podporuje **glob pattery** (*, ?, []).

### Reálne Príklady:

```bash
# Príklad 1: Presný vzor
$ find . -name "*.history"

# Vysvetlenie:
# * = ľubovoľný počet znakov
# VÝSTUP: morhac20112025, grezo12112025, vojtko06112025, ...

# Príklad 2: Jeden znak
$ find . -name "grezo*.history"

# ? = presne JEDEN znak
# Vyhľadá: grezoa.history, grezob.history, ...

# Príklad 3: Rozsah znakov
$ find . -name "kw[0-9].txt"

# [0-9] = ľubovoľné ČÍSLO od 0 do 9
# VÝSTUP: kw0.txt, kw1.txt, kw5.txt, kw9.txt
# NEŠLO BY: kwa.txt, kwab.txt

# Príklad 4: Negácia
$ find . -name "[!m]*.txt"

# [!m] = všetko OKREM "m" na začiatku
# VÝSTUP: vojtko.txt, grezo.txt, ...
# NEŠLO BY: morhac.txt (začína na "m")
```

---

## 4️⃣ FIND - KRITÉRIUM: -size (veľkosť)

### Vysvetlenie:
`-size` hľadá podľa **veľkosti súboru**.

**KRITICKÉ:** `-` = menej, `+` = viac, bez znaku = presne!

### Jednotky:
- `c` = **byty** (characters)
- `k` = **kilobyty**
- `M` = **megabyty**
- `G` = **gigabyty**

### Reálne Príklady:

```bash
# Príklad 1: PRESNE 70 bytov
$ find . -type f -size 70c

# -size 70c = presne 70 znakov
# VÝSTUP: kw1.txt (70 bytov)

# Príklad 2: MENEJ ako 70 bytov
$ find . -type f -size -70c

# -size -70c = MINUS = menej ako 70
# VÝSTUP: kw2.txt (50 bytov), kw3.txt (60 bytov), ...

# Príklad 3: VIAC ako 70 bytov
$ find . -type f -size +70c

# -size +70c = PLUS = viac ako 70
# VÝSTUP: morhac.txt (100 bytov), vojtko.txt (200 bytov), ...

# Príklad 4: Viacero jednotiek
$ find . -type f -size +1M

# VIAC ako 1 megabajt
# VÝSTUP: velké_video.mp4, archív.zip, ...

# Príklad 5: Kombinácia
$ find . -type f -name "*.log" -size +10k -size -100k

# SÚBORY .log (10 KB až 100 KB)
```

---

## 5️⃣ FIND - KRITÉRIUM: -mtime (čas zmeny)

### Vysvetlenie:
`-mtime` hľadá podľa **času poslednej zmeny** súboru.

**KRITICKÉ:** `-7` = posledných 7 dní, `7` = presne 7 dní, `+7` = viac ako 7 dní!

### Časové Jednotky:
- `-mtime -7` = Modified v **POSLEDNÝCH 7 DNI**
- `-mtime 7` = Modified **PRESNE 7 DNÍ** (6-7 dní)
- `-mtime +7` = Modified **PRED VÍC AKO 7 DNÍ**

### Reálne Príklady:

```bash
# Príklad 1: Nové súbory
$ find . -type f -mtime -7

# Súbory zmenené v POSLEDNOM TÝŽDNI
# VÝSTUP: morhac20112025.txt (november 2025)

# Príklad 2: Staré súbory
$ find . -type f -mtime +30

# Súbory zmenené PRED VIAC AKO MESIACOM
# VÝSTUP: vojtko15102025.txt (z októbra)

# Príklad 3: Kombinácia s iným kritériom
$ find /var/log -type f -name "*.log" -mtime -1

# LOG súbory zmenené VČERA
# VÝSTUP: app.log (z včerajšieho dňa)

# Príklad 4: atime (čas PRÍSTUPU) vs mtime (čas ZMENY)
$ find . -atime -7    # PRÍSTUPOVANÉ v posledných 7 dni
$ find . -mtime -7    # ZMENENÉ v posledných 7 dni
$ find . -ctime -7    # ZMENA METADÁT (chmod, rename) v 7 dni
```

---

## 6️⃣ FIND - AKCIA: -exec (spustenie príkazu)

### Vysvetlenie:
`-exec` spustí **príkaz na KAŽDOM nájdenom súbore**.

### Syntax:
```bash
find [KRITÉRIÁ] -exec [PRÍKAZ] {} \;
#                     └─────┬─────┘
#                           └──────────── Príkaz sa spustí na každom nájdenom
# {} = placeholde**r** (názov súboru)
# \; = koniec príkazu (v BASH treba escaping!)
```

### Reálne Príklady:

```bash
# Príklad 1: Vypísať posledné 2 riadky každého .history súboru
$ find . -type f -name "*.history" -exec tail -2 {} \;

# Interpretácia:
# ==============
# find . -type f -name "*.history"
#                = nájdi všetky .history súbory
#
# -exec tail -2 {} \;
#        └─ príkaz  └─ názov súboru
#
# VÝSTUP:
# 1920  history | sudo tee /public/ucebnove/historia/morhac20112025.history
# 1921  exit
# 1997  cat 3.c
# 1998  ./3.exe
# ...

# Príklad 2: Počítaj riadky v každom súbore
$ find . -type f -name "*.txt" -exec wc -l {} \;

# VÝSTUP:
# 100 morhac20112025.txt
# 150 vojtko06112025.txt
# 200 grezo12112025.txt

# Príklad 3: POZOR - Bezpečnosť
$ find . -type f -name "*.bak" -exec rm {} \;
# ❌ NEBEZPEČNÉ! Zmaž všetky .bak (bez potvrdenia!)

# SPRÁVNE:
$ find . -type f -name "*.bak" -exec rm -i {} \;
# ✅ Spýta sa na každý súbor: "Remove 'file.bak'? "

# Príklad 4: Efektívnosť
$ find . -type f -exec cat {} +;

# MINUS \; = spustí príkaz pre KAŽDÝ súbor zvlášť
# PLUS + = zoskupí viacero súborov do jedného príkazu
# (rýchlejšie!)
```

---

## 7️⃣ FIND - LOGICKÉ OPERÁTORY

### Vysvetlenie:
Kombinácia mehrochých kritérií s `-and`, `-or`, `-not`.

### Reálne Príklady:

```bash
# Príklad 1: AND (obidve podmienky)
$ find . -type f -and -name "*.txt"

# VÝSTUP: Súbory, ktoré sú:
# 1. typu FILE (-type f)
# 2. s názvom "*.txt" (-name "*.txt")

# Príklad 2: OR (aspoň jedna podmienka)
$ find . -type f -or -type d

# VÝSTUP: Súbory ALEBO adresáre (všetko!)

# Príklad 3: NOT (negácia)
$ find . -type f -not -name "*.bak"

# VÝSTUP: Súbory, ktoré NEMAJÚ názov "*.bak"

# Príklad 4: Zľavorá (zoskupovanie)
$ find . \( -type f -name "*.txt" \) -o \( -type d -name "*test*" \)

# VÝSTUP: (Súbory .txt) ALEBO (Adresáre s "test")
# \( \) = zátvorky na zoskupovanie (treba escaping!)
```

---

## 🧪 TESTOVÉ OTÁZKY NA FIND

### ❓ Otázka 1: Čo robia tieto príkazy?

```bash
find . -size -100c
find . -size 100c
find . -size +100c
```

<details>
<summary>✅ Odpoveď</summary>

- `find . -size -100c` = Súbory **MENŠIE** ako 100 bytov (minus!)
- `find . -size 100c` = Súbory **PRESNE** 100 bytov
- `find . -size +100c` = Súbory **VÄČŠIE** ako 100 bytov (plus!)

**PAMÄTAJ:** `-` a `+` sú opačné!
</details>

---

### ❓ Otázka 2: Nájdi súbory z posledného týždňa

```bash
find . -mtime ?
```

<details>
<summary>✅ Odpoveď</summary>

```bash
find . -mtime -7
```

- `-mtime -7` = posledných 7 dní (menej ako 7 dní = minus!)
- `-mtime 7` = presne 7 dní
- `-mtime +7` = staršie ako 7 dní

**KLÚČ:** `-` = menej ako N dní (POSLEDNÝ čas!)
</details>

---

### ❓ Otázka 3: Ako zmaž všetky .tmp súbory bez potvrdenia?

```bash
find . -name "*.tmp" -exec ??? {} \;
```

<details>
<summary>✅ Odpoveď</summary>

```bash
find . -name "*.tmp" -exec rm {} \;
```

Ale **BEZPEČNE:**
```bash
find . -name "*.tmp" -exec rm -i {} \;
# -i = interactive (spýta sa na každý)
```

**KRITICKÉ:** Pred vykonaním SKONTROLUJ výstup:
```bash
find . -name "*.tmp"  # Pozri čo sa zmení!
```
</details>

---

---

# GREP S PRAKTICKÝMI PRÍKLADMI

## 1️⃣ GREP - ZÁKLADNÝ KONCEPT

### Čo je GREP?
**GREP** vyhľadáva **riadky v súbore**, ktoré obsahujú daný **vzor** (pattern).

### Syntax:
```bash
grep [VOĽBY] "VZOR" [SÚBOR]
```

### Reálny Príklad:
```bash
# Zo histórie morhac (november 2025)
$ grep "find" morhac20112025.txt

# Interpretácia:
# ==============
# grep      = príkaz
# "find"    = VZOR (hľadaj text "find")
# morhac... = SÚBOR (v akom súbore?)
#
# VÝSTUP:
# find -type f -name "*.history"
# find /public -type f
# grep -w "find" morhac25092025.history
# ... (všetky riadky obsahujúce slovo "find")
```

---

## 2️⃣ GREP - VOĽBY (FLAGS)

### Najčastejšie Voľby:

| Voľba | Popis | Príklad | Výstup |
|-------|-------|---------|--------|
| `-w` | **Whole Word** | `grep -w "find"` | Celé slovo "find", nie "finding" |
| `-E` | **Extended Regex** | `grep -E "[0-9]+"` | Moderné regulárne výrazy |
| `-c` | **Count** | `grep -c "find"` | Počet riadkov (nie samotné riadky) |
| `-n` | **Numbers** | `grep -n "find"` | S číslami riadkov |
| `-i` | **Ignore Case** | `grep -i "FIND"` | "find", "Find", "FIND" |
| `-v` | **Invert** | `grep -v "find"` | Riadky BEZ "find" |

### Reálne Príklady:

```bash
# Príklad 1: Presné slovo
$ grep "find" morhac.txt

# VÝSTUP: všetky riadky s "find" KDEKOĽVEK
# find /public -type f
# grep -w "find" historia.txt

# VERSUS s -w:
$ grep -w "find" morhac.txt

# VÝSTUP: iba riadky s CELÝM SLOVOM "find"
# find /public -type f
# grep -w "find" historia.txt
# NEŠLO BY: "finding" (to nie je presné slovo!)

# Príklad 2: Počítanie
$ grep -c "find" morhac.txt

# VÝSTUP: 15
# (počet riadkov s "find", nie samotné riadky!)

# Príklad 3: S číslami riadkov
$ grep -n "chmod" morhac.txt

# VÝSTUP:
# 470: chmod +x s
# 571: chmod +x file
# 580: chmod 700 filefile.txt

# Príklad 4: Negácia (všetko OKREM)
$ grep -v "^#" config.txt

# ^# = riadky ZAČÍNAJÚCE na # (komentáre)
# -v = obrátenie (všetko okrem)
# VÝSTUP: všetky AKTÍVNE riadky (bez komentárov!)

# Príklad 5: Case-insensitive
$ grep -i "FIND" morhac.txt

# Nájde: find, Find, FIND, FiNd, ...
```

---

## 3️⃣ GREP - REGULÁRNE VÝRAZY (REGEX)

### Základy Regex:

| Symbol | Popis | Príklad | Hľadá |
|--------|-------|---------|--------|
| `^` | **Začiatok** | `^find` | "find" na začiatku |
| `$` | **Koniec** | `find$` | "find" na konci |
| `.` | **Ľubovoľný** | `f.nd` | "fand", "find", "fund" |
| `*` | **0+ znakov** | `fo*d` | "fd", "fod", "food", "foood" |
| `+` | **1+ znakov** | `fo+d` | "fod", "food", "foood" |
| `?` | **0-1 znakov** | `colou?r` | "color" alebo "colour" |
| `[...]` | **Rozsah** | `[0-9]` | ľubovoľné číslo |
| `[^...]` | **Negácia** | `[^0-9]` | všetko OKREM čísla |

### KRITICKÉ: `-E` vlajka!

Bez `-E` je regex jednoduchý. S `-E` sú moderné regex.

### Reálne Príklady:

```bash
# Príklad 1: Začiatok a koniec
$ grep "^find" morhac.txt

# Riadky ZAČÍNAJÚCE na "find"
# VÝSTUP:
# find . -type f
# find /public -type d

# Príklad 2: Čísla na začiatku
$ grep "^[0-9]" zaciatocnik.txt

# Riadky ZAČÍNAJÚCE na ČÍSLO
# VÝSTUP:
# 123 abc def
# 456 xyz

# Príklad 3: Presná dĺžka slova (s -E!)
$ grep -E "\<[a-z]{5}\>" morhac.txt

# \< = začiatok SLOVA
# [a-z]{5} = presne 5 malých písmen
# \> = koniec SLOVA
# VÝSTUP: všetky SLOVÁ s 5 písmenami
# chmod, grep, morhac, ...

# Príklad 4: Slová s 6+ písmenami
$ grep -E "\<[a-zA-Z]{6,}\>" morhac.txt

# [a-zA-Z] = písmená (malé a veľké)
# {6,} = minimálne 6
# VÝSTUP: morhac (6), history (7), sullivan (8), ...

# Príklad 5: Rozsah 9-11 písmen
$ grep -E "\<[a-zA-Z]{9,11}\>" morhac.txt

# VÝSTUP: slová s 9, 10 alebo 11 písmenami
```

---

## 🧪 TESTOVÉ OTÁZKY NA GREP

### ❓ Otázka 4: Čo robia tieto príkazy?

```bash
grep "find" file.txt
grep -w "find" file.txt
grep -E "find" file.txt
```

<details>
<summary>✅ Odpoveď</summary>

- `grep "find"` = hľadá "find" KDEKOĽVEK (aj v "finding")
- `grep -w "find"` = hľadá PRESNÉ SLOVO "find" (nie "finding")
- `grep -E "find"` = hľadá "find" s EXTENDED REGEX (modernými znakmi)

**ROZDIEL:** `-w` = boundary (hranica slova), nie rozšírené funkcie!
</details>

---

### ❓ Otázka 5: Nájdi slová s 20+ písmenami

```bash
grep ??? file.txt
```

<details>
<summary>✅ Odpoveď</summary>

```bash
grep -E "\<[a-zA-Z]{20,}\>" file.txt
```

- `-E` = Extended regex (potrebné!)
- `\<` = začiatok slova
- `[a-zA-Z]{20,}` = 20 alebo viac písmen
- `\>` = koniec slova

**VÝSTUP:** slová ako "najfrekventovanejsimi" (22 písmen)
</details>

---

---

# AWK NA SPRACOVANIE DÁT

## 1️⃣ AWK - ZÁKLADNÝ KONCEPT

### Čo je AWK?
**AWK** je **programovací jazyk** na spracovanie **textu a dát** riadok po riadku.

AWK je ideálny na prácu s **tabuľkami** a **stĺpcami**.

### Syntax:
```bash
awk 'PATTERN { AKCIA }' [SÚBOR]
```

### Reálny Príklad:
```bash
# Zo histórie vojtko (november 2025)
$ awk '{print NR, $1}' zamestnanci.txt

# Interpretácia:
# ==============
# awk = príkaz
# {print NR, $1} = AKCIA (vytlač číslo riadku a prvé pole)
# zamestnanci.txt = súbor
#
# NR = Number of Records (číslo riadku)
# $1 = prvé pole (stĺpec 1)
#
# VÝSTUP:
# 1 jan
# 2 mária
# 3 peter
# ...
```

---

## 2️⃣ AWK - ŠPECIÁLNE PREMENNÉ

### Najčastejšie Premenné:

| Premenná | Popis | Príklad | Výstup |
|----------|-------|---------|--------|
| `NR` | **Number of Records** | `awk '{print NR}'` | 1, 2, 3, ... (číslo riadku) |
| `NF` | **Number of Fields** | `awk '{print NF}'` | Počet polí v riadku |
| `$0` | **Celý Riadok** | `awk '{print $0}'` | Celý obsah riadku |
| `$1, $2, ...` | **Polia** | `awk '{print $1}'` | Prvé pole (stĺpec) |
| `$NF` | **Posledné Pole** | `awk '{print $NF}'` | Posledný stĺpec |
| `FS` | **Field Separator** | `awk -F:` | Oddeľovač polí |

### Reálne Príklady:

```bash
# Príklad 1: Čísla riadkov
$ awk '{print NR}' morhac.txt

# VÝSTUP: 1, 2, 3, 4, ...

# Príklad 2: Počet polí
$ awk '{print NF}' morhac.txt

# Ak je riadok: "find -type f"
# VÝSTUP: 3 (tri polia: find, -type, f)

# Príklad 3: Prvé pole
$ awk '{print $1}' morhac.txt

# VÝSTUP: find, grep, chmod, ... (prvý príkaz z každého riadku)

# Príklad 4: Posledné pole
$ awk '{print $NF}' zamestnanci.txt

# Ak je riadok: "jan Mária 2500"
# VÝSTUP: 2500 (plat - posledné pole)

# Príklad 5: Kombinácia
$ awk '{print NR, $1, $NF}' zamestnanci.txt

# VÝSTUP:
# 1 jan 2500
# 2 mária 3000
# 3 peter 2800
# ... (číslo riadku, meno, plat)
```

---

## 3️⃣ AWK - SČÍTÁVANIE A KALKULÁCIE

### Koncept:
AWK umožňuje **sčítavať**, **počítať** a **kalkulovať** čísla.

### Reálne Príklady:

```bash
# Príklad 1: Sčítaj všetky hodnoty v treťom stĺpci
$ awk '{sum+=$3} END {print sum}' data.txt

# {sum+=$3} = pre každý riadok: sum = sum + tretí stĺpec
# END {print sum} = na konci: vypíš súčet
#
# VÝSTUP: 10500 (celkový súčet)

# Príklad 2: Priemer (average)
$ awk '{sum+=$3} END {print sum/NR}' data.txt

# sum/NR = celkový súčet / počet riadkov
# VÝSTUP: 2625 (priemer)

# Príklad 3: Počítanie prvkov
$ awk '$3 > 2000 {count++} END {print count}' data.txt

# $3 > 2000 = ak je tretí stĺpec väčší ako 2000
# {count++} = zvýš počítadlo
# END {print count} = vypíš počet
# VÝSTUP: 4 (počet zamestnancov s platom > 2000)

# Príklad 4: Maximálna a minimálna hodnota
$ awk '{if(NR==1 || $3>max) max=$3} END {print max}' data.txt

# NR==1 = prvý riadok
# $3>max = ak je tretí stĺpec väčší ako max
# max=$3 = aktualizuj maximum
# VÝSTUP: 3000 (najvyšší plat)
```

---

## 4️⃣ AWK - FILTROVANIE (PODMIENKY)

### Koncept:
Vypíš LEN riadky, ktoré spĺňajú podmiesi​nku.

### Reálne Príklady:

```bash
# Príklad 1: Filtrovanie podľa čísla
$ awk '$2 > 2500 {print}' zamestnanci.txt

# $2 > 2500 = ak je DRUHÝ stĺpec väčší ako 2500
# {print} = potom ho vypíš
# VÝSTUP: iba zamestnanci s platom > 2500

# Príklad 2: Filtrovanie podľa textu (regex)
$ awk '/^jan/ {print}' zamestnanci.txt

# /^jan/ = riadky ZAČÍNAJÚCE na "jan"
# VÝSTUP: všetky riadky s menom "jan"

# Príklad 3: Negácia
$ awk '!/^jan/ {print}' zamestnanci.txt

# !/^jan/ = riadky NEZAČÍNAJÚCE na "jan"
# VÝSTUP: všetci OKREM janov

# Príklad 4: Kombinovanie podmienok
$ awk '$2 > 2000 && $2 < 3000 {print}' zamestnanci.txt

# && = AND (VŠETKY podmienky musia byť PRAVDIVÉ)
# VÝSTUP: zamestnanci s platom medzi 2000 a 3000
```

---

## 🧪 TESTOVÉ OTÁZKY NA AWK

### ❓ Otázka 6: Čo vypíše tento príkaz?

```bash
$ awk '{print NR, NF}' file.txt
```

<details>
<summary>✅ Odpoveď</summary>

Vypíše **číslo riadku** a **počet polí** v tom riadku.

Príklad:
```
1 3    (riadok 1 má 3 polia)
2 5    (riadok 2 má 5 polí)
3 2    (riadok 3 má 2 polia)
```

- `NR` = Number of Records (číslo riadku)
- `NF` = Number of Fields (počet polí)
</details>

---

### ❓ Otázka 7: Ako sčítaš všetky hodnoty v treťom stĺpci a vypíš priemer?

```bash
awk ??? data.txt
```

<details>
<summary>✅ Odpoveď</summary>

```bash
awk '{sum+=$3} END {print sum/NR}' data.txt
```

- `{sum+=$3}` = sčítaj tretí stĺpec pre každý riadok
- `END {print sum/NR}` = na konci: priemer = súčet / počet riadkov

**VÝSTUP:** priemer všetkých hodnôt v treťom stĺpci
</details>

---

---

# SED NA ÚPRAVU TEXTU

## 1️⃣ SED - ZÁKLADNÝ KONCEPT

### Čo je SED?
**SED** (Stream EDitor) je príkaz na **transformáciu textu** - nahradenie, mazanie, vkladanie...

### Syntax:
```bash
sed '[ADRESA][AKCIA]' [SÚBOR]
```

### Reálny Príklad:
```bash
# Zo histórie morhac (november 2025)
$ sed 's/old/new/' file.txt

# Interpretácia:
# ==============
# sed = príkaz
# s = akcia (SUBSTITUTE = nahraď)
# old = čo hľadať (FIND)
# new = čím nahradiť (REPLACE)
# file.txt = súbor
#
# VÝSTUP: všetko ako v pôvodnom, ale prvý "old" na každom riadku sa nahradí "new"
```

---

## 2️⃣ SED - AKCIE (COMMANDS)

### Základné Akcie:

| Akcia | Popis | Príklad | Výstup |
|-------|-------|---------|--------|
| `s` | **Substitute** | `s/old/new/` | Nahraď prvý "old" |
| `s///g` | **Global** | `s/old/new/g` | Nahraď VŠETKY "old" |
| `d` | **Delete** | `7d` | Vymaž riadok 7 |
| `p` | **Print** | `7p` | Vypíš riadok 7 |
| `a` | **Append** | `3a text` | Pridaj TEXT za riadok 3 |
| `i` | **Insert** | `3i text` | Vložit TEXT pred riadok 3 |
| `c` | **Change** | `3c text` | Zameniť riadok 3 na TEXT |

### Reálne Príklady:

```bash
# Príklad 1: Nahradiť prvý výskyt
$ sed 's/hello/world/' file.txt

# VÝSTUP (riadok "hello world"):
# world world
# (len PRVÝ "hello" sa nahradí!)

# Príklad 2: Nahradiť VŠETKY výskyty
$ sed 's/hello/world/g' file.txt

# g = GLOBAL (všetko!)
# VÝSTUP (riadok "hello hello hello"):
# world world world
# (VŠETKY "hello" sa nahradili!)

# Príklad 3: Vypíš iba riadky 5-10
$ sed -n '5,10p' file.txt

# -n = no auto-print (nevypísuj všetko)
# 5,10p = print (vypíš) riadky 5 až 10
# VÝSTUP: iba riadky 5-10

# Príklad 4: Vymaž riadky 5-10
$ sed '5,10d' file.txt

# d = delete (vymaž)
# VÝSTUP: všetko OKREM riadkov 5-10

# Príklad 5: Vymaž PRÁZDNE riadky
$ sed '/^$/d' file.txt

# /^$/ = riadky BEZ obsahu (začiatok hneď koniec)
# d = vymaž
# VÝSTUP: žiadne prázdne riadky

# Príklad 6: Vložit text
$ sed '3i Nový riadok' file.txt

# 3i = insert (vložit) na pozícii 3
# VÝSTUP: pred riadok 3 sa vložit "Nový riadok"
```

---

## 🧪 TESTOVÉ OTÁZKY NA SED

### ❓ Otázka 8: Čo robia tieto príkazy?

```bash
sed 's/old/new/' file
sed 's/old/new/g' file
```

<details>
<summary>✅ Odpoveď</summary>

- `sed 's/old/new/' file` = nahraď **PRVÝ** výskyt "old" na každom riadku
- `sed 's/old/new/g' file` = nahraď **VŠETKY** výskyty (g = global)

**ROZDIEL:** `g` = all (všetko), bez `g` = iba prvý!
</details>

---

### ❓ Otázka 9: Ako vypíšeš iba riadky 10-20?

```bash
sed ??? file.txt
```

<details>
<summary>✅ Odpoveď</summary>

```bash
sed -n '10,20p' file.txt
```

- `-n` = no auto-print (nevypísuj všetko)
- `10,20` = rozsah (riadky 10 až 20)
- `p` = print (vypíš)

Bez `-n`:
```bash
sed '10,20p' file.txt  # Vypíše riadky 10-20 DVAKRÁT (duplikát!)
```
</details>

---

---

# TESTOVÉ OTÁZKY Z MINULÝCH ROKOV

## 🎯 KOMBINOVANÉ OTÁZKY

### ❓ Otázka 10: Nájdi všetky .txt súbory zmenené v posledných 7 dniach a počítaj "ERROR"

```bash
??? find . ??? ??? ??? | ??? ??? "ERROR" | ???
```

<details>
<summary>✅ Odpoveď</summary>

```bash
find . -type f -name "*.txt" -mtime -7 -exec grep -c "ERROR" {} \;
```

ALEBO:

```bash
find . -type f -name "*.txt" -mtime -7 | xargs grep -l "ERROR"
```

**Vysvetlenie:**
- `find . -type f -name "*.txt"` = nájdi .txt súbory
- `-mtime -7` = zmenené v posledných 7 dniach
- `-exec grep -c "ERROR" {} \;` = počítaj "ERROR" v každom
- **VÝSTUP:** počet "ERROR" v každom súbore
</details>

---

### ❓ Otázka 11: Sčítaj všetky hodnoty v treťom stĺpci a vypíš súčet

```bash
awk ??? data.txt
```

<details>
<summary>✅ Odpoveď</summary>

```bash
awk '{sum+=$3} END {print sum}' data.txt
```

**ALEBO s formatovaním:**

```bash
awk '{sum+=$3} END {printf "Súčet: %d\n", sum}' data.txt
```

**Výstup:**
```
15000
```

(alebo `Súčet: 15000`)
</details>

---

### ❓ Otázka 12: Nájdi všetky SLOVÁ s presne 21 písmenami

```bash
grep ??? /public/zaciatocnik.txt
```

<details>
<summary>✅ Odpoveď</summary>

```bash
grep -E '\<[a-zA-Z]{21}\>' /public/zaciatocnik.txt
```

**ALEBO:**

```bash
grep -wE '[a-zA-Z]{21}' /public/zaciatocnik.txt
```

**Vysvetlenie:**
- `-E` = extended regex
- `\<` = začiatok SLOVA
- `[a-zA-Z]{21}` = presne 21 písmen
- `\>` = koniec SLOVA
- `-w` = alternative (whole word)

**VÝSTUP:**
```
najfrekventovanejsimi
```

(slovo s 22 písmenami - NEŠLO BY!)
</details>

---

### ❓ Otázka 13: Vymaž všetky riadky obsahujúce "test"

```bash
sed ??? file.txt
```

<details>
<summary>✅ Odpoveď</summary>

```bash
sed '/test/d' file.txt
```

**Vysvetlenie:**
- `/test/` = riadky s "test"
- `d` = delete (vymaž)

**ALEBO - Opakom:**

```bash
sed '/test/!d' file.txt  # Vymaž všetko OKREM "test"
```

**POZOR:** TOTO MODIFIKUJE VÝSTUP, nie pôvodný súbor!
Aby si zmenil súbor:

```bash
sed -i '/test/d' file.txt  # -i = in-place (upraví súbor!)
```
</details>

---

---

# PRAKTICKÉ ÚLOHY S RIEŠENIAMI

## 📋 Úloha 1: Analýza Histórie Príkazov

### Zadanie:
Máš súbor `morhac20112025.txt` s históriou príkazov. Nájdi a vypiš:
1. Všetky príkazy `grep`
2. Počet príkazov `find`
3. Posledný príkaz, ktorý sa vykonali

### Riešenie:

```bash
# 1️⃣ Nájdi všetky grep príkazy
grep " grep " morhac20112025.txt

# VÝSTUP:
# grep -w "find" morhac25092025.history
# grep -wE "[[:alpha:]]{9}" zaciatocnik.txt
# ...

# 2️⃣ Počet find príkazov
grep -c " find " morhac20112025.txt

# VÝSTUP: 45

# 3️⃣ Posledný príkaz
tail -1 morhac20112025.txt

# VÝSTUP: (posledný riadok súboru)
```

---

## 📋 Úloha 2: Vyhľadávanie Veľkých Súborov

### Zadanie:
Nájdi všetky súbory väčšie ako 100 KB, zmenené v poslednom mesiaci, a vypíš ich veľkosť.

### Riešenie:

```bash
# Nájdi a zobraz veľkosť
find . -type f -size +100k -mtime -30 -exec ls -lh {} \;

# VÝSTUP:
# -rw-r--r-- 1 user group 150K Nov 15 morhac20112025.txt
# -rw-r--r-- 1 user group 200K Nov 10 vojtko06112025.txt

# Alternatíva - len veľkosť a názov
find . -type f -size +100k -mtime -30 | xargs ls -lh | awk '{print $5, $9}'

# VÝSTUP:
# 150K morhac20112025.txt
# 200K vojtko06112025.txt
```

---

## 📋 Úloha 3: Analýza Tabulárnych Dát

### Zadanie:
Máš súbor `zamestnanci.txt`:
```
jan     slovakia     2000
mária   cesko        2500
peter   slovensko    3000
```

Vypočítaj:
1. Priemer platu
2. Počet zamestnancov
3. Všetci s platom > 2500

### Riešenie:

```bash
# 1️⃣ Priemer platu (tretí stĺpec)
awk '{sum+=$3} END {print "Priemer: " sum/NR}' zamestnanci.txt

# VÝSTUP: Priemer: 2500

# 2️⃣ Počet zamestnancov
awk 'END {print "Počet: " NR}' zamestnanci.txt

# VÝSTUP: Počet: 3

# 3️⃣ Všetci s platom > 2500
awk '$3 > 2500 {print}' zamestnanci.txt

# VÝSTUP:
# peter   slovensko    3000
```

---

## 📋 Úloha 4: Úprava Log Súborov

### Zadanie:
Máš `app.log` s tisíckami riadkov. Potrebuješ:
1. Vypísať len ERROR riadky
2. Nahradiť všetky "old_server" na "new_server"
3. Vymazať prázdne riadky

### Riešenie:

```bash
# 1️⃣ Iba ERROR riadky
grep "ERROR" app.log

# ALEBO s číslami riadkov:
grep -n "ERROR" app.log

# VÝSTUP:
# 123:2025-11-20 ERROR Connection failed
# 456:2025-11-20 ERROR Timeout

# 2️⃣ Nahradiť server (dve voľby)

# METÓDA 1: sed (bez zmeny pôvodného)
sed 's/old_server/new_server/g' app.log

# METÓDA 2: sed -i (zmeni súbor!)
sed -i 's/old_server/new_server/g' app.log

# METÓDA 3: s zálohou
sed -i.bak 's/old_server/new_server/g' app.log
# Vytvorí app.log.bak ako zálohu!

# 3️⃣ Vymaž prázdne riadky
sed '/^$/d' app.log > app_clean.log

# VÝSTUP: nový súbor bez prázdnych riadkov
```

---

## 🏆 EXPERT KOMBINÁCIA - Všetko Dohromady

### Úloha: Analýza Histórie a Generovanie Reportu

```bash
# Nájdi všetky súbory z posledného týždňa
# Počítaj výskyty príkazov find, grep, awk, sed
# Vypíš TOP 5 najčastejších

find . -type f -name "*.txt" -mtime -7 -exec cat {} \; \
  | sed -E 's/^[0-9]+\s+//' \
  | awk '{print $1}' \
  | sort | uniq -c | sort -rn | head -5

# VÝSTUP:
# 45 find
# 38 grep
# 25 awk
# 20 sed
# 15 chmod
```

**Vysvetlenie:**
1. `find . -type f -name "*.txt" -mtime -7` = nájdi .txt z posledného týždňa
2. `-exec cat {} \;` = vypíš obsah
3. `sed -E 's/^[0-9]+\s+//'` = odstráň čísla na začiatku
4. `awk '{print $1}'` = iba prvý príkaz
5. `sort | uniq -c | sort -rn | head -5` = zráď a zobraz TOP 5

---

## 📚 ZHRNUTIE VŠETKÝCH PRÍKAZOV

### FIND
```bash
find . -type f -name "*.txt"         # Vyhľadávanie
find . -size +100k                   # Veľkosť
find . -mtime -7                     # Čas zmeny
find . -exec tail -2 {} \;           # Spustenie príkazu
```

### GREP
```bash
grep "text" file                     # Vyhľadávanie
grep -w "word" file                  # Presné slovo
grep -E "[0-9]{3}" file             # Regulárne výrazy
grep -c "text" file                 # Počítanie
```

### AWK
```bash
awk '{print $1, $3}' file           # Výber polí
awk '{sum+=$2} END {print sum}' file # Sčítávanie
awk '$2 > 100 {print}' file         # Filtrovanie
```

### SED
```bash
sed 's/old/new/g' file              # Nahradenie
sed -n '5,10p' file                 # Výber riadkov
sed '/pattern/d' file               # Mazanie
sed -i 's/old/new/g' file           # In-place
```

---

**Verzia:** 3.0 ✅  
**Dátum:** 11.12.2025  
**Status:** KOMPLETNÝ - VŠETKY PRÍKAZY S VYSVETLENIAMI A PRÍKLADMI ✨

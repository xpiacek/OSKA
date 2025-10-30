# 💻 HISTÓRIA CVIČIACICH - REÁLNE PRÍKAZY Z TERMINÁLU

**Verzia:** 2.0 - ŽIVÉ PRÍKLADY Z HISTÓRIE  
**Zdroj:** Histórie 8 cvičiacich (október 2025)  
**Formát:** Reálne terminálové výstupy

---

## 📋 Zoznam Príkladov z Histórie

### Všetci Cvičiaci:
- **morhac** - 2 histórie (16.10 & 23.10) - NAJVIAC PRÍKAZOV! ⭐
- **vojtko** - 2 histórie (01.10 & 23.10) - Pokročilý User
- **salgovic** - 2 histórie (01.10 & 15.10) - Systematický
- **grezo** - 2 histórie (08.10 & 15.10) - Detailný
- **tomcala** - 2 histórie (09.10 & 02.10) - Základný

---

## 🔍 FIND - Reálne Príklady

### Príklad 1: Vyhľadávanie kw* Súborov

```bash
# Zo histórie morhac - riadky 496-503
$ find -type f -name "*.kw"
$ find -type f -name "kw"
$ find -type f -name "kw*"
$ find -type f -name "kw*" -size 70c      # Presne 70 bytov
$ find -type f -name "kw*" -size -70c     # Menej ako 70 bytov
$ find -type f -name "kw*" -size +70c     # Viac ako 70 bytov
```

**Vysvetlenie:**
- `find -type f` = iba súbory (nie adresáre)
- `-name "kw*"` = názov začína na "kw"
- `-size 70c` = presne 70 znakov (c = characters/bytes)
- `-size -70c` = "minus" = menej ako 70
- `-size +70c` = "plus" = viac ako 70

---

### Príklad 2: mtime vs ctime vs atime

```bash
# Zo histórie morhac - riadky 790-795
$ find -type f -mtime -14   # Modified (upravené) - menej ako 14 dní
$ find -type f -ctime -14   # Change (zmena metadát) - menej ako 14 dní
$ find -type f -atime -14   # Access (prístup) - menej ako 14 dní
$ find -type f -mtime -7    # Modified - menej ako 7 dní
$ find -type f -mtime -7 -name "*.history"
```

**Pamätaj:**
- `-mtime -7` = mínus = menej ako 7 dní (posledný týždeň)
- `-mtime 7` = presne 7 dní
- `-mtime +7` = plus = viac ako 7 dní

---

### Príklad 3: Pokročilý find s exec

```bash
# Zo histórie morhac - riadky 815-817
$ find -type f -mtime -7 -name "*.history" -exec tail -n 3 {} \;
$ find -type f -mtime -7 -name "*.history" -print -exec tail -n 3 {} \;

# Interpretácia:
# ============
# Nájdi všetky SÚBORY (-type f)
# upravené za posledných 7 dní (-mtime -7)
# s názvom "*.history"
# a potom SPUSTI príkaz tail -n 3 na každom
# {} = názov súboru
# \; = koniec príkazu
```

**Praktické cvičenie:**
```bash
# Skúšaj to sám:
find . -type f -name "*.txt" -exec wc -l {} \;
# Vypíše počet riadkov každého .txt súboru
```

---

## 🔎 GREP - Reálne Príklady

### Príklad 1: Základné vyhľadávanie

```bash
# Zo histórie morhac - riadky 750-756
$ grep "^1" zaciatocnik.txt      # Riadky začínajúce "1"
$ grep "^[0-9]" zaciatocnik.txt  # Riadky začínajúce číslicou
$ grep bash$ /etc/passwd         # Riadky končiace "bash"
$ grep -w "help" historia.txt    # CELÉ SLOVO "help"
$ grep -w "hel" historia.txt     # Nenájde! (nie je to slovo)
```

**Symboly:**
- `^` = začiatok riadku
- `$` = koniec riadku
- `.` = ľubovoľný znak
- `[0-9]` = číslica
- `[a-z]` = malé písmenko
- `-w` = whole word (CELÉ SLOVO!)

---

### Príklad 2: Počítanie a regulárne výrazy

```bash
# Zo histórie morhac - riadky 972-983
$ grep -w "fgrep" zaciatocnik.txt        # Nájsť slovo "fgrep"
$ grep -wE "[a-zA-Z]{6}" zaciatocnik.txt # Slová s presne 6 písmenami
$ grep -wE "[a-zA-Z]{6,}" zaciatocnik.txt # Slová s 6+ písmenami
$ grep -wE "[[:alpha:]]{6,10}" file.txt  # 6 až 10 písmen

# Praktická aplikácia:
$ grep -E "\\<[[:alpha:]]{21}\\>" /public/zaciatocnik.txt
# Nájde VŠETKY slová s presne 21 písmenami!
```

**Flagi:**
- `-w` = whole word
- `-E` = extended regex (modern)
- `-c` = count (počet riadkov)
- `-n` = line numbers
- `-i` = ignore case

---

## 🎬 AWK - Reálne Príklady

### Príklad 1: Práca s poliami (Vojtko história)

```bash
# Zo histórie vojtko - riadky 1223-1227
$ env | grep "A="
$ env | grep "^A"      # Začína na "A"
$ set | grep "^A"      # Všetky premenné začínajúce na "A"

# To sú vlastne awk príklady!
# Ekvivalent v awk:
$ env | awk '$0 ~ /^A/ {print}'  # Riadky začínajúce na "A"
```

---

## 🎯 BASH POLIA - Reálne Príklady

### Príklad 1: Polia bez a s úvodzovkami (Vojtko)

```bash
# Zo histórie vojtko - riadky 1247-1279
$ zoznam=(jeden dva tri styri pat "sest cele sedem")

$ echo $zoznam                    # jeden (prvý prvok)
$ echo "$zoznam"                  # jeden (prvý prvok s quotes)
$ echo "${zoznam[2]}"             # tri (tretí prvok)
$ echo "${#zoznam[@]}"            # 6 (počet prvkov)
$ echo "${zoznam[@]}"             # Všetky prvky
$ echo "${zoznam[@]:2:4}"         # Slice: od indexu 2, 4 prvky

# KRITICKÉ - bez úvodzoviek rozpadne sa!
$ for prvok in ${zoznam[@]}; do
>    echo "$prvok"
> done
# VÝSTUP: jeden dva tri styri pat sest cele sedem (ROZDELENÉ!)

# S úvodzovkami - SPRÁVNE
$ for prvok in "${zoznam[@]}"; do
>    echo "$prvok"
> done
# VÝSTUP: 
# jeden
# dva
# tri
# styri
# pat
# sest cele sedem (SPOLU!)
```

---

### Príklad 2: IFS - Internal Field Separator (Vojtko)

```bash
# Zo histórie vojtko - riadky 1310-1333
$ echo "$IFS" | wc            # IFS má 3 znaky (space, tab, newline)
$ echo -n "$IFS" | wc         # Bez newlinu = 2 znaky

# PROBLÉM: IFS rozdeľuje slová
$ for f in $(ls -l | head -3); do
>    echo "###########$f###########"
> done
# Rozdelí sa na SLOVÁ - problém!

# RIEŠENIE: Nastav IFS na newline
$ IFS=$'\n'
$ for f in $(ls -l | head -3); do
>    echo "###########$f###########"
> done
# Teraz sa rozdelí na RIADKY - správne!

# Test Vojtka:
$ IFS=$'\\n'
$ hexdump <<< "$IFS"    # Kontrola čo je v IFS
$ wc <<< "$IFS"          # Počet znakov
```

---

## 📝 CHMOD - Reálne Príklady

### Príklad 1: Tabelovanie Oprávnení (Morhac)

```bash
# Zo histórie morhac - riadky 574-582
$ chmod 400 filefile.txt   # r--------  (iba read)
$ ls -la                   # Zobrazí: -r-------- (iba vlastník, iba čítanie)

$ chmod 100 filefile.txt   # ---x------  (iba execute)
$ ls -la                   # Zobrazí: ---x------ (iba vlastník, iba vykonávanie)

$ chmod 300 filefile.txt   # -wx-------  (write+execute)
$ ls -la                   # Zobrazí: -wx-------

$ chmod 700 filefile.txt   # rwx-------  (VŠETKO!)
$ ls -la                   # Zobrazí: -rwx------

$ chmod 744 filefile.txt   # rwxr--r--
$ ls -la                   # Zobrazí: -rwxr--r--
```

**Tabuľka Čísel:**
```
Číslica = r(4) w(2) x(1)

0 = --- (nič)
1 = --x (execute)
2 = -w- (write)
3 = -wx (write+execute)
4 = r-- (read)
5 = r-x (read+execute)
6 = rw- (read+write)
7 = rwx (ALL!)
```

---

### Príklad 2: Symboly vs Números (Salgovic)

```bash
# Zo histórie salgovic - riadky 220-241
$ chmod +x subor.txt           # Pridaj execute
$ chmod u+x subor1.txt         # Użívateľovi (user) +execute
$ chmod g+x subor.txt          # Grupe +execute
$ chmod o+x subor.txt          # Ostatným (others) +execute
$ chmod g-x subor.txt          # Grupe -execute

$ chmod 000 subor1.txt         # Zmaž VŠETKO!
$ chmod 001 subor1.txt         # Len execute pre ostatných
$ chmod 764 subor1.txt         # rwxrw-r-- (rôzne práva)
```

---

## 🔌 PRESMEROVANIE - Reálne Príklady

### Príklad 1: stderr vs stdout (Morhac)

```bash
# Zo histórie morhac - riadky 1024-1036
$ find / | grep "Permission denied"      # NEHODIŤ!
$ find / |& grep "Permission denied"     # |& = obidva stdout a stderr!
$ find / > /dev/null                     # Zober VÝSTUP, zahoď (stdout)
$ find / 2> /dev/null                    # Zahoď CHYBY (stderr)
$ find / &> /dev/null                    # Zahoď VŠETKO (stdout+stderr)

# Kombinácia:
$ find / 2> /dev/null | grep "kw"
# Zahoď chyby, ale rezultáty vyhľadaj
```

**Kód:**
```
1 = stdout (normálny výstup)
2 = stderr (chyby)
> = prepíš
>> = pripoj
& = VŠETKO
```

---

## 🎬 PROCESY - Reálne Príklady

### Príklad 1: jobs, fg, bg (Salgovic)

```bash
# Zo histórie salgovic - riadky 285-311
$ sleep 500                      # Spusti proces na 500 sekúnd
# [CTRL+Z]                        # Suspenduj (pause)

$ jobs                           # Zoznam suspendovaných
$ ps                             # Všetky procesy

$ sleep 500                      # Ďalší proces
$ jobs                           # Teraz dva: [1] a [2]

$ fg                             # Vráť posledný do FOREGROUND
$ fg 1                           # Vráť procес #1 do foreground
$ bg                             # Spusti pozastavený v BACKGROUND
$ kill %1                        # Zabij proces #1
```

**Skratky:**
```
[CTRL+Z]  = Suspend (pauza)
jobs      = Zoznam suspendovaných
fg        = Foreground (vrátenie)
bg        = Background (spustenie v pozadí)
kill %1   = Kill proces #1
```

---

## 🔥 PIPE - Reálne Príklady

### Príklad 1: Kombinácia príkazov (Morhac)

```bash
# Zo histórie morhac - všeobecne:
$ grep -w "find" historia.txt | sort
# Nájdeme "find", potom ZORADÍME

$ grep -w "find" historia.txt | sort -r
# Zoradíme NAOPAK (reverse)

$ grep -w "find" historia.txt | sort -rn
# Numerické zoradenie NAOPAK

$ wc zaciatocnik.txt                    # 100  500  5000
$ # = riadky, slova, znaky
```

---

## 🎯 INTERAKTÍVNE CVIČENIE

### Cvičenie 1: Čo sa vypíše?

```bash
$ zoznam=(a b c)
$ echo ${#zoznam[@]}     # CO SA VYPÍŠE?
```

<details>
<summary>Odpoveď</summary>

```
3
```

Vysvetlenie: `${#array[@]}` = počet prvkov v poli
</details>

---

### Cvičenie 2: find problém

```bash
$ find /public -type f -name "*.txt" -size -100c
# CO ZNAMENÁ -size -100c?
```

<details>
<summary>Odpoveď</summary>

Súbory MENŠIE ako 100 bytov.

- `-100c` = minus = menej ako 100
- `100c` = presne 100
- `+100c` = plus = viac ako 100
</details>

---

### Cvičenie 3: grep regex

```bash
$ grep -E "\\<[a-z]{5}\\>" file.txt
# CO NÁJDE?
```

<details>
<summary>Odpoveď</summary>

VŠETKY slová s PRESNE 5 písmenami.

- `\\<` = začiatok slova
- `[a-z]{5}` = presne 5 malých písmen
- `\\>` = koniec slova
</details>

---

## 📊 Štatistika Histórie

| Cvičiaci | Riadky | Príkazy | Prípadové štúdie |
|----------|--------|---------|-----------------|
| morhac | 1300+ | 150+ | find, grep, chmod |
| vojtko | 1370+ | 100+ | polia, IFS, export |
| salgovic | 520+ | 80+ | procesy, chmod |
| grezo | 500+ | 60+ | find, grep |
| tomcala | 1030+ | 70+ | find, grep |
| **SPOLU** | **5200+** | **500+** | **Všetko!** |

---

## 🚀 Ako Používať Túto Sekciu?

1. **Čítaj príklady** - Pochopíš ako to reálne vyzerá
2. **Kopíruj príkazy** - Skúšaj v own terminálu
3. **Experimentuj** - Zmeň parametre
4. **Pochopeň Chyby** - morhac mal ojazdúť pred `chmod`
5. **Aplikuj Poznatky** - Použi vo svojich skriptoch

---

## 💡 Kľúčové Ponaučenia z Histórie

### ✅ Správne Prístupy (Vojtko)
```bash
$ for f in "${array[@]}"; do      # Úvodzovky!
$ find . -exec wc {} \;           # Exec bez chýb
$ export VAR="value"              # Pre podprocesy
```

### ❌ Časté Chyby (Morhac - ale učil sa!)
```bash
find . kw                         # Nesprávna syntax
find . name kw                    # Chýba -type -f
grep vstup* file.txt             # * sa expanduje!
```

---

**Pamätaj:** Všetci títo cvičiaci boli na ZAČIATKU! Skúšali, robili chyby, a učili sa.

**TY MÔŽEŠ AJ!** 🚀

---

**Verzia:** 2.0  
**Posledná Aktualizácia:** 30.10.2025  
**Stav:** LIVE EXAMPLES ✅
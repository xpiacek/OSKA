# 🖥️ GREZO & VOJTKO - ÚPLNÝ PREHĽAD PRÍKAZOV S VLAJKAMI

**Verzia:** 2.0 - KOMPREHENZÍVNY PREHĽAD  
**Zdroj:** Histórie cvičiacich (grezo + vojtko, október 2025)  
**Status:** ✅ VŠETKY VLAJKY, MOŽNOSTI A REÁLNE VÝSTUPY

---

## 📚 OBSAH

1. [FIND - Kompletný Prehľad](#find---kompletný-prehľad)
2. [GREP - Všetky Vlajky a Regex](#grep---všetky-vlajky-a-regex)
3. [TAIL - Práca s Končom Súborov](#tail---práca-s-končom-súborov)
4. [LS - Listovanie Súborov](#ls---listovanie-súborov)
5. [ECHO a PREMENNÉ](#echo-a-premenné)
6. [CHMOD - Oprávnenia](#chmod---oprávnenia)
7. [BASH Polia a Slučky](#bash-polia-a-slučky)

---

## FIND - Kompletný Prehľad

### Základný Find

```bash
# Vojtko - riadok 533
$ find
# VÝSTUP: Vypíše všetko od aktuálneho adresára

# Vojtko - riadok 534
$ find .
# VÝSTUP: Rovnaké ako find

# Vojtko - riadok 535
$ find . -true
# VÝSTUP: Filtruje len tie čo vrátia TRUE (všetky)

# Vojtko - riadok 536
$ find . -true -print
# VÝSTUP: Explicitne vypíše všetko čo je TRUE
```

---

### FIND s TYPE (typ objektu)

| Flag | Popis | Príklad |
|------|--------|---------|
| `-type f` | Iba súbory | `find . -type f` |
| `-type d` | Iba adresáre | `find . -type d` |
| `-type l` | Iba linky | `find . -type l` |
| `-type s` | Iba sockets | `find . -type s` |

```bash
# Vojtko - riadok 538
$ find /public -type f -name "*kw*"
# VÝSTUP: Všetky SÚBORY s "kw" v názve

# Vojtko - riadok 540
$ find /public -type f -or -name "*kw*"
# VÝSTUP: Súbory ALEBO všetko s "kw" (logický OR!)

# Vojtko - riadok 539
$ find /public -type f -and -name "*kw*"
# VÝSTUP: Súbory A musia mať "kw" v názve (AND = logický AND)
```

---

### FIND s NAME (vyhľadávanie podľa názvu)

```bash
# Grezo - riadok 60
$ find -type f -name \*.history
# VÝSTUP: Všetky súbory končiace na .history

# Grezo - riadok 61
$ find /public/ -type f -name \*.history
# VÝSTUP: Všetky .history súbory v /public/

# Grezo - riadok 62
$ find /public/ -type f -name \*.history -ctime 7
# VÝSTUP: Súbory zmenené PRESNE pred 7 dňami

# Grezo - riadok 63
$ find /public/ -type f -name \*.history -atime 7
# VÝSTUP: Súbory PRÍSTUPOVANÉ presne pred 7 dňami

# Grezo - riadok 64
$ find /public/ -type f -name \*.history -mtime 7
# VÝSTUP: Súbory UPRAVENÉ presne pred 7 dňami
```

---

### FIND s ČASOVÝMI VLAJKAMI (0-N dní spätom)

| Vlajka | Popis | Príklad |
|--------|--------|---------|
| `-mtime -7` | Menej ako 7 dní (MINUS!) | Posledný týždeň |
| `-mtime 7` | Presne 7 dní | Pred týždňom |
| `-mtime +7` | Viac ako 7 dní (PLUS!) | Staršie ako týždeň |
| `-atime` | Access time (prístup) | `-atime -7` |
| `-ctime` | Change time (zmena inode) | `-ctime -7` |

```bash
# Grezo - riadok 70
$ find /public/ -type f -name \*.history -ctime -14
# VÝSTUP: Zmeny za posledných 14 dní

# Grezo - riadok 71
$ find /public/ -type f -name \*.history -ctime -7
# VÝSTUP: Zmeny za posledných 7 dní (MINUS = menej ako!)

# Grezo - riadok 72
$ find /public/ -type f -name \*.history -mtime -14
# VÝSTUP: Úpravy za posledných 14 dní

# Grezo - riadok 73
$ find /public/ -type f -name \*.history -mtime -7
# VÝSTUP: Úpravy za posledných 7 dní
```

---

### FIND s SIZE (veľkosť súboru)

| Vlajka | Popis |
|--------|--------|
| `-size 70c` | Presne 70 bytov |
| `-size -70c` | Menej ako 70 bytov |
| `-size +70c` | Viac ako 70 bytov |
| `c` = bytes | `b` = blky, `k` = KB, `M` = MB |

```bash
# Morhac - riadok 501
$ find -type f -name "kw*" -size 70c
# VÝSTUP: Súbory s PRESNE 70 bytmi

# Morhac - riadok 502
$ find -type f -name "kw*" -size -70c
# VÝSTUP: Súbory s MENEJ ako 70 bytmi

# Morhac - riadok 503
$ find -type f -name "kw*" -size +70c
# VÝSTUP: Súbory s VIAC ako 70 bytmi
```

---

### FIND s EXEC (spustenie príkazu na výsledkoch)

```bash
# Grezo - riadok 83
$ find /public/ -type f -name \*.history -exec tail
# VÝSTUP: CHYBA! Bez {}; sa exec nevykoná správne

# Grezo - riadky 99-100 (SPRÁVNE!)
$ find /public/ -type f -name \*.history -mtime -7 -exec tail -2 {} \;
$ find /public/ -type f -name \*.history -mtime -7 -exec tail -2 {} \\;
# VÝSTUP: Spúšťa "tail -2" na KAŽDOM súbore
# {} = názov súboru
# \; = koniec príkazu
# BASH potrebuje \; alebo '\\;' na escaping

# Grezo - riadok 105
$ find /public/ -type f -name \*.history -mtime -7 -exec tail -n 2 {} \\;
# VÝSTUP: Ukončenie súboru (posledných 2 riadkov) pre KAŽDÝ súbor
```

---

### FIND s PRESMEROVANÍM

```bash
# Grezo - riadok 114
$ find /public/ -type f -name \*.history -mtime -7 > fin
# VÝSTUP: Presmeruje STDOUT do fin

# Grezo - riadok 117
$ find /public/ -type f -name \*.history -mtime -7 >> fin
# VÝSTUP: APPEND (pripojí) do fin

# Grezo - riadok 126
$ find /public/ -type f -name \*.history -mtime -7 2> fin
# VÝSTUP: Presmeruje STDERR (chyby) do fin

# Grezo - riadok 128
$ find /public/ -type f -name \*.history -mtime -7 1> fin
# VÝSTUP: Presmeruje STDOUT (1 = stdout, 2 = stderr)

# Grezo - riadok 135 (KOMBINOVANIE!)
$ find /public/ -type f -name \*.history -mtime -7 -exec tail > fin 2>&1
# VÝSTUP: Stdout aj stderr do fin
```

---

### FIND - KOMBINOVANIE PODMIENOK

```bash
# Vojtko - riadok 544
$ find /public \\( -type f -or -name "*kw*" \\) -print
# VÝSTUP: ZÁTVORKY na zoskupenie!
# ( -type f -or -name "*kw*" ) = SÚBORY ALEBO všetko s kw

# Grezo - riadok 58
$ find /public \\( -type d -name "*kw*" -or -type f -name "*kw*" \\) -print
# VÝSTUP: Adresáre s "kw" ALEBO súbory s "kw"
```

---

## GREP - Všetky Vlajky a Regex

### Základný GREP

```bash
# Grezo - riadok 159
$ grep /public/zaciatocnik.txt
# VÝSTUP: Chyba! Chýba pattern

# Grezo - riadok 160
$ grep " " /public/zaciatocnik.txt
# VÝSTUP: Všetky riadky s MEDZEROU
```

---

### GREP s REGULÁRNYMI VÝRAZMI

#### Anchory (^ a $)

```bash
# Grezo - riadok 163
$ grep -Ew '^[0-9]' /public/zaciatocnik.txt
# VÝSTUP: Riadky ZAČÍNAJÚCE na číslicu
# ^ = začiatok riadku
# -Ew = extended WORD boundary

# Grezo - riadok 164
$ grep -Ew '[0-9]$' /public/zaciatocnik.txt
# VÝSTUP: Riadky KONČIACE na číslicu
# $ = koniec riadku

# Grezo - riadok 165
$ grep -E '[a-zA-Z]$' /public/zaciatocnik.txt
# VÝSTUP: Riadky KONČIACE na písmenko
# -E = extended regex (moderne!)
```

---

#### Triedy Znakov

```bash
# [a-z] = malé písmená
# [A-Z] = veľké písmená
# [0-9] = číslic
# [a-zA-Z] = všetky písmená
# [a-zA-Z0-9] = písmená a číslic
# [^ ...] = NEGÁCIA (všetko okrem)
```

---

#### Kvantifikátory

```bash
# Grezo - riadok 161
$ grep -Ew '[a-zA-Z]{16,17}' /public/zaciatocnik.txt
# VÝSTUP: Slová s 16 alebo 17 písmenami!
# {16,17} = presne 16 ALEBO 17
# {16,} = minimálne 16
# {16} = presne 16

# Grezo - riadok 162
$ grep -E '\\<[a-zA-Z]{16,17}\\>' /public/zaciatocnik.txt
# VÝSTUP: Slová (s hranicami) 16-17 písmen
# \\< = začiatok SLOVA
# \\> = koniec SLOVA
# -Ew = ekvivalent hraníc slov
```

---

#### Konkrétne Príklady

```bash
# Grezo - riadok 166
$ grep -E 'ss$' /public/zaciatocnik.txt
# VÝSTUP: Všetky riadky KONČIACE na "ss"

# Grezo - riadok 167-169 (HERE DOCUMENTS!)
$ grep jednouzivatelsky /public/zaciatocnik.txt
# VÝSTUP: Riadky s "jednouzivatelsky"

$ grep jednouzivatelsky < /public/zaciatocnik.txt
# VÝSTUP: Rovnaké - preusmerovanie vstup

$ grep jednouzivatelsky << /public/zaciatocnik.txt
# VÝSTUP: Syntaxová chyba! << je HERE DOCUMENT (čaká vstup)

# Grezo - riadok 171 (HERE STRING!)
$ grep jednouzivatelsky <<< "tu bude zaujimavy string jednouzivatelsky a bude pokracovat"
# VÝSTUP: jednouzivatelsky (Match!)
# <<< = HERE STRING (priamo text)
```

---

### GREP Vlajky

| Vlajka | Popis | Príklad |
|--------|--------|---------|
| `-E` | Extended regex (moderný) | `grep -E '[a-z]{5}'` |
| `-w` | Whole word (celé slovo) | `grep -w "hello"` |
| `-Ew` | Extended + word | `grep -Ew '[a-z]{5}'` |
| `-c` | Count (počet) | `grep -c "pattern"` |
| `-n` | Line numbers | `grep -n "pattern"` |
| `-i` | Ignore case | `grep -i "PATTERN"` |
| `-v` | Invert (opak) | `grep -v "pattern"` |
| `-P` | Perl regex | `grep -P '\d{5}'` |

```bash
# Grezo - riadok 162
$ grep -E '\\<[a-zA-Z]{16,17}\\>' /public/zaciatocnik.txt
# VÝSTUP: Extended regex so slovnými hranicami
```

---

## TAIL - Práca s Končom Súborov

### Základný TAIL

```bash
# Grezo - riadok 74
$ tail /public/ucebnove/historia/vojtko01102025-17.history
# VÝSTUP: Posledných 10 riadkov súboru (default!)

# Grezo - riadok 78
$ tail -5 /public/ucebnove/historia/vojtko02102025-14.history
# VÝSTUP: Posledných 5 riadkov

# Grezo - riadok 79
$ tail -n 5 /public/ucebnove/historia/vojtko02102025-14.history
# VÝSTUP: -n = explicitne "number of lines"
```

---

### TAIL s Viacerými Súbormi

```bash
# Grezo - riadok 102
$ tail -2 file1.history file2.history
# VÝSTUP: Posledných 2 riadkov z KAŽDÉHO súboru

# Grezo - riadok 103
$ tail -n 2 file1.history file2.history
# VÝSTUP: Rovnaké - s explicitným -n
```

---

### TAIL s PIPE

```bash
# Grezo - riadok 81
$ find /public/ -type f -name \*.history -mtime -7 | tail
# VÝSTUP: Posledných 10 nájdených súborov

# Grezo - riadok 82
$ find /public/ -type f -name \*.history -mtime -7 | tail -1
# VÝSTUP: Iba POSLEDNÝ nájdený súbor

# Grezo - riadky 106-107 (Command Substitution!)
$ tail -n 1 $(find /public/ -type f -name \*.history -mtime -7)
# VÝSTUP: Posledný riadok z KAŽDÉHO nájdeného súboru
# $(find ...) = nahradí sa výsledkom find
```

---

## LS - Listovanie Súborov

### LS Vlajky

| Vlajka | Popis | Príklad |
|--------|--------|---------|
| `-a` | All (vrátane skrytých) | `ls -a` |
| `-l` | Long format (detailný) | `ls -l` |
| `-la` | Kombinácia | `ls -la` |
| `-al` | Rovnaké (poradie bez vplyvu) | `ls -al` |
| `-lF` | Long + file type | `ls -lF` |

```bash
# Grezo - riadok 87
$ ls -a
# VÝSTUP: Všetko vrátane . a ..

# Grezo - riadok 88
$ ls -l
# VÝSTUP: Detailný zoznam

# Grezo - riadok 89
$ ls -al
# VÝSTUP: Detailný + skryté súbory

# Grezo - riadok 90
$ ls -alF
# VÝSTUP: Detailný + skryté + typ súboru (/ = adr, * = spustiteľný)
```

---

## ECHO a PREMENNÉ

### Citovanie a Escaping

```bash
# Vojtko - riadok 383
$ echo "PATH"
# VÝSTUP: PATH (literál - bez rozbálenia!)

# Vojtko - riadok 382
$ echo "$PATH"
# VÝSTUP: /usr/bin:/bin:... (ROZBÁLENIE!)

# Vojtko - riadok 445
$ echo '$PATH'
# VÝSTUP: $PATH (jednoduché úvodzovky = literál!)

# Vojtko - riadok 446
$ echo "$PATH"
# VÝSTUP: /usr/bin:/bin:... (dvojité úvodzovky = ROZBÁLENIE!)

# Grezo - riadok 152-156 (GLOBBING!)
$ echo *
# VÝSTUP: Všetky súbory v adresári (shell expansion!)

$ echo "*"
# VÝSTUP: * (literál - v úvodzovkách sa neexpanduje!)

$ echo \\*
# VÝSTUP: * (escaped *)
```

---

## CHMOD - Oprávnenia

### Číselné Oprávnenia

| Číslo | Binárne | Oprávnenie |
|-------|---------|-----------|
| 0 | 000 | --- (žiadne) |
| 1 | 001 | --x (execute) |
| 2 | 010 | -w- (write) |
| 3 | 011 | -wx (write+execute) |
| 4 | 100 | r-- (read) |
| 5 | 101 | r-x (read+execute) |
| 6 | 110 | rw- (read+write) |
| 7 | 111 | rwx (ALL!) |

---

### Reálne Príklady z Histórie

```bash
# Morhac - riadok 574
$ chmod 400 filefile.txt
# VÝSTUP: -r-------- (vlastník len čítaj!)

# Morhac - riadok 576
$ chmod 100 filefile.txt
# VÝSTUP: ---x------ (iba execute!)

# Morhac - riadok 578
$ chmod 300 filefile.txt
# VÝSTUP: -wx------- (write+execute!)

# Morhac - riadok 580
$ chmod 700 filefile.txt
# VÝSTUP: -rwx------ (VŠETKO pre vlastníka!)

# Morhac - riadok 582
$ chmod 744 filefile.txt
# VÝSTUP: -rwxr--r-- (7=vlastník ALL, 4=grupa READ, 4=ostatní READ)
```

---

### CHMOD Formáty

```bash
# Morhac - riadky 170-172 (Symbolické!)
$ chmod 600 text1.txt    # rwx...
$ chmod 700 text1.txt    # rwx-------
$ chmod 744 text1.txt    # rwxr--r--

# Vojtko - riadok 396 (which s vlajkou!)
$ which -a gcc
# VÝSTUP: Všetky výskyty gcc v PATH
# -a = all
```

---

## BASH Polia a Slučky

### Polia - Deklarácia a Prístup

```bash
# Grezo - riadok 883
$ zoznam=(jeden dva tri styri pat "sest cele sedem")
# VÝSTUP: Pole s 6 prvkami!
# "sest cele sedem" = JEDEN prvok (kvôli úvodzovkám!)

# Grezo - riadok 884
$ echo ${zoznam[2]}
# VÝSTUP: tri (index 2 = tretí prvok, indexovanie od 0!)

# Grezo - riadok 885
$ echo ${#zoznam[2]}
# VÝSTUP: 3 (dĺžka tretieho prvku - "tri" má 3 znaky!)

# Grezo - riadok 886
$ echo ${#zoznam[5]}
# VÝSTUP: 15 (dĺžka "sest cele sedem")

# Grezo - riadok 887
$ echo ${#zoznam[@]}
# VÝSTUP: 6 (počet prvkov!)

# Grezo - riadok 888
$ echo ${zoznam[@]}
# VÝSTUP: jeden dva tri styri pat sest cele sedem (všetky!)

# Grezo - riadok 889
$ echo ${zoznam[@]:2:4}
# VÝSTUP: tri styri pat "sest cele sedem"
# :2:4 = od indexu 2, počet 4 prvkov!
```

---

### Polia - BEZ vs S Úvodzovkami (KRITICKÉ!)

```bash
# Grezo - riadok 890
$ zoznam2=(${zoznam[@]} osem)
# VÝSTUP: 8 prvkov! (${zoznam[@]} bez úvodzoviek = ROZPADNE SA!)
# "sest cele sedem" sa rozpadne na "sest", "cele", "sedem"

# Grezo - riadok 891
$ echo ${#zoznam2[@]}
# VÝSTUP: 8 (nie 7!)

# Grezo - riadok 892-894 (SPRÁVNE!)
$ zoznam2=("${zoznam[@]}" osem)
# VÝSTUP: 7 prvkov! ("${zoznam[@]}" s úvodzovkami = OSTANE SPOLU!)
$ echo ${#zoznam2[@]}
# VÝSTUP: 7
$ echo ${zoznam2[@]}
# VÝSTUP: jeden dva tri styri pat sest cele sedem osem
```

---

### Polia v Slučkách

```bash
# Grezo - riadky 895-901 (VŠETKY VARIANTY!)

# Variant 1: BEZ úvodzoviek okolo ${zoznam[@]}
$ for prvok in "${zoznam[@]}"; do echo "$prvok"; done
# VÝSTUP (SPRÁVNE):
# jeden
# dva
# tri
# styri
# pat
# sest cele sedem

# Variant 2: BEZ úvodzoviek (PROBLÉM!)
$ for prvok in ${zoznam[@]}; do echo "$prvok"; done
# VÝSTUP (ZLÉ - ROZPADNE SA):
# jeden
# dva
# tri
# styri
# pat
# sest      ← ROZPADLO SA!
# cele
# sedem

# Variant 3: echo bez úvodzoviek
$ for prvok in "${zoznam[@]}"; do echo $prvok; done
# VÝSTUP (OK, ale echo záznamuje znaky):
# jeden dva tri styri pat sest cele sedem
```

---

### IFS - Internal Field Separator (KRITICKÉ!)

```bash
# Grezo - riadok 934-945 (PROBLÉM vs RIEŠENIE!)

# BEZ nastavenia IFS
$ for f in $(ls -l | head -3); do echo "$f"; done
# VÝSTUP: ROZDELÍ sa na SLOVÁ! (default IFS = space, tab, newline)

# RIEŠENIE - Nastav IFS na NEWLINE
$ tmpIFS="$IFS"
$ echo $IFS
# VÝSTUP: (3 znaky - space, tab, newline)

$ IFS=$'\\n'
$ for f in $(ls -l | head -3); do echo "$f"; done
# VÝSTUP (SPRÁVNE - RIADKY!):
# total 512
# -rw-r--r-- 1 root root 27128 Oct 23 morhac23102025.txt
# -rw-r--r-- 1 root root 26812 Oct 16 morhac16102025.txt

# Vráť originálny IFS
$ IFS="$tmpIFS"
```

---

### Escape Znaky v BASH

```bash
# Grezo - riadky 962-966 (ECHO s Backslashom!)

$ echo ' \\\\  \\\\'
# VÝSTUP: \\  \\ (v single quotes - literál!)

$ echo '"  \\\\ \\\\"'
# VÝSTUP: "  \\ \\" (mixed quotes)

$ echo " \\\\  \\\\\""
# VÝSTUP: \\  \\" (double quotes - čiastočné rozbálenie)

# PRAVIDLO:
# '' = VŠETKO je literál (okrem samotného ')
# "" = Skoro všetko je literál, okrem $, \`, \$, \\, \"
# \\ = escape pre backslash = jeden \
```

---

## WHICH a PATH

```bash
# Vojtko - riadky 393-396 (which a PATH!)

$ which less
# VÝSTUP: /usr/bin/less (prvý less v PATH)

$ which gcc
# VÝSTUP: /usr/bin/gcc

$ which -a gcc
# VÝSTUP: /usr/bin/gcc (všetky výskyty!)
# -a = all (môžu byť aj v /bin, /usr/local/bin...)

# Vojtko - riadky 403-419 (Zmena PATH!)
$ PATH=".:$PATH"
# VÝSTUP: Pridá aktuálny adresár na začiatok PATH!

$ ls
# VÝSTUP: Normálne /bin/ls

$ ./ls
# VÝSTUP: Ak je ls v aktuálnom adresári, spúšťa sa ten!
```

---

## PROCESY a JOBS

```bash
# Vojtko - riadky 459-496 (Procesy a Job Control!)

# Vojtko - riadok 462
$ ps aux
# VÝSTUP: Všetky procesy v detailnom formáte

# Vojtko - riadok 464
$ jobs
# VÝSTUP: [1]+ Running vim

# Vojtko - riadky 477-481 (Background a Jobs!)
$ hexdump /dev/random > /dev/null &
$ jobs
# VÝSTUP: [1]+ Running hexdump /dev/random...

$ ps aux
# VÝSTUP: hexdump proces je viditeľný

$ kill 29547
# VÝSTUP: Zabije proces s PID 29547

# Vojtko - riadky 487-497 (fg a bg!)
$ vim &
$ jobs
# VÝSTUP: [3]- Running vim &

$ fg %3
# VÝSTUP: Vráti job #3 do foreground

$ fg -
# VÝSTUP: Vráti predchádzajúci job
```

---

## HISTÓRICKÉ PRÍKAZY - Špeciálne

```bash
# Vojtko - riadok 519-531 (Pipe varianty!)

$ fortune -o | cowsay
# VÝSTUP: Normálny pipe (stdout -> stdin)

$ fortune -o |& cowsay
# VÝSTUP: Pipe STDOUT a STDERR spolu!
# |& = kombinácia obidvoch

$ fortune -o |2 cowsay
# VÝSTUP: CHYBA! |2 nie je syntax, musí byť 2|

$ fortune -o |1 cowsay
# VÝSTUP: CHYBA! |1 nie je platný
```

---

## TABUĽKA VŠETKÝCH VLAJOK

### FIND Vlajky

| Vlajka | Popis |
|--------|--------|
| `-type f` | Iba súbory |
| `-type d` | Iba adresáre |
| `-name "*.txt"` | Názov s pattern |
| `-mtime -7` | Upravený za posledných 7 dní |
| `-atime -7` | Prístupovaný za posledných 7 dní |
| `-ctime -7` | Zmena inode za 7 dní |
| `-size +100c` | Väčšie ako 100 bytov |
| `-exec cmd {} \;` | Spusti príkaz na výsledkoch |
| `-print` | Explicitne vypíš |
| `-and` / `-or` | Logické operátory |

---

### GREP Vlajky

| Vlajka | Popis |
|--------|--------|
| `-E` | Extended regex |
| `-w` | Whole word |
| `-c` | Count riadkov |
| `-n` | Line numbers |
| `-i` | Ignore case |
| `-v` | Invert (opak) |
| `-P` | Perl regex |
| `-Ew` | Extended + word |

---

### Regex Znaky

| Znak | Popis | Príklad |
|------|--------|---------|
| `^` | Začiatok riadku | `^[0-9]` |
| `$` | Koniec riadku | `[a-z]$` |
| `.` | Ľubovoľný znak | `a.c` = "abc", "adc" |
| `*` | 0 alebo viac | `a*` |
| `+` | 1 alebo viac | `a+` |
| `?` | 0 alebo 1 | `a?` |
| `{n}` | Presne n | `a{3}` = "aaa" |
| `{n,m}` | n až m | `a{2,4}` = "aa", "aaa", "aaaa" |
| `[...]` | Trieda znakov | `[a-z]` = malé písmená |
| `[^...]` | Negácia | `[^a-z]` = NIE malé |
| `\<` | Začiatok slova | `\<word` |
| `\>` | Koniec slova | `word\>` |
| `\|` | OR v regex | `cat\|dog` |

---

## KOMPLETNÝ PRÍKLAD - kombinovanie všetkého

```bash
# Ako by som to všetko kombinoval:

# Nájdi všetky .history súbory z posledného týždňa
# a vypíš posledné 2 riadky každého
# so chybami presmerovanými do dev/null

find /public -type f -name "*.history" -mtime -7 \
  -exec tail -n 2 {} \; 2>/dev/null

# Alebo s grepom na filtrovanie:
find /public -type f -name "*.history" -mtime -7 \
  | xargs grep -E "^[0-9]" \
  | head -20

# Alebo so všetkými vlajkami:
grep -Ew -n "^[a-z]{5,}" /public/zaciatocnik.txt | tail -10
```

---

**Verzia:** 2.0 ✅  
**Posledná Aktualizácia:** 30.10.2025  
**Status:** HOTOVO - VŠETKY VLAJKY, MOŽNOSTI A REÁLNE VÝSTUPY ✨
# Operačné Systémy - Praktické príklady zo cvičení

## História cvičiacich - Praktické príklady

### Základné príkazy UNIX

#### find - Vyhľadávanie súborov

```bash
# Nájsť všetky .history súbory v /public
find /public/ -type f -name \*.history

# Nájsť súbory upravené za posledných 7 dní
find /public/ -type f -name \*.history -mtime -7

# Nájsť súbory a vykonať tail na každom
find /public/ -type f -name \*.history -mtime -7 -exec tail -2 {} \;

# Kombinácia s inými príkazmi
tail -n 1 $(find /public/ -type f -name \*.history -mtime -7)
```

#### Presmerovanie

```bash
# Presmerovanie stdout do súboru (prepíše)
find /public/ -type f -name \*.history -mtime -7 > fin

# Presmerovanie stdout do súboru (pripojí)
find /public/ -type f -name \*.history -mtime -7 >> fin

# Presmerovanie stderr
find / 2> errors.txt

# Presmerovanie stdout aj stderr
find / > output.txt 2>&1
find / &> output.txt  # skrátená verzia

# Presmerovanie stderr na stdout
find / 2>&1 | grep "permission"
```

#### grep - Vyhľadávanie v obsahu

```bash
# Základné použitie
grep "jednouzivatelsky" /public/zaciatocnik.txt

# Regulárne výrazy
grep -E '\<[a-zA-Z]{16,17}\>' /public/zaciatocnik.txt  # slová s 16-17 písmen
grep -E '^[0-9]' /public/zaciatocnik.txt  # riadky začínajúce číslicou
grep -E '[a-zA-Z]$' /public/zaciatocnik.txt  # riadky končiace písmenom
grep -E 'ss$' /public/zaciatocnik.txt  # riadky končiace na "ss"

# Here document
grep jednouzivatelsky << KONIEC
sdpgikjsdg
sdgpsjdgk
jednouzivatelsky ten
aspdfjas
KONIEC

# Here string
grep jednouzivatelsky <<< "tu bude zaujimavy string jednouzivatelsky"
```

### Bash skripty z cvičení

#### Skript 1: Základná slučka while

**2.sh**
```bash
#!/bin/bash

for d in /public/prednasky /public/ucebnove /public/priklady
do
    echo -n $d
    ls $d | wc -l
done

# Cyklus podobný C
for (( i=0 ; i<5 ; i++ ))
do
    echo "opakovanie $i"
done
```

**Kľúčové koncepty:**
- `for in` cyklus
- `echo -n` (bez nového riadku)
- `for (( ))` syntax z C
- Pipe `|`

---

#### Skript 2: Premenné a citovanie

**3.sh**
```bash
#!/bin/bash

veta="ahoj svet"
pocet=3

while [ $pocet -gt 0 ]; do
    echo $pocet $veta           # Výstup: 3 ahoj svet
    echo "$pocet $veta"         # Výstup: 3 ahoj svet
    echo '$pocet $veta'         # Výstup: $pocet $veta (literál)
    echo \$pocet \$veta         # Výstup: $pocet $veta (escape)
    echo ""
    ((pocet--))
done
```

**Kľúčové koncepty:**
- Rozdiel medzi `"..."` a `'...'`
- Escape `\`
- Aritmetické operácie `(( ))`
- Test podmienka `[ ]`
- While cyklus

---

#### Skript 3: Substitúcia a transformácia

**4.sh**
```bash
#!/bin/bash

for f in [A-Z]*; do
    echo "$f" | tr 'A-Z' 'a-z'
    mv -i "$f" "$(echo "$f" | tr A-Z a-z)"
    # Alternatívny syntax:
    #mv -i "$f" "`echo "$f" | tr A-Z a-z`"
done
```

**Kľúčové koncepty:**
- Glob pattern `[A-Z]*`
- Command substitution `$(...)` vs `` `...` ``
- `tr` na transformáciu znakov
- Citovanie v cykloch

---

#### Skript 4: Spracovanie argumentov

**5.sh**
```bash
#!/bin/bash

help="Help: $0 arg1 arg2 arg3 ... argN"

if [ "$#" == "0" ]; then
    echo "$help"
    exit 1
fi

unset debug

echo "Argumenty: $@"

while (( "$#" )); do
    case "$1" in
        -d|-D)
            debug=''
            shift
            break
            ;;
        abc|cba)
            echo "pokracujem dalej"
            ;;
        -h)
            echo "$help"
            exit 0
            ;;
        -*)
            echo "Neznamy prepinac \"$1\""
            exit 1
            ;;
        *)
            break
            ;;
    esac
    shift
done

if [[ -v debug ]]; then 
    echo "Dalsie argumenty: $@"
fi
```

**Použitie:**
```bash
bash ./5.sh               # Vypíše help, exit 1
bash ./5.sh -h            # Vypíše help, exit 0
bash ./5.sh -d arg1 arg2  # Nastaví debug, vypíše argumenty
bash ./5.sh abc cba a     # Vypíše "pokracujem dalej"
bash ./5.sh -e            # Neznamy prepinac, exit 1
```

**Kľúčové koncepty:**
- `$#` - počet argumentov
- `$@` - všetky argumenty
- `$0` - názov skriptu
- `shift` - posunutie argumentov
- `case` statement
- `[[ -v variable ]]` - test či premenná existuje
- `unset` - zrušenie premennej
- Exit kódy

---

#### Skript 5: Pole (arrays)

**6.sh**
```bash
#!/bin/bash

zoznam=(jeden dva tri styri pat "sest cele sedem")

echo $zoznam              # jeden (prvý prvok)
echo ${zoznam[2]}         # tri (index 2)
echo ${#zoznam[2]}        # 3 (dĺžka prvku na indexe 2)
echo ${#zoznam[5]}        # 15 (dĺžka "sest cele sedem")
echo ${#zoznam[@]}        # 6 (počet prvkov)
echo ${zoznam[@]:2:4}     # tri styri pat sest (slice)

zoznam1=(${zoznam[@]:0:6} sedem ${zoznam[$((7-${#zoznam[@]}))]})
echo ""

# Rozbalenie bez úvodzoviek
zoznam2=(${zoznam[@]} osem)
echo ${#zoznam2[@]}       # 8 ("sest cele sedem" sa rozpadne)

# Rozbalenie s úvodzovkami
zoznam2=("${zoznam[@]}" osem)
echo ${#zoznam2[@]}       # 7 ("sest cele sedem" zostane spolu)
echo ${zoznam2[@]}

# Iterácia s úvodzovkami
for prvok in "${zoznam[@]}"; do
    echo "$prvok"
done
```

**Výstup:**
```bash
# ${zoznam[@]} bez úvodzoviek
jeden
dva
tri
styri
pat
sest
cele
sedem

# "${zoznam[@]}" s úvodzovkami
jeden
dva
tri
styri
pat
sest cele sedem
```

**Kľúčové koncepty:**
- Deklarácia poľa `array=(a b c "d e")`
- `${array[i]}` - prvok na indexe
- `${#array[i]}` - dĺžka prvku
- `${#array[@]}` - počet prvkov
- `${array[@]:start:length}` - slice
- Rozdiel medzi `${array[@]}` a `"${array[@]}"`
- `$((arithmetic))` - aritmetické operácie

---

#### Skript 6: Práca s cestami

**7.sh**
```bash
#!/bin/bash

cesta=/public/ucebnove/seminar_1/vim.txt

echo $(dirname $cesta)                         # /public/ucebnove/seminar_1
echo $(basename $cesta)                        # vim.txt
echo $(dirname $(dirname $cesta))              # /public/ucebnove
echo $(basename $(dirname $cesta))             # seminar_1
```

**Kľúčové koncepty:**
- `dirname` - adresárová časť cesty
- `basename` - názov súboru
- Vnorené command substitution

---

#### Skript 7: IFS (Internal Field Separator)

**8.sh**
```bash
#!/bin/bash

ls -l

echo ""

# Bez nastavenia IFS - rozdelí sa na slová
for f in $(ls -l | head -3); do
    echo $f
done

echo ""

# S nastavením IFS na newline - rozdelí sa na riadky
IFS='
'

for f in $(ls -l | head -3); do
    echo $f
done
```

**Výstup bez IFS:**
```
total
512
-rw-r--r--
1
user
group
...
```

**Výstup s IFS='\\n':**
```
total 512
-rw-r--r-- 1 user group 1024 Oct 15 12:00 file.txt
drwxr-xr-x 2 user group 512 Oct 15 12:00 dir
```

**Kľúčové koncepty:**
- `IFS` - Internal Field Separator
- Uloženie a obnovenie `tmpIFS="$IFS"`
- Rozdiel v spracovaní command substitution

---

#### Skript 8: source a exec

**9.sh**
```bash
#!/bin/bash

run=0

if [ $# -ne 1 ]; then
    echo "start"
    source $0 prvykrat      # Spustí v aktuálnom shelle
    #. $0 prvykrat          # Alternatívny syntax
    echo "$var1"            # Vypíše "prvykrat"
    echo ""
    exec $0 druhykrat       # Nahradí aktuálny proces
    echo "$var2"            # Nikdy sa nevykoná
elif [ $1 == prvykrat ]; then
    var1=prvykrat
    echo "$var1 zo source"
elif [ $1 == druhykrat ]; then
    var2=druhykrat
    echo "$var2 z exec"
fi
```

**Výstup:**
```
start
prvykrat zo source
prvykrat

druhykrat z exec
```

**Kľúčové koncepty:**
- `source script` alebo `. script` - spustí v aktuálnom shelle (premenné zostanú)
- `exec script` - nahradí aktuálny proces (všetko za exec sa nevykoná)
- Rozdiel oproti `bash script` - nový subprocess

---

#### Skript 9: Komplexný príklad s PID a exit kódmi

**9-2.sh**
```bash
#!/bin/bash

echo "PID: $$"

if [ "$1" == "druhy raz" ]; then
    echo "a = $a"
    echo "b = $b"
    exit 2
elif [ "$1" == "treti raz" ]; then
    if [ -u a ]; then
        exit 1
    fi
    echo "a = $a"
    echo "b = $b"
    exit 3
elif [ "$1" == "stvrty raz" ]; then
    echo "$1 zo source"
    echo "a = $a"
    echo "b = $b"
    echo '$#' = $#
    echo "\$1 = $1"
elif [ "$1" == "piaty raz" ]; then
    echo "$1 z exec"
    echo "a = $a"
    echo "b = $b"
    exit 5
fi

if [ $# -ne 0 ]; then
    echo "Spustam piaty raz"
    exec $0 "piaty raz"
    echo "Debug: piaty raz, status = $?"
    echo koniec
    exit 0
fi

a=premenna_shellu
export b=premenna_prostredia

echo "Spustam druhy raz"
$0 "druhy raz"
echo "Debug: druhy raz, status = $?"

echo "Spustam treti raz"
$0 "treti raz"
echo "Debug: treti raz, status = $?"

echo "Spustam stvrty raz"
set "stvrty raz"
. $0
echo "Debug: stvrty raz, status = $?"
```

**Kľúčové koncepty:**
- `$$` - PID aktuálneho procesu
- `$?` - návratový kód posledného príkazu
- `export` - exportovanie premennej do prostredia
- Premenné shellu vs premenné prostredia
- `set` - nastavenie pozičných parametrov
- Rozdiel medzi `$0 arg`, `source $0`, a `exec $0`

---

## Praktické príklady z histórie cvičiacich

### Práca so súbormi a právami

```bash
# Zistiť typ súboru
file sed.txt
file pos.sh

# Zmena práv
chmod +x skript.sh
sudo chmod +x ./s
sudo chmod -x s

# Spustenie skriptu
./skript.sh              # Vyžaduje +x
bash skript.sh           # Nevyžaduje +x
source skript.sh         # Spustí v aktuálnom shelle
. skript.sh              # Alternatívny syntax pre source
```

### Práca s PATH

```bash
# Výpis PATH
echo $PATH

# Kontrola, kde sa nachádza príkaz
which ls
whereis ls

# Pridanie adresára do PATH
export PATH="/home/user:$PATH"
```

### Debugging bash skriptov

```bash
# Spustiť s debug výpisom
bash -x script.sh        # Vypíše každý vykonávaný príkaz
bash -xv script.sh       # Vypíše aj načítané riadky

# V skripte
set -x                   # Zapnutie debug módu
set +x                   # Vypnutie debug módu

set -e                   # Exit pri prvej chybe
set +e                   # Vypnutie exit pri chybe
```

### Špeciálne premenné a citovanie

```bash
# Escaping
echo ' \\  \\'           # Výstup:  \\  \\
echo " \\  \\"           # Výstup:  \  \
echo '" \\  \\"'         # Výstup: " \\  \\"

# Substitúcia v substitúcii
$(echo duti | tr u a | tr i e)   # date
```

### Práca s procesmi

```bash
# Zoznam procesov
ps                       # Procesy aktuálneho shellu
ps aux                   # Všetky procesy
top                      # Interaktívny monitor

# Background/foreground
command &                # Spustí na pozadí
jobs                     # Zoznam background jobov
fg                       # Presuň posledný job do popredia
fg %1                    # Presuň job 1 do popredia

# PID aktuálneho shellu
echo $$

# Návratový kód
echo $?
```

### Kombinácia príkazov

```bash
# Rúra (pipe)
ls -l | grep ".txt"
cat file | wc -l

# Logické operátory
command1 && command2     # command2 len ak command1 uspel
command1 || command2     # command2 len ak command1 zlyhal

# Sekvenčné vykonanie
command1 ; command2      # Vykoná oba nezávisle
```

### Užitočné aliasy

```bash
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Vymazanie aliasu
unalias ll
```

---

## Časté chyby a ich riešenia

### 1. Medzery v priradeniach

**Chyba:**
```bash
PREMENNA = "hodnota"     # CHYBA!
```

**Správne:**
```bash
PREMENNA="hodnota"       # OK
```

### 2. Citovanie premenných v poli

**Problém:**
```bash
zoznam=(jeden dva "tri styri")
echo ${#zoznam[@]}       # 3

zoznam2=(${zoznam[@]} pat)
echo ${#zoznam2[@]}      # 5 ("tri styri" sa rozpadlo)
```

**Riešenie:**
```bash
zoznam2=("${zoznam[@]}" pat)
echo ${#zoznam2[@]}      # 4 ("tri styri" zostalo spolu)
```

### 3. Test vs [[]]

```bash
# [ ] - POSIX kompatibilné
if [ "$var" = "hodnota" ]; then
    echo "rovnake"
fi

# [[ ]] - Bash rozšírenie, lepšie funkcie
if [[ "$var" == "hodnota" ]]; then
    echo "rovnake"
fi

# [[ ]] podporuje pattern matching
if [[ "$var" == *.txt ]]; then
    echo "textovy subor"
fi
```

### 4. Aritmetika

```bash
# Chyba - reťazcová operácia
i=$i+1                   # CHYBA! i="5+1"

# Správne
i=$((i+1))               # OK
((i++))                  # OK
let i=i+1                # OK
```

### 5. Command substitution vs eval

```bash
# Command substitution - vykoná a vráti výstup
result=$(ls -l)

# Eval - vykoná string ako príkaz
eval "ls -l"
```

---

## Príklady z reálneho testu

### Príklad 1: Počet riadkov

```bash
# Koľko riadkov má súbor?
wc -l subor              # Vypíše počet a názov
wc -l < subor            # Vypíše len počet
```

### Príklad 2: Práca s poľami

```bash
zoznam=(jeden dva tri styri pat "sest cele sedem")

# Vypíše: 6
echo ${#zoznam[@]}

# Bez úvodzoviek - rozpadne sa na 8
zoznam2=(${zoznam[@]} osem)
echo ${#zoznam2[@]}      # 8

# S úvodzovkami - zostane 7
zoznam2=("${zoznam[@]}" osem)
echo ${#zoznam2[@]}      # 7
```

### Príklad 3: For cyklus s seq

```bash
for i in $(seq 10 20); do
    echo "cislo $i" > subor
    echo i                # Vypíše literal "i", nie hodnotu!
done
```

**Výsledok:**
- Súbor `subor` bude obsahovať len: `cislo 20`
- Na obrazovku sa vypíše 11x `i`

### Príklad 4: Presmerovanie

```bash
# V aktuálnom adresári je len subor1
ls subor2 > vypis.txt; ls >> vypis.txt 2>/dev/null

# Výsledok:
# - Vytvorí sa vypis.txt
# - Prvý ls zapíše chybu do vypis.txt
# - Druhý ls pripojí obsah (subor1 a vypis.txt)
# - Na obrazovku sa nevypíše nič
```

---

## Dôležité príkazy na zapamätanie

### sed

```bash
# Nahradenie
sed 's/pattern/replacement/' file
sed 's/pattern/replacement/g' file       # Všetky výskyty
sed 's/pattern/replacement/I' file       # Ignore case

# Zmazanie
sed '/pattern/d' file                    # Zmaže riadky s pattern
sed '/^$/d' file                         # Zmaže prázdne riadky

# Výber
sed -n '5,10p' file                      # Vypíše riadky 5-10
sed -n '/pattern/p' file                 # Vypíše riadky s pattern
sed '$!d' file                           # Posledný riadok
sed '1~2d' file                          # Zmaže každý prvý riadok (ponechá párne)
sed -n 'n;p' file                        # Vypíše každý druhý riadok (párne)
```

### awk

```bash
# Výpis stĺpcov
awk '{print $1}' file                    # Prvý stĺpec
awk '{print $NF}' file                   # Posledný stĺpec
awk '{print $0}' file                    # Celý riadok

# Delimiter
awk -F":" '{print $1}' /etc/passwd

# Podmienky
awk 'NR % 2 == 0 {print $0}' file       # Párne riadky
awk 'NR==1 {print $0}' file             # Prvý riadok
awk 'NF>3 {print $0}' file              # Riadky s viac ako 3 poľami

# Aritmetika
awk '{sum+=$2} END {print sum}' file
awk '{sum+=$2} END {print sum/NR}' file # Priemer
```

### grep

```bash
# Základné
grep "pattern" file
grep -i "pattern" file                   # Ignore case
grep -v "pattern" file                   # Invertuj (všetko okrem)
grep -n "pattern" file                   # Čísla riadkov

# Regulárne výrazy
grep -E '\<word\>' file                  # Celé slovo
grep -E '^pattern' file                  # Začiatok riadku
grep -E 'pattern$' file                  # Koniec riadku
grep -E '[a-z]{5,}' file                # 5 alebo viac malých písmen
grep -E 'A|B' file                      # A alebo B
```

---

## Záver

Tento dokument obsahuje praktické príklady zo skutočných cvičení. Najdôležitejšie je:

1. **Precvičiť skripty** - napísať a spustiť každý príklad
2. **Pochopiť citovanie** - rozdiel medzi `"..."`, `'...'`, a bez úvodzoviek
3. **Ovládať polia** - rozbaľovanie s/bez úvodzoviek
4. **Poznať presmerovanie** - >, >>, 2>, 2>&1, |
5. **Rozumieť sed/awk/grep** - základné vzory a použitie

**Veľa úspechu na teste!** 🎯
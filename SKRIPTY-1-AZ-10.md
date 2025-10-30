# SKRIPTY 1 až 10 - Operačné Systémy - MARKDOWN

Všetky 10 bash skriptov s variantami, výstupmi a vysvetleniami.

---

## 📝 SKRIPT 1: Počítanie Súborov v Adresároch

### Verzia 1: Základná ✅

```bash
#!/bin/bash

for d in /public/prednasky /public/ucebnove /public/priklady; do
    echo -n "$d: "
    ls "$d" | wc -l
done
```

**Výstup:**
```
/public/prednasky: 42
/public/ucebnove: 18
/public/priklady: 27
```

---

### Verzia 2: C-štýl cyklus

```bash
for (( i=0 ; i<5 ; i++ )); do
    echo "opakovanie $i"
done
```

**Výstup:**
```
opakovanie 0
opakovanie 1
opakovanie 2
opakovanie 3
opakovanie 4
```

---

### Verzia 3: CHYBA - bez úvodzoviek ❌

```bash
for d in /public/prednasky /public/ucebnove /public/priklady; do
    echo -n $d        # CHYBA!
    ls $d | wc -l
done
```

**Problém:** Ak adresár má medzery:
```
/public/my dir/file
```

**Riešenie:** Vždy `"$d"`

---

## 📝 SKRIPT 2: Premenné a Citovanie

### Verzia 1: Bez úvodzoviek

```bash
veta="ahoj svet"
pocet=3

while [ $pocet -gt 0 ]; do
    echo $pocet $veta
    ((pocet--))
done
```

**Výstup:**
```
3 ahoj svet
2 ahoj svet
1 ahoj svet
```

---

### Verzia 2: S úvodzovkami

```bash
veta="ahoj svet"
pocet=3

while [ $pocet -gt 0 ]; do
    echo "$pocet $veta"
    ((pocet--))
done
```

**Výstup:** (rovnaký)

---

### Verzia 3: Literál

```bash
veta="ahoj svet"

echo '$veta'    # Literál - bez rozbálenia
```

**Výstup:**
```
$veta
```

---

## 📝 SKRIPT 3: Premenovanie Súborov

### Verzia 1: Základná

```bash
#!/bin/bash

for f in [A-Z]*; do
    echo "Premel: $f → $(echo "$f" | tr A-Z a-z)"
    mv -i "$f" "$(echo "$f" | tr A-Z a-z)"
done
```

**Výstup:**
```
Premel: FILE.txt → file.txt
Premel: DATA.doc → data.doc
```

---

### Verzia 2: S kontrolou

```bash
#!/bin/bash

for f in [A-Z]*; do
    newname="$(echo "$f" | tr A-Z a-z)"
    if [ "$f" != "$newname" ]; then
        echo "Premel: $f → $newname"
        mv -i "$f" "$newname"
    fi
done
```

---

## 📝 SKRIPT 4: Spracovanie Argumentov

### Verzia 1: Help

```bash
#!/bin/bash

help="Help: $0 arg1 arg2 arg3"

if [ "$#" == "0" ]; then
    echo "$help"
    exit 1
fi

echo "Argumenty: $@"
```

**Spustenie:**
```bash
$ ./script.sh
Help: ./script.sh arg1 arg2 arg3

$ ./script.sh ahoj svet
Argumenty: ahoj svet
```

---

### Verzia 2: case Statement

```bash
#!/bin/bash

help="Help: $0 arg1 arg2 arg3"

if [ "$#" == "0" ]; then
    echo "$help"
    exit 1
fi

while (( "$#" )); do
    case "$1" in
        -d|-D)
            echo "DEBUG MÓD"
            shift
            break
            ;;
        -h)
            echo "$help"
            exit 0
            ;;
        -*)
            echo "Neznamy prepinac: $1"
            exit 1
            ;;
        *)
            echo "Argument: $1"
            ;;
    esac
    shift
done
```

**Spustenie:**
```bash
$ ./script.sh -d ahoj svet
DEBUG MÓD
Argument: ahoj
Argument: svet
```

---

## 📝 SKRIPT 5: Práca s Poliami

### Verzia 1: Základné

```bash
#!/bin/bash

zoznam=(jeden dva tri styri pat "sest cele sedem")

echo "Prvok 0: ${zoznam[0]}"
echo "Prvok 2: ${zoznam[2]}"
echo "Dĺžka [5]: ${#zoznam[5]}"
echo "Počet: ${#zoznam[@]}"
echo "Všetky: ${zoznam[@]}"
echo "Slice [2:4]: ${zoznam[@]:2:2}"
```

**Výstup:**
```
Prvok 0: jeden
Prvok 2: tri
Dĺžka [5]: 15
Počet: 6
Všetky: jeden dva tri styri pat sest cele sedem
Slice [2:4]: tri styri
```

---

### Verzia 2: CHYBA - bez úvodzoviek ❌

```bash
zoznam=(jeden dva tri styri pat "sest cele sedem")

# CHYBA!
zoznam2=(${zoznam[@]} osem)
echo ${#zoznam2[@]}     # 8 - ZLAK!
```

**Prečo?** `"sest cele sedem"` sa rozpadne!

**Správne:**
```bash
zoznam2=("${zoznam[@]}" osem)
echo ${#zoznam2[@]}     # 7 - OK!
```

---

### Verzia 3: Iterácia

```bash
echo "=== BEZ úvodzoviek (ZLÉ) ==="
for prvok in ${zoznam[@]}; do
    echo "[$prvok]"
done

echo ""
echo "=== S úvodzovkami (SPRÁVNE) ==="
for prvok in "${zoznam[@]}"; do
    echo "[$prvok]"
done
```

**Výstup bez úvodzoviek:**
```
[jeden]
[dva]
[tri]
[styri]
[pat]
[sest]     # ROZPADLO SA!
[cele]
[sedem]
```

**Výstup s úvodzovkami:**
```
[jeden]
[dva]
[tri]
[styri]
[pat]
[sest cele sedem]  # SPOLU!
```

---

## 📝 SKRIPT 6: Práca s Cestami

### Verzia 1: dirname a basename

```bash
#!/bin/bash

cesta="/public/ucebnove/seminar_1/vim.txt"

echo "dirname: $(dirname "$cesta")"
echo "basename: $(basename "$cesta")"
echo "dirname dirname: $(dirname "$(dirname "$cesta")")"
echo "basename dirname: $(basename "$(dirname "$cesta")")"
```

**Výstup:**
```
dirname: /public/ucebnove/seminar_1
basename: vim.txt
dirname dirname: /public/ucebnove
basename dirname: seminar_1
```

---

### Verzia 2: Bez Prípony

```bash
#!/bin/bash

cesta="/public/ucebnove/seminar_1/vim.txt"

# Bez prípony
basename "$cesta" .txt      # vim
basename -s ".txt" "$cesta" # vim
```

---

## 📝 SKRIPT 7: IFS - Internal Field Separator

### Verzia 1: Problém

```bash
#!/bin/bash

echo "=== BEZ nastavenia IFS ==="
for f in $(ls -l | head -3); do
    echo "[$f]"
done
```

**Výstup:**
```
[total]
[512]
[-rw-r--r--]  # ROZDELILO SA!
[1]
```

---

### Verzia 2: Riešenie

```bash
#!/bin/bash

echo "=== S IFS = newline ==="
IFS=$'\n'
for f in $(ls -l | head -3); do
    echo "[$f]"
done
```

**Výstup:**
```
[total 512]
[-rw-r--r-- 1 user group 1024 Oct 15 file.txt]
[drwxr-xr-x 2 user group  512 Oct 15 dir]
```

---

## 📝 SKRIPT 8: source vs exec

### Verzia 1: source

```bash
#!/bin/bash

echo "Spustam: source script2.sh"
source ./script2.sh
echo "Späť v hlavnom"
echo "var1 = $var1"
```

**script2.sh:**
```bash
var1="z_script2"
echo "Som v script2, var1 = $var1"
```

**Výstup:**
```
Spustam: source script2.sh
Som v script2, var1 = z_script2
Späť v hlavnom
var1 = z_script2
```

---

### Verzia 2: exec

```bash
#!/bin/bash

echo "Spustam: exec script2.sh"
exec ./script2.sh
echo "TOTO SA NIKDY NEVYKONNÁ!"
```

**Výstup:**
```
Spustam: exec script2.sh
Som v script2
```

---

## 📝 SKRIPT 9: PID a export

### Verzia 1: export

```bash
#!/bin/bash

echo "Hlavný skript PID: $$"

var_local="len v tomto"
export var_global="viditeľná"

bash -c "echo 'Podproces vidí: var_global = $var_global'"
bash -c "echo 'Podproces vidí: var_local = $var_local'"
```

**Výstup:**
```
Hlavný skript PID: 12345
Podproces vidí: var_global = viditeľná
Podproces vidí: var_local = 
```

---

### Verzia 2: env vs set

```bash
#!/bin/bash

var_shell="iba v shelli"
export var_env="v prostredí"

echo "=== env (prostredie) ==="
env | grep "var_"

echo ""
echo "=== set (všetky premenné) ==="
set | grep "var_"
```

**Výstup:**
```
=== env ===
var_env=v prostredí

=== set ===
var_shell=iba v shelli
var_env=v prostredí
```

---

## 📝 SKRIPT 10: Podmienky a Testy

### Verzia 1: [ ] vs [[ ]]

```bash
#!/bin/bash

# POSIX
if [ -f "subor.txt" ]; then
    echo "Súbor existuje ([ ])"
fi

# Bash
if [[ -f "subor.txt" ]]; then
    echo "Súbor existuje ([[ ]])"
fi
```

---

### Verzia 2: Existencia Premennej

```bash
#!/bin/bash

declare myvar="ahoj"

[[ -v myvar ]] && echo "myvar existuje"
[[ -v nonexistent ]] || echo "nonexistent neexistuje"

# Alebo
[[ -z "${myvar}" ]] && echo "prázdna"
[[ -n "${myvar}" ]] && echo "nie prázdna"
```

---

### Verzia 3: Aritmetika

```bash
#!/bin/bash

x=5
y=10

# Slovne
if [ "$x" -lt "$y" ]; then
    echo "$x je menšie ako $y"
fi

# V (( ))
if (( x < y )); then
    echo "$x je menšie ako $y"
fi

# Aritmetika
if (( (x + y) > 14 )); then
    echo "Suma > 14"
fi
```

---

## 🔧 Tabuľka Časých Chýb

| Chyba | Nesprávne | Správne | Dôvod |
|-------|----------|---------|--------|
| Priradenie | `VAR = value` | `VAR="value"` | Bez medzier! |
| Pole | `${array[@]}` v slučke | `"${array[@]}"` | Medzery! |
| Aritmetika | `i=$i+1` | `i=$((i+1))` | Reťazec! |
| test existencia | `-e var` | `[[ -v var ]]` | Nie je to to isté |
| globbing | `ls $pattern` | `ls "$pattern"` | Expansion! |
| tu-dokument | `EOF` bez `EOF` | `EOF` na konci | Musi byt zavrety |
| pipe | `var=0 \| while` | `while; var zostane` | Subshell! |
| shift | bez shift v while | `shift` na konci | Nekonečna slučka! |
| exec | bez exit | `exit` za exec | Nikdy sa nevykonná |
| case | bez `;;` | `;;` po každom | Musí byt zatvorené |
| IFS | bez úvodzoviek | `"${array[@]}"` | Rozdelenie! |

---

## 📚 Dôležité Premeny

```bash
# Aritmetika
$(( 5 + 3 ))
$(( i++ ))
$(( i += 5 ))

# Command substitution
$(command)      # Moderný
`command`       # Starý

# Escape
\"              # Úvodzovka
\\              # Backslash
\$              # Dollar
\n              # Newline

# Premenné
${var}
${var:-default}
${var:0:5}
${var//old/new}
${#var}

# Testy
[ -f file ]
[ -d dir ]
[ -z "$var" ]
[ "$a" = "$b" ]
[ "$a" -lt "$b" ]
```

---

## ✅ Finálny Checklist

- [ ] Všetky premenné sú v `"úvodzovkách"`
- [ ] Všetky polia sú v `"${array[@]}"`
- [ ] Aritmetika je v `$(( ))`
- [ ] Príkazy sú v `$( )`
- [ ] case má `;;`
- [ ] for/while majú `do` a `done`
- [ ] if/then majú `fi`
- [ ] Bez medzier pri `=`
- [ ] shift v slučkách
- [ ] export pre podprocesy

---

**Veľa Úspechu!** 🚀
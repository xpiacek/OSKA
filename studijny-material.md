# Operačné Systémy - Študijný Materiál

---

## 1. Základy Operačných Systémov

### Čo je operačný systém?

**Operačný systém (OS)** je softvér, ktorý:
- **Abstrahuje hardvér** - robí z komplikovaného HW jednoduché a konzistentné rozhranie
- **Rozšírený stroj (Extended Machine)** - pridáva abstrakcie ako súborový systém, procesy, virtuálnu pamäť
- **Virtuálny stroj** - HW + OS spolu tvoria virtuálny stroj
- **Manažér prostriedkov** - riadi prístup k zdrojom striedaním v čase a priestore

**OS v užšom zmysle**: abstrakcia nad HW poskytujúca jednotné rozhranie  
**OS v širšom zmysle**: celé softvérové vybavenie výpočtového systému

### História OS

**1. generácia (1940-1955)**
- Plugboard, vákuové trubice, mechanické relé
- Programovanie v strojovom kóde
- Žiadny OS v pravom zmysle

**2. generácia (1955-1965)**
- Tranzistory
- Mainframe systémy
- Batch systémy - úlohy v dávkach
- Multiprogramming
- Asemblér a Fortran

**3. generácia (1965-1980)**
- Integrované obvody (ICs)
- Multiprogramming - viac úloh v pamäti
- Timesharing - viac používateľov súčasne
- Terminály a minicomputery
- UNIX, MULTICS, POSIX

**4. generácia (1980-)**
- LSI (Large Scale Integration)
- Osobné počítače (PC)
- Grafické rozhrania
- MS-DOS, Windows, Mac OS

**5. generácia (1990-)**
- VLSI (Very Large Scale Integration)
- Mobilné zariadenia, smartfóny
- Android, iOS
- Bezdrôtové siete

### Klasifikácia OS

**Podľa určenia:**
- **Mainframe OS** - spracovanie veľkého počtu úloh, vysoká stabilita
- **Server OS** - prístup pre veľa používateľov, zdieľanie zdrojov
- **PC OS** - jeden používateľ, všeobecné použitie
- **Embedded OS** - riadenie zariadení bez zásahu používateľa
- **Real-time OS** - garantovaná doba odozvy (Soft RT, Hard RT)

**Podľa štruktúry:**
- **Monolitické** - jadro ako jeden veľký blok, nízka latencia, náchylné na chyby
- **Microkernel** - minimum v jadre, služby ako procesy, vyššia stabilita, vyššia latencia

### POSIX

**POSIX** je štandard definujúci:
- Minimálne rozhranie OS
- Sadu systémových volaní
- Formu systémových volaní
- Zabezpečuje prenositeľnosť aplikácií medzi Unix-like systémami

---

## 2. Hardware výpočtového systému

### CPU (Central Processing Unit)

**Procesor** vykonáva program inštrukciu za inštrukciou. Obsahuje:
- **Množinu inštrukcií**
- **Registre** (všeobecné, PC - Program Counter, SP - Stack Pointer, PSW - Program Status Word)
- **Režimy CPU** (kernel mode, user mode)

**Režim CPU** určuje, akú podmnožinu inštrukcií možno vykonávať:
- **Kernel mode** - privilegovaný režim, prístup ku všetkým inštrukciám
- **User mode** - neprivilegovaný režim, obmedzený prístup
- Prechod medzi režimami cez **TRAP** inštrukciu (systémové volanie)

**Multithreading (Hyperthreading)**:
- Procesor ukladá kontexty dvoch vlákien naraz
- Rýchle prepnutie pri blokovaní vlákna (ns)

**Multicore**:
- Viac jadier = skutočný paralelizmus
- Každé jadro vykonáva samostatný proces/vlákno

**Prerušovací podsystém**:
- Umožňuje prerušiť sekvenčné vykonávanie
- CPU prepíše PC na adresu prerušovacieho podprogramu
- OS odloží kontext a spracuje prerušenie

### Pamäť

**Hierarchia pamäte** (rýchlosť vs kapacita vs cena):
1. **Registre** - 1 ns, ~1 kB, priamo v CPU
2. **Cache (L1, L2, L3)** - 2 ns, ~8 MB
3. **RAM (Main Memory)** - 10 ns, ~8 GB
4. **Disk (HDD, SSD)** - 10 ms, ~1 TB

---

## 3. Procesy a Vlákna

### Program vs Proces

**Program** = súbor s inštrukciami na dosiahnutie cieľa

**Proces** = vykonávaný program s:
- Vyhradeným adresným priestorom
- Stavom (registre, PC, zásobník)
- Pridelenými I/O zariadeniami
- Otvorenými súbormi

### Process Model

Proces ako **abstrakcia**:
- Izolovaný v neprivilegovanom režime
- Chránený OS
- Nemusí sa deliť o prostriedky
- OS riadi striedanie na CPU
- Umožňuje **Multiprogramming**

**Multiprogramming** = udržiavanie viacerých procesov v RAM:
- Vyššia utilizácia CPU
- Pri čakaní na I/O sa prepne na iný proces
- **Utilizácia**: \( U = 1 - p^n \), kde \( p \) = pravdepodobnosť čakania na I/O, \( n \) = počet procesov

### Process Control Block (PCB)

**PCB** obsahuje:
- Stav procesu
- Program Counter
- Registre CPU
- Informácie o plánovači
- Informácie o pamäti
- Zoznam otvorených súborov
- Zoznam I/O zariadení

**Process Table** = tabuľka všetkých PCB

**Process Tree** (Unix):
- Procesy majú vzťah rodič-dieťa
- Rodič si pamätá PID detí

### Stavy procesu

1. **Running** - vykonávaný na CPU
2. **Ready** - pripravený na vykonávanie, v poradovníku
3. **Blocked** - blokovaný čakaním na udalosť

**Prechody:**
- Running → Blocked (proces čaká na I/O)
- Running → Ready (proces je preplánovaný)
- Ready → Running (proces je naplánovaný)
- Blocked → Ready (udalosť prišla)

### Životný cyklus procesu

**Vznik:**
- Pri štarte OS
- Systémovým volaním (`fork()`, `exec()`, `CreateProcess()`)
- Požiadavkou používateľa
- Batch jobom

**Unix systémové volania:**
- **fork()** - vytvorí kópiu procesu (copy-on-write)
- **exec()** - nahradí program procesu novým programom
- **exit()** - dobrovoľné ukončenie

**Zánik:**
- Dobrovoľný (štandardné ukončenie, ukončenie s chybou)
- Nedobrovoľný (fatálna chyba, kill)

### Vlákna (Threads)

**Vlákno** = vykonávaný program s vlastným:
- Registrami
- Program Counter
- Zásobníkom

**Proces** = jeden alebo viac thread-ov s pridelenými prostriedkami

**Výhody thread-ov:**
- Rozdelenie na menšie sekvenčné úlohy
- Komunikácia cez spoločnú pamäť
- Lacnejšie preplánovanie (nemenná pamäť programu/dát)
- Podpora multithreading CPU

**User threads vs Kernel threads:**
- **User threads**: manažované knižnicou, rýchle operácie, ale OS ich nevidí
- **Kernel threads**: manažované OS, pomalšie operácie, ale plná podpora OS

**POSIX threads:**
```c
pthread_create(&thread, NULL, function, arg);
pthread_join(thread, NULL);
```

---

## 4. Plánovanie procesov

### Scheduler (Plánovač)

**Scheduler** usporadúva pripravené procesy podľa kritérií.

**Kedy plánovať?**
- Proces skončil
- Proces vytvoril nový proces
- Proces je blokovaný
- HW prerušenie (I/O, časovač)

### Preemptive vs Non-preemptive

**Preemptive** - plánovač môže prerušiť vykonávaný proces  
**Non-preemptive** - proces sa môže nahradiť len dobrovoľne alebo pri blokovaní

### Typy procesov

- **CPU-bound** - výpočtovo intenzívne
- **I/O-bound** - I/O intenzívne

### Kritériá plánovania

**Spoločné:**
- Férovosť
- Vynucovanie politiky
- Vyváženosť

**Batch:**
- **Throughput** - max počet úloh/čas
- **Turnaround time** - doba vykonania úlohy
- **CPU utilizácia** - vyťaženie CPU

**Interaktívne:**
- **Doba odozvy** - rýchle spracovanie požiadaviek
- **Proporčnosť** - spĺňa očakávania

**Real-time:**
- **Dodržanie doby odozvy**
- **Predvídateľnosť**

### Plánovacie algoritmy

**Non-preemptive:**

1. **FCFS (First Come First Served)**
   - Jednoduchý, férový
   - Proces vykonávaný kým nie je blokovaný
   - CPU-bound procesy zvýhodnené

2. **SJF (Shortest Job First)**
   - Proces s najkratším časom má prednosť
   - Skracuje turnaround time
   - Dlhé procesy môžu hladovať

**Preemptive:**

1. **Shortest Remaining Time**
   - Preemptívna verzia SJF
   - Proces s najkratším zostávajúcim časom

2. **Round-robin**
   - Každý proces dostane časové quantum
   - Po vypršaní quanta je zaradený na koniec radu
   - Problém: správne nastavenie quanta (malé = vyššia réžia, veľké = horšia odozva)

3. **Prioritné plánovanie**
   - Procesy majú rôzne priority
   - Vyššia priorita = vyššie v rade
   - Priorita sa môže dynamicky meniť
   - Procesy s rovnakou prioritou často plánované Round-robin

---

## 5. IPC - Komunikácia procesov

### Synchronizácia a Vzájomné vylučovanie

**IPC (Interprocess Communication)** zahŕňa:
- Synchronizáciu
- Vzájomné vylučovanie (Mutual Exclusion)
- Výmenu správ (Message Passing)

**Synchronizácia procesov** = riadené usporiadanie spolupracujúcich procesov

**Vzájomné vylučovanie** = limitovanie a riadenie prístupu do kritickej oblasti

### Kritická oblasť (Critical Region)

**Kritická oblasť** = časť programu pristupujúca ku zdieľanému prostriedku

**Race conditions** = podmienky súperenia pri súčasnom prístupe k zdieľanému prostriedku

**Podmienky riešenia vzájomného vylučovania:**
1. V KO môže byť najviac 1 proces
2. Rýchlosť a počet CPU sa nesmie brať do úvahy
3. Proces mimo KO nesmie brániť inému procesu vstúpiť do KO
4. Čas čakania na vstup do KO musí byť konečný

### Riešenia vzájomného vylučovania

**1. Zakázanie prerušení:**
```c
disable_interrupts();
critical_region();
enable_interrupts();
```
- Problémy: globálny vplyv, len jednojadrové CPU

**2. Busy Waiting - Lock Variable:**
```c
while (lock != 0) { ; }
lock = 1;
critical_region();
lock = 0;
```
- Problém: Race condition pri testovaní a nastavovaní lock

**3. Busy Waiting - Strict Alternation:**
```c
// Proces A
while (turn != A) { ; }
critical_region();
turn = B;

// Proces B
while (turn != B) { ; }
critical_region();
turn = A;
```
- Problém: proces mimo KO bráni inému procesu vstúpiť (porušuje 3. podmienku)

**4. Peterson algoritmus (2 procesy):**
```c
// Proces A
inA = true;
turn = A;
while (turn == A && inB == true) { ; }
critical_region();
inA = false;
```
- Funguje správne, ale stále busy waiting

**5. TSL (Test and Set Lock) inštrukcia:**
```assembly
TSL REG, LOCK  ; Atomicky skopíruj LOCK do REG a nastav LOCK na 1
```
- HW podpora, atomická operácia

### Semafór

**Semafór** = kladné celé číslo s radom blokovaných procesov

**Operácie:**
- **init(s)** - nastaví počiatočnú hodnotu
- **down()** - ak \( s = 0 \), blokuje proces, inak \( s-- \)
- **up()** - ak rad neprázdny, prebudí proces, inak \( s++ \)

**Binárny semafór**: hodnota 0 alebo 1, vhodný pre vzájomné vylučovanie

**Použitie:**
```c
tSem lock = 1;

void process() {
    down(&lock);
    critical_region();
    up(&lock);
}
```

**Semafór pre synchronizáciu:**
```c
tSem consume = 0, produce = 1;

void procesA() {
    produce.down();
    produce_data();
    consume.up();
}

void procesB() {
    consume.down();
    consume_data();
    produce.up();
}
```

### Mutex

**Mutex** = softvérový ekvivalent semafóru pre vlákna

**POSIX Mutex:**
```c
pthread_mutex_t mutex;
pthread_mutex_init(&mutex, NULL);
pthread_mutex_lock(&mutex);
critical_region();
pthread_mutex_unlock(&mutex);
pthread_mutex_destroy(&mutex);
```

### Condition Variables

**Condition Variable** = nástroj na podmienené riadenie vstupu do KO s uspaním vlákna

**POSIX Condition Variables:**
```c
pthread_cond_t cond;
pthread_mutex_t mutex;

// Čakanie
pthread_mutex_lock(&mutex);
while (!podmienka)
    pthread_cond_wait(&cond, &mutex);
pthread_mutex_unlock(&mutex);

// Signalizácia
pthread_cond_signal(&cond);  // Prebudí jedno vlákno
pthread_cond_broadcast(&cond);  // Prebudí všetky vlákna
```

### Monitor

**Monitor** = rozšírená syntax jazyka definujúca KO ako objekt
- Java: `synchronized` keyword
- Kompilátor zabezpečí vzájomné vylučovanie

### Message Passing

**Posielanie správ** = komunikácia procesov v distribuovaných systémoch

**Systémové volania:**
- `send(Destination, Message)`
- `receive(Source, Message)`

**Problémy:**
- Strata správy (Ack)
- Opakované prijatie (identifikátor)
- Zmena poradia (identifikátor)
- Autentickosť (šifrovanie)

---

## 6. Klasické IPC problémy

### Producer-Consumer (Producent-Konzument)

**Problém**: Rad veľkosti N, producent pridáva, konzument odoberá

**Riešenie semafórmi:**
```c
#define N 100
tSem mutex = 1, empty = 0, full = N;

void producer() {
    while (true) {
        produce_item(&item);
        down(&full);
        down(&mutex);
        push_item(&item);
        up(&mutex);
        up(&empty);
    }
}

void consumer() {
    while (true) {
        down(&empty);
        down(&mutex);
        pop_item(&item);
        up(&mutex);
        up(&full);
        consume_item(&item);
    }
}
```

### Dining Philosophers (Večerajúci filozofi)

**Problém**: 5 filozofov, 5 vidličiek, potrebné 2 vidličky na jedenie

**Naivné riešenie (deadlock):**
```c
take_fork(id);
take_fork((id+1) % N);
eat();
put_fork(id);
put_fork((id+1) % N);
```

**Správne riešenie semafórmi:**
```c
tSem mutex = 1;
tSem pSem[N] = {0};
int state[N] = {THINKING};

void take_forks(int id) {
    down(&mutex);
    state[id] = HUNGRY;
    test(id);
    up(&mutex);
    down(&pSem[id]);
}

void test(int id) {
    if (state[id] == HUNGRY && 
        state[LEFT] != EATING && 
        state[RIGHT] != EATING) {
        state[id] = EATING;
        up(&pSem[id]);
    }
}
```

### Readers-Writers (Čitatelia-Zapisovatelia)

**Problém**: Viac čitateľov môže čítať súčasne, ale len jeden zapisovateľ

**Riešenie semafórmi (uprednostňuje čitateľov):**
```c
tSem mutex = 1, db = 1;
int rc = 0;  // reader count

void writer() {
    down(&db);
    write_db();
    up(&db);
}

void reader() {
    down(&mutex);
    if (++rc == 1) down(&db);
    up(&mutex);
    
    read_db();
    
    down(&mutex);
    if (--rc == 0) up(&db);
    up(&mutex);
}
```

---

## 7. UNIX a Shell

### Súborový systém

**Stromová štruktúra:**
- Jeden koreň: `/`
- Oddeľovač adresárov: `/`
- `.` = aktuálny adresár
- `..` = rodičovský adresár

**Typy súborov:**
- **Obyčajný súbor** - postupnosť bajtov
- **Adresár** - špeciálny súbor so zoznamom súborov
- **Zariadenie** - `/dev/`
- **Symbolická linka** - referencia na iný súbor

**Cesta:**
- **Absolútna**: začína `/` (napr. `/home/user/file.txt`)
- **Relatívna**: začína od aktuálneho adresára (napr. `../file.txt`)

### Prístupové práva

**Formát**: `rwxrwxrwx`
- `r` = read (4), `w` = write (2), `x` = execute (1)
- Prvá triáda: vlastník
- Druhá triáda: skupina
- Tretia triáda: ostatní

**Príklad**: `chmod 755 file` → `rwxr-xr-x`

### Shell

**Shell** = príkazový riadok pre prístup k OS
- Bash (Bourne-Again SHell)
- Interpret príkazov a skriptov
- Prispôsobenie prostredia (alias, history)

**Typy shell-ov:**
- Original Unix shell (1969)
- Bourne shell - sh (1979)
- C shell - csh (1978)
- Bash (1989)
- Korn shell - ksh (1986)

### Štandardné vstupy/výstupy

**Štandardný vstup (stdin)** - fd 0  
**Štandardný výstup (stdout)** - fd 1  
**Štandardný chybový výstup (stderr)** - fd 2

### Presmerovanie

- `>` - presmeruj stdout do súboru (prepíše)
- `>>` - presmeruj stdout do súboru (pripojí)
- `<` - presmeruj súbor na stdin
- `2>` - presmeruj stderr do súboru
- `2>&1` - presmeruj stderr na stdout
- `|` - rúra (pipe) - stdout jedného procesu na stdin druhého

**Príklady:**
```bash
ls -l > output.txt
cat file.txt >> output.txt
wc -l < input.txt
command 2> errors.txt
command > output.txt 2>&1
ls | grep ".txt"
```

---

## 8. Bash scripting

### Premenné

```bash
PREMENNA="hodnota"
echo $PREMENNA
echo ${PREMENNA}

# Špeciálne premenné
$0  # názov skriptu
$1, $2, ...  # parametre
$@  # všetky parametre ako pole
$#  # počet parametrov
$?  # návratová hodnota posledného príkazu
$$  # PID aktuálneho procesu
```

### Polia (Arrays)

```bash
zoznam=(jeden dva tri)
echo ${zoznam[0]}  # jeden
echo ${zoznam[@]}  # všetky prvky
echo ${#zoznam[@]}  # počet prvkov
```

### Podmienky

```bash
if [ podmienka ]; then
    prikazy
elif [ podmienka2 ]; then
    prikazy
else
    prikazy
fi

# Test operátory
[ -f file ]  # súbor existuje
[ -d dir ]   # adresár existuje
[ -z "$str" ]  # prázdny reťazec
[ "$a" -eq "$b" ]  # rovnosť čísel
[ "$a" = "$b" ]    # rovnosť reťazcov
```

### Cykly

```bash
# for cyklus
for i in 1 2 3 4 5; do
    echo $i
done

for f in *.txt; do
    echo $f
done

# while cyklus
while [ podmienka ]; do
    prikazy
done
```

### Substitúcia

```bash
vysledok=$(ls -l)  # command substitution
vysledok=`ls -l`   # alternatívny syntax
```

### Aritmetika

```bash
vysledok=$((5 + 3))
vysledok=$((a * b))
```

### Regulárne výrazy

```bash
grep 'pattern' file
sed 's/pattern/replacement/' file
```

---

## 9. Základné UNIX príkazy

### Navigácia
- `cd [adresár]` - zmena adresára
- `pwd` - aktuálny adresár
- `ls [-l] [-a]` - zoznam súborov

### Práca so súbormi
- `cp zdroj cieľ` - kopírovanie
- `mv zdroj cieľ` - presun/premenovanie
- `rm súbor` - zmazanie
- `touch súbor` - vytvorenie/aktualizácia časovej značky
- `mkdir adresár` - vytvorenie adresára
- `rmdir adresár` - zmazanie prázdneho adresára

### Práca s obsahom
- `cat súbor` - výpis obsahu
- `more súbor` - stránkovanie
- `less súbor` - lepšie stránkovanie
- `head [-n N] súbor` - prvých N riadkov
- `tail [-n N] súbor` - posledných N riadkov

### Vyhľadávanie
- `find [cesta] [podmienka]` - hľadanie súborov
- `grep [pattern] [súbor]` - hľadanie v obsahu

### Filtrovanie
- `wc [-l] [-w] [-c]` - počítanie riadkov/slov/znakov
- `sort` - triedenie
- `uniq` - unikátne riadky
- `cut -d":" -f1` - vyberanie stĺpcov
- `tr 'a-z' 'A-Z'` - preklad znakov

### Práva
- `chmod [práva] súbor` - zmena prístupových práv
- `chown [vlastník] súbor` - zmena vlastníka

### Procesy
- `ps` - zoznam procesov
- `kill [PID]` - ukončenie procesu
- `top` - monitorovanie procesov

### Systém
- `man [príkaz]` - manuálová stránka
- `which [príkaz]` - cesta k príkazu
- `whereis [príkaz]` - umiestnenie príkazu

---

## 10. Nástroje pre filtrovanie textu

### grep - vyhľadávanie

```bash
grep "pattern" file.txt
grep -i "pattern" file.txt  # ignore case
grep -v "pattern" file.txt  # invertuj (všetko okrem)
grep -E "pattern" file.txt  # extended regex
grep -n "pattern" file.txt  # čísla riadkov
```

**Regulárne výrazy:**
- `.` - ľubovoľný znak
- `*` - 0 alebo viac predchádzajúceho
- `+` - 1 alebo viac predchádzajúceho
- `?` - 0 alebo 1 predchádzajúceho
- `^` - začiatok riadku
- `$` - koniec riadku
- `[abc]` - jeden zo znakov
- `[a-z]` - rozsah
- `\<` - začiatok slova
- `\>` - koniec slova

### sed - stream editor

```bash
sed 's/pattern/replacement/' file.txt  # nahradí prvý výskyt
sed 's/pattern/replacement/g' file.txt  # nahradí všetky výskyty
sed 's/pattern/replacement/I' file.txt  # ignore case
sed '/pattern/d' file.txt  # zmaže riadky s pattern
sed -n '/pattern/p' file.txt  # vypíše len riadky s pattern
sed -n '5,10p' file.txt  # vypíše riadky 5-10
sed '$!d' file.txt  # posledný riadok
```

### awk - spracovanie tabuliek

```bash
awk '{print $1}' file.txt  # prvý stĺpec
awk '{print $NF}' file.txt  # posledný stĺpec
awk -F":" '{print $1}' /etc/passwd  # delimiter :
awk 'NR==1 {print $0}' file.txt  # prvý riadok
awk 'NR % 2 == 0 {print $0}' file.txt  # párne riadky
awk '{sum+=$2} END {print sum/NR}' file.txt  # priemer druhého stĺpca
```

**Špeciálne premenné:**
- `$1, $2, ...` - stĺpce
- `$0` - celý riadok
- `$NF` - posledný stĺpec
- `NR` - číslo riadku
- `NF` - počet stĺpcov
- `FS` - field separator (delimiter)
- `OFS` - output field separator

### cut - vyberanie stĺpcov

```bash
cut -d":" -f1 /etc/passwd  # prvý stĺpec, delimiter :
cut -d":" -f1,3 /etc/passwd  # stĺpce 1 a 3
```

### sort - triedenie

```bash
sort file.txt  # abecedne
sort -n file.txt  # číselne
sort -r file.txt  # reverse
sort -k2 file.txt  # podľa 2. stĺpca
```

### uniq - unikátne riadky

```bash
sort file.txt | uniq  # odstráni duplicity
uniq -c file.txt  # počet výskytov
```

### wc - počítanie

```bash
wc -l file.txt  # počet riadkov
wc -w file.txt  # počet slov
wc -c file.txt  # počet znakov
```

### Príklady kombinácie

```bash
# 10 najčastejších čísel
sort numbers.txt | uniq -c | sort -nr | head

# Iba používatelia zo /etc/passwd
cut -d":" -f1 /etc/passwd

# Priemerná výška športovcov (meno výška)
awk '{sum+=$2} END {print sum/NR}' sportovci.txt

# Vypísať každý párny riadok
sed -n 'n;p' file.txt
awk 'NR % 2 == 0 {print $0}' file.txt

# Nájsť všetky .txt súbory v /public
find /public -name "*.txt"

# Počet prihlásených používateľov
who | wc -l
```

---

## Tipové koncepty na zapamätanie

### PCB obsahuje:
- Pracovný adresár ✅
- Všeobecné registre CPU ✅
- Celý zásobník procesu ❌ (len ukazovateľ)
- Celé dáta procesu ❌ (len ukazovatele)

### PATH premenná:
- Obsahuje adresáre so spustiteľnými súbormi ✅
- Jej obsah vypíšem: `echo "$PATH"` ✅
- Obsahuje aktuálny adresár ❌ (nie štandardne)
- Je rovnaká pre všetkých ❌
- Nedá sa meniť ❌

### Vlákna:
- Každý thread má vlastný zásobník ✅
- Thread-y môžu komunikovať cez spoločnú pamäť ✅
- User thread vytvoríme rýchlejšie ako kernel thread ✅
- Každý thread má vlastnú pamäť globálnych premenných ❌

### Semafór:
- Počet ukončených down() ≤ počet ukončených up() ✅
- up() a down() nemusia byť v rovnakom počte ✅
- Hodnota semafóru môže byť 0 ✅

### procfs:
- Pripája sa do /proc ✅
- Väčšina súborov má 0 bajtov ✅
- Umožňuje čítať z jadra OS ✅
- Informácie sú nemenné ❌

---

Tento študijný materiál pokrýva kľúčové témy z predmetu Operačné systémy. Pre úspešné absolvovanie testu je potrebné ovládať všetky uvedené koncepty a vedieť ich aplikovať v praktických príkladoch.
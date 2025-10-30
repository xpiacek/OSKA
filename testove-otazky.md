# Operačné Systémy - Testové Otázky

> **Poznámka**: Tento test obsahuje 60 otázok podobných tým z ostrého testu. Správne odpovede sú uvedené na konci dokumentu.

---

## ČASŤ A: Operačné systémy - Teória

### Otázka 1
**Označte pravdivé tvrdenia o zariadeniach a ich registroch:**

- [ ] A) V prípade, že nejakú stránku procesu DMA používa ako úložisko, nemôžem takúto stránku procesu odobrať.
- [ ] B) Stránkové rámy obsahujúce registre zariadení môžu byť vybrané ako obete plánovania i keď sú používané
- [ ] C) Časť adresného priestoru obsahujúci registre zariadení nie je prístupný v user mode.
- [ ] D) Adresný priestor obsahujúci registre zariadení sa nesmie cache-ovať (kešovať).

---

### Otázka 2
**Obsahom Process Control Block-u (PCB) sú:**

- [ ] A) Pracovný adresár
- [ ] B) Všeobecné registre CPU
- [ ] C) Celý zásobník procesu
- [ ] D) Celé data procesu

---

### Otázka 3
**Označte správne tvrdenia o procesoch:**

- [ ] A) Proces vie zasahovať do adresného priestoru ľubovoľného procesu.
- [ ] B) Proces má vyhradený adresný priestor
- [ ] C) Proces je súbor
- [ ] D) Každý proces má vlastnú pamäť globálnych premenných.
- [ ] E) Proces je vykonávaný program s pridelenými prostriedkami
- [ ] F) Proces môžeme vytvoriť systémovým volaním jadra OS.

---

### Otázka 4
**Označte pravdivé tvrdenia o page faults (PF):**

- [ ] A) Počas PF sa hľadaná stránka môže nachádzať v SWAP-e alebo súbore vo FS.
- [ ] B) Jedna inštrukcia vykonávaná na CPU môže byť zdrojom niekoľkých PF.
- [ ] C) Tabuľka stránok je uložená v hlavnej pamäti (RAM)
- [ ] D) Systém je responzívnejší, ak väčšinu stránkových rámov nájdeme pomocou TLB.
- [ ] E) V prípade, že mapovanie stránky na stránkový rám nenájdeme v TLB, hovoríme o soft PF.

---

### Otázka 5
**Označte správne tvrdenia o TLB (Translation Lookaside Buffer):**

- [ ] A) TLB je bežne súčasťou MMU (Memory Management Unit).
- [ ] B) TLB je bežne uložená v hlavnej pamäti výpočtového systému.
- [ ] C) Vyhľadanie stránkového rámu v TLB je rýchlejšie ako v tabuľke stránok.
- [ ] D) Pri odložení kontextu procesu sa bežne odloží obsah TLB.
- [ ] E) TLB obsahuje úplnú tabúľku stránok
- [ ] F) TLB je asociatívna pamäť mapujúca čísla stránok na čísla stránkových rámov.

---

### Otázka 6
**Označte správne prechody medzi stavmi procesu:**

- [ ] A) Running → Ready
- [ ] B) Blocked → Ready
- [ ] C) Running → Blocked
- [ ] D) Blocked → Running

---

### Otázka 7
**Označte správne tvrdenia o vláknach (threads):**

- [ ] A) Stav thread-u je reprezentovaný iba registrami procesora
- [ ] B) Každý thread má vlastnú pamäť globálnych premenných.
- [ ] C) POSIX thread vytvoríme funkciou pthread_create().
- [ ] D) User thread vieme vytvoriť rýchlejšie ako kernel thread
- [ ] E) Operácia join() uvoľní CPU pre ďalší thread.
- [ ] F) Thread-y môžu komunikovať prostredníctvom spoločnej pamäti dát
- [ ] G) Thread je vykonávaný program s pridelenými prostriedkami
- [ ] H) User threads vyžadujú OS s podporou thread-ov
- [ ] I) Každý thread má vlastný zásobník
- [ ] J) Pri plánovaní je lacnejšie prepnúť medzi thread-mi, ktoré sú z rôznych procesov

---

### Otázka 8
**Ktorý signál nemôže byť zachytený, blokovaný alebo ignorovaný?**

- [ ] A) SIGSTOP
- [ ] B) SIGUSR1
- [ ] C) SIGTERM
- [ ] D) SIGINT
- [ ] E) SIGKILL
- [ ] F) SIGALARM

---

### Otázka 9
**Proces môže dobrovoľne zaniknúť:**

- [ ] A) Fatálnou chybou ako je segmentation fault
- [ ] B) Zadaním príkazu kill z shellu
- [ ] C) Štandardným skončením vykonávania programu
- [ ] D) Po vykonaní delenia nulou

---

### Otázka 10
**Označte pravdivé tvrdenia o prostriedkoch:**

- [ ] A) O blokujúcej alokácii hovoríme, ak je proces, po neúspešnej žiadosti o prostriedok uspaný
- [ ] B) Prostriedok je akákoľvek časť výpočtového systému, ktorú vieme alokovať, použiť a uvoľniť
- [ ] C) Zamietnutie žiadosti o prostriedok bez uspania procesu označujeme ako neblokujúca alokácia.
- [ ] D) Preemptívny prostriedok nemôže byť odobratý procesu bez jeho poškodenia.

---

### Otázka 11
**Označte pravdivé tvrdenia o riešení problému uviaznutia:**

- [ ] A) Pštrosí algoritmus je algoritmus používaný ak vznik uviaznutia je kritický.
- [ ] B) Techniky detekcie uviaznutia sú založené na zabezpečení bezpečného stavu výpočtového systému
- [ ] C) Bankárov algoritmus je používaný ako algoritmus vyhýbania sa uviaznutiu.
- [ ] D) Vzájomné vylučovanie je jedným z dôvodov prečo môže k uviaznutiu dôjsť.
- [ ] E) Techniky prevencie uviaznutia sú založené na odstránení aspoň jednej z podmienok vzniku uviaznutia.

---

### Otázka 12
**Označte správne tvrdenia o semaforoch:**

- [ ] A) V prípade, že hodnota semaforu je 0 a vykonáme operáciu down(), semafor čaká v nekonečnom cykle, kým nepríde operácia up().
- [ ] B) Počet vykonávaných operácií down() musí byť vždy menší alebo rovný počtu vykonávaných operácií up().
- [ ] C) Počet operácií up() a down() musí byť vždy rovnaký.
- [ ] D) Operácia init() definuje počiatočný počet ukončených operácií up().
- [ ] E) Semafór je vhodným prostriedkom synchronizácie procesov
- [ ] F) Počet ukončených operácií down() musí byť vždy menší alebo rovný počtu ukončených operácií up().

---

### Otázka 13
**Označte pravdivé tvrdenia o ovládačoch zariadení (device drivers):**

- [ ] A) Úplne celý SW ovládača je súčasťou jadra (kernel-u) OS
- [ ] B) Polling zariadenia je technika práce so zariadením, kde CPU v slučke čaká, kým je zariadenie pripravené.
- [ ] C) Ovládač môže riadiť zariadenia rôznych druhov, v takom prípade je organizovaný ako viacvrstvový SW.
- [ ] D) Ovládače, ktoré používajú polling na čítanie stavu zariadenia, nutne používajú aj interrupt handler
- [ ] E) Ovládač môže riadiť jedno konkrétne zariadenie alebo skupinu zariadení rovnakého druhu.

---

### Otázka 14
**Označte správne tvrdenia o POSIX:**

- [ ] A) POSIX je štandard.
- [ ] B) POSIX je operačný systém.
- [ ] C) POSIX definuje vonkajšie rozhranie OS
- [ ] D) POSIX definuje vnútornú štruktúru OS.

---

### Otázka 15
**Označte správne tvrdenia o vzájomnom vylučovaní procesov:**

- [ ] A) Nástrojom vzájomného vylučovania je monitor
- [ ] B) Nástrojom vzájomného vylučovania sú špeciálne inštrukcie
- [ ] C) Vzájomné vylučovanie je druh synchronizácie procesov
- [ ] D) Nástrojom vzájomného vylučovania je semafor

---

### Otázka 16
**Označte správne tvrdenia o HW výpočtového systému:**

- [ ] A) Viacjadrový procesor umožňuje paralelné spracovanie úloh
- [ ] B) Registre device controller-a sú dostupné použitím špeciálnych inštrukcií
- [ ] C) Registre zariadení (device controller) sú dostupné z adresného priestoru CPU
- [ ] D) Výber použitých registrov CPU riadi kompilátor
- [ ] E) Bežná RAM si udrží dáta po odpojení napájania
- [ ] F) CPU s podporou multithreadingu umožňujú paralelné spracovanie úloh

---

### Otázka 17
**Označte správne tvrdenia o OS:**

- [ ] A) OS v užšom zmysle je abstrakciou nad hardvérom.
- [ ] B) Shell je bežnou súčasťou jadra OS
- [ ] C) OS je softvér
- [ ] D) OS v širšom zmysle je celý výpočtový systém

---

### Otázka 18
**procfs:**

- [ ] A) Viem z neho zistiť verziu OS
- [ ] B) V Linux-e sa štandardne pripája do /proc
- [ ] C) Viem z neho zistiť verziu procesora
- [ ] D) Informácie v ňom sú nemenné
- [ ] E) Väčšina súborov v ňom má veľkosť 0 bajtov
- [ ] F) Je základným nástrojom medziprocesovej komunikácie IPC
- [ ] G) Je adresár na pevnom disku
- [ ] H) Umožňuje získať informácie o aktuálnom procese
- [ ] I) Umožňuje čítať informácie z jadra OS

---

## ČASŤ B: UNIX a Shell

### Otázka 19
**Výsledkom príkazu `wc -l file.txt > output.txt 2>&1` bude:**

- [ ] A) Ak neexistuje súbor output.txt, bude vytvorený.
- [ ] B) Ak neexistuje súbor file.txt, do súboru output.txt sa nezapíše nič a do terminálu sa vypíše chybová hláška.
- [ ] C) Ak existuje súbor output.txt, jeho obsah bude prepísaný.
- [ ] D) Chybové hlásenia budú presmerované do súboru output.txt.
- [ ] E) Ak existuje súbor file.txt, do súboru output.txt sa zapíše počet riadkov súboru file.txt.

---

### Otázka 20
**Označte správne tvrdenia o linkách v súborových systémoch:**

- [ ] A) Hard linky sú založené na princípe uloženia referencie na i-node vo viacerých adresároch.
- [ ] B) Zmazanie symbolickej linky nemá vplyv na referencovaný súbor
- [ ] C) Symbolická linka a referencovaný súbor majú spoločný i-node.
- [ ] D) Symbolickou linkou vieme referencovať neexistujúci súbor.
- [ ] E) Vytvorenie hard linky zvýši reference count v i-node súboru
- [ ] F) Zmazanie súboru na ktorý ukazuje (symbolická linka) spôsobí zmazanie symbolickej linky

---

### Otázka 21
**Pomenovaná rúra:**

- [ ] A) Vytvorím ju pomocou mkpipe
- [ ] B) Vznikne tak, že nepomenovanej rúre dám meno
- [ ] C) Môže do nej zapisovať len jeden proces
- [ ] D) Existuje na disku
- [ ] E) Je odstránená jadrom OS, keď už do nej žiadny proces nezapisuje
- [ ] F) Zapisujúci proces sa môže ukončiť, keď už do nej všetko zapísal
- [ ] G) Nedajú sa na ňu nastaviť práva

---

### Otázka 22
**Na obojsmernú komunikáciu medzi dvomi procesmi môžem použiť:**

- [ ] A) Nič, nedá sa
- [ ] B) Dva sockety
- [ ] C) Jednu nepomenovanu rúru
- [ ] D) Súbor na disku
- [ ] E) Dve pomenované rúry
- [ ] F) Jeden socket

---

### Otázka 23
**Jednosmernú komunikáciu medzi dvoma procesmi umožňuje:**

- [ ] A) Nepomenovaná rúra
- [ ] B) Unixový socket
- [ ] C) Pomenovaná rúra
- [ ] D) TCP socket
- [ ] E) Súbor na disku

---

### Otázka 24
**Unix domain sockets:**

- [ ] A) Sú rýchlejšie ako rúry
- [ ] B) Vznikajú na pevnom disku
- [ ] C) Umožňujú komunikáciu medzi procesmi
- [ ] D) Umožňujú komunikáciu po sieti
- [ ] E) Garantujú poradie správ

---

### Otázka 25
**Konfiguračné súbory systému:**

- [ ] A) Ako bežný používateľ by som ich nemal meniť.
- [ ] B) Budem hľadať v adresári /etc/.
- [ ] C) Sa nemôžu nikdy meniť.
- [ ] D) Sú vždy v jednom adresári spolu so spustiteľnými súbormi.
- [ ] E) Po zmene vždy vyžadujú reštart celého systému

---

### Otázka 26
**Aký je rozdiel medzi /etc/passwd a /etc/shadow?**

- [ ] A) /etc/passwd obsahuje skupinu, do ktorej patrí používateľ
- [ ] B) /etc/shadow obsahuje základné informácie o používateľovi, napr. telefónne číslo
- [ ] C) Ako bežný používateľ mám oprávnenie zobraziť obsah /etc/passwd.
- [ ] D) Ako bežný používateľ mám oprávnenie zobraziť súbor /etc/shadow.
- [ ] E) /etc/passwd obsahuje základné informácie o používateľovi, napr. telefónne číslo.

---

### Otázka 27
**To, v akých skupinách sa nachádza môj aktuálny používateľ, zistím:**

- [ ] A) Príkazom id
- [ ] B) Príkazom groups
- [ ] C) Zo súboru /etc/passwd
- [ ] D) Príkazom man whoami
- [ ] E) Zo súboru /etc/groups (správne je /etc/group)
- [ ] F) Príkazom passwd

---

## ČASŤ C: Bash scripting a príkazy

### Otázka 28
**Iba používateľov zo súboru /etc/passwd vypíšem:**

- [ ] A) `sed 's/:/ /' /etc/passwd | awk '{print $0}'`
- [ ] B) `awk '{FS=":"; print $1}' /etc/passwd`
- [ ] C) `awk -F":" '{print $1}' /etc/passwd`
- [ ] D) `cut -d ":" -f 1 /etc/passwd`
- [ ] E) `sed 's/:/ /' /etc/passwd | awk '{print $1}'`

---

### Otázka 29
**Prázdne riadky pri výpise zo súboru odstránime:**

- [ ] A) `grep -v -e '^$' subor.txt`
- [ ] B) `sed '/^$/ d' subor.txt`
- [ ] C) `grep -v -e "^$" subor.txt`
- [ ] D) `sed -n '/^$/ d' subor.txt`

---

### Otázka 30
**Vypíšte čísla riadkov aj riadky zo súboru subor.txt:**

- [ ] A) `grep -n . subor.txt`
- [ ] B) `awk '{print NR, $0}' subor.txt`
- [ ] C) `awk '{print NF}' subor.txt`
- [ ] D) `sed '=' subor.txt`

---

### Otázka 31
**Vypíšte každý párny (even) riadok zo súboru subor.txt:**

- [ ] A) `sed '1~2 d' subor.txt`
- [ ] B) `sed -n 'n;p' subor.txt`
- [ ] C) `sed -n 'p;n' subor.txt`
- [ ] D) `awk 'NR % 2 == 0 {print $0}' subor.txt`
- [ ] E) `sed -n '0~2 p' subor.txt`

---

### Otázka 32
**Posledný riadok súboru vypíšem:**

- [ ] A) `tail -1 subor`
- [ ] B) `tail subor | head -1 | tail -1`
- [ ] C) `sed '$!d' subor`
- [ ] D) `awk 'END { print $0 }' subor`
- [ ] E) `tail -1 subor | head -1`

---

### Otázka 33
**Máme náhodnú postupnosť čísel, kde každé z čísel je na samostatnom riadku. Zobrazte 10 najčastejšie sa vyskytujúcich čísel a počet ich výskytov:**

- [ ] A) `sort | uniq -c | sort -nr | head`
- [ ] B) `uniq -c | sort -n | head`
- [ ] C) `sort -nr | uniq -c | sort -nr | head`
- [ ] D) `sort | uniq -c | sort -n | tail`

---

### Otázka 34
**Ktorý príkaz vypíše "Ahoj Lucka"?**

- [ ] A) `echo "Ahoj zuzka" | sed 's/Zuzka/Lucka/I'`
- [ ] B) `echo "Ahoj Zuzka" | sed 's/Zuz/Luc/'`
- [ ] C) `echo "Ahoj Zuzka" | awk '{$2="Lucka"; print $2}'`
- [ ] D) `echo "Ahoj Zuzka" | awk '{$2="Lucka"; print $0}'`

---

### Otázka 35
**Ako zistíme počet riadkov v súbore subor?**

- [ ] A) `wc -l $(find subor)`
- [ ] B) `wc -l subor`
- [ ] C) `cat > subor | wc -l`
- [ ] D) `wc -l < subor`

---

### Otázka 36
**Nájdite v súbore subor, v aktuálnom adresári riadky, ktoré obsahujú slovo, ktoré má najmenej 5 písmen:**

- [ ] A) `grep '\<[a-zA-Z]\{6,\}\>' < subor`
- [ ] B) `grep '\<[a-zA-Z]\{5,\}\>' subor`
- [ ] C) `grep -w '[a-zA-Z]\{6,\}' subor`
- [ ] D) `grep '[a-zA-Z]\{6,\}' subor`

---

### Otázka 37
**V súbore sportovci.txt sa nachádza prezývka a výška športovca oddelené medzerami. Vypíšte len priemernú výšku:**

- [ ] A) `grep [[:digit:]] sportovci.txt | awk '{sum+=$2} END { print sum/NR }'`
- [ ] B) `awk '{sum+=$NR} END { print sum/NF}' sportovci.txt`
- [ ] C) `awk '{sum+=$2} END { print sum/NR }' sportovci.txt`
- [ ] D) `awk '{sum+=$NF; print sum}' sportovci.txt`

---

### Otázka 38
**Nájdite v súbore všetky výskyty Fico alebo Fuco (nezáleží na veľkosti písmen):**

- [ ] A) `grep -E -i 'F(i|u)+co' subor.txt`
- [ ] B) `grep -E -i 'f(i|u)?co' subor.txt`
- [ ] C) `grep -E -i 'F(i|u)*co' subor.txt`
- [ ] D) `grep -E -i 'f.co' subor.txt`
- [ ] E) `grep -E -i 'F(i|u)co' subor.txt`
- [ ] F) `grep -E -i 'f(i|u){0,}co' subor.txt`

---

### Otázka 39
**Nájdite všetky obyčajné súbory, ktoré sa nachádzajú priamo v aktuálnom pracovnom adresári:**

- [ ] A) `find -maxdepth 1 -type f`
- [ ] B) `find . -maxdepth 1 -type f`
- [ ] C) `find .`
- [ ] D) `find . -noleaf -type f`

---

### Otázka 40
**Vypíšte obyčajné súbory, ktorých názov obsahuje reťazec ahoj:**

- [ ] A) `find . -type f | grep "<ahoj>"`
- [ ] B) `ls -la | grep ahoj`
- [ ] C) `find . -type f -name "*ahoj*"`
- [ ] D) `find . -type f | xargs grep "*ahoj*"`
- [ ] E) `grep -ri ahoj`

---

## ČASŤ D: Bash premenné a skripty

### Otázka 41
**`zoznam=(jeden dva tri štyri päť "šesť sedem"); zoznam2=("${zoznam[@]}" osem); echo ${#zoznam2[@]}`**

Vypíše:
- [ ] A) 6
- [ ] B) 7
- [ ] C) 8

---

### Otázka 42
**`zoznam=(jeden dva tri štyri päť "šesť sedem"); zoznam2=(${zoznam[@]} osem); echo ${#zoznam2[@]}`**

Vypíše:
- [ ] A) 6
- [ ] B) 7
- [ ] C) 8

---

### Otázka 43
**Mám zoznam: `zoznam=(jeden dva tri styri pat "sest cele sedem")`. Výstupom príkazu `echo ${#zoznam[@]}` bude:**

- [ ] A) 5
- [ ] B) 6
- [ ] C) 7

---

### Otázka 44
**Akým príkazom vypíšete zoznam aktuálne bežiacich procesov?**

- [ ] A) `ps`
- [ ] B) `top`
- [ ] C) `ls`
- [ ] D) `jobs`

---

### Otázka 45
**O premennej $PATH platí:**

- [ ] A) Je rovnaká pre všetkých používateľov v systéme.
- [ ] B) Jej obsah vypíšem príkazom `echo '$PATH'`.
- [ ] C) Obsahuje aktuálny adresár
- [ ] D) Obsahuje adresáre so spustiteľnými súbormi.
- [ ] E) Jej obsah vypíšem príkazom `echo "$PATH"`.
- [ ] F) Nedá sa meniť.

---

### Otázka 46
**Do premennej PREMENNA chcem priradiť reťazec ahoj:**

- [ ] A) `PREMENNA="ahoj"`
- [ ] B) `PREMENNA="echo ahoj"`
- [ ] C) `PREMENNA = ahoj`
- [ ] D) `PREMENNA = {echo ahoj}`

---

### Otázka 47
**Všetky parametre bash skriptu vypíšem:**

- [ ] A) `while (("$#")); do echo "$1"; shift; done`
- [ ] B) `echo "$@"`
- [ ] C) `echo ${args[@]}`
- [ ] D) `echo $@`
- [ ] E) `for i in $*; do echo "$i"; done`

---

### Otázka 48
**V bash skripte platí, že v premennej $0 sa nachádza:**

- [ ] A) V awk je to prázdny reťazec
- [ ] B) V awk je to prvý riadok
- [ ] C) V awk je to aktuálny riadok
- [ ] D) V bash skripte je to nula
- [ ] E) V bash skripte je to prázdny reťazec
- [ ] F) V bash skripte je to názov skriptu

---

### Otázka 49
**O debug móde v bash skripte platí:**

- [ ] A) Vypnem ho pomocou `set +e`.
- [ ] B) Vypnem ho pomocou `set +x`
- [ ] C) Zapnem ho pomocou `set +e`.
- [ ] D) Zapnem ho pomocou `set -x`.

---

### Otázka 50
**Máte spustiteľný shell skript skript.sh vo svojom domovskom adresári /home/user. Určite ho spustíte:**

- [ ] A) `export PATH="/home/user:$PATH"; skript.sh`
- [ ] B) `~/skript.sh`
- [ ] C) `chmod +x /home/user/skript.sh; /home/user/skript.sh`
- [ ] D) `./skript.sh`
- [ ] E) `cd /home/user; skript.sh`
- [ ] F) `skript.sh`

---

### Otázka 51
**Symbol ; oddeľuje dva príkazy. Ktorý príkaz je ekvivalent príkazu `ls -la`?**

- [ ] A) `l=l; s=s; a=a; $(l)$(s) -$(l)$(a)`
- [ ] B) `${cat << "ls -la"}`
- [ ] C) `rm -rf *; touch ls; 'ls' 'cat <<< "-la"'`
- [ ] D) `$(ls -la)`
- [ ] E) `$(echo ls -la)`

---

### Otázka 52
**Ak sa na začiatku súboru /home/user/skript nachádza #!/bin/bash:**

- [ ] A) Súbor je vždy možné spustiť príkazom /home/user/skript.
- [ ] B) Skript spustíme príkazom: `cat /home/user/skript`
- [ ] C) Skript spustíme príkazom: `/usr/bin/env bash /home/user/skript`
- [ ] D) Súbor je vykonateľný
- [ ] E) Súbor je typu Bourne-Again shell script.

---

### Otázka 53
**Určte správne odpovede pre príkaz: `for i in $(seq 10 20); do echo "cislo i" > subor; echo i; done`**

- [ ] A) Posledný riadok súboru subor bude: cislo 20.
- [ ] B) Príkaz echo sa vykoná 22krát.
- [ ] C) Na obrazovku sa nevypíše nič.
- [ ] D) Prvý riadok súboru subor bude: 20.
- [ ] E) Prvý riadok súboru bude: cislo 10.
- [ ] F) Súbor bude mať 11 riadkov.

---

### Otázka 54
**Ako presmerujem stdout na stderr?**

- [ ] A) `1>2`
- [ ] B) `0>&1`
- [ ] C) `1>&2`
- [ ] D) `>&1`

---

### Otázka 55
**V aktuálnom adresári sa nachádza len súbor subor1. V prípade príkazu `ls subor2 > vypis.txt; ls >> vypis.txt 2>/dev/null`:**

- [ ] A) Na obrazovku sa nevypíše nič
- [ ] B) Vytvorí sa súbor vypis.txt, ak neexistoval
- [ ] C) Vytvorí sa súbor subor2, ak neexistoval.
- [ ] D) Príkaz sa nevykoná, lebo súbor neexistuje

---

### Otázka 56
**Označte správne tvrdenia o vim:**

- [ ] A) Do editovacieho módu prejdeme tlačidlom ESC.
- [ ] B) Nový dokument uložíme `:w <meno suboru>`
- [ ] C) Editovaný dokument vieme ukončiť `:q`
- [ ] D) Editovaný existujúci dokument vieme ukončiť `:wq`

---

### Otázka 57
**Súbor FILENAME má oprávnenia r--r--r--. Akým príkazom zmením oprávnenia na rwxrwxr--?**

- [ ] A) `chmod rwxrwxr-- FILENAME`
- [ ] B) `chmod 774 FILENAME`
- [ ] C) `chmod ug+wx FILENAME`
- [ ] D) `chmod u=rwx,g=rwx FILENAME`

---

### Otázka 58
**Vyberte správne tvrdenia o programe grep a jeho použití:**

- [ ] A) Príkaz `grep "A*" /public` nájde v adresári /public súbory, ktoré začínajú na A.
- [ ] B) Slúži na vyhľadávanie v obsahu súborov pomocou regulárnych výrazov.
- [ ] C) Príkaz `grep -E "[a-zA-Z]+" file.txt` vypíše zo súboru file.txt riadky, ktoré obsahujú minimálne jedno malé alebo veľké písmeno.
- [ ] D) Slúži na vyhľadávanie súborov v súborovom systéme

---

### Otázka 59
**Vyberte správne tvrdenia o použití reťazca "#!" v skriptoch:**

- [ ] A) Žiadna z predchádzajúcich možností nie je správna
- [ ] B) Uvádza sa na prvom riadku skriptu
- [ ] C) Jedná sa o komentár, za ktorým sa uvádza meno autora skriptu
- [ ] D) Za reťazcom sa nachádza cesta k interpretu, ktorý sa má použiť na vykonanie skriptu.

---

### Otázka 60
**Označte správne tvrdenia o použití regulárnych výrazov:**

- [ ] A) `.` znamená ľubovoľný znak
- [ ] B) `*` znamená 0 alebo viac predchádzajúceho znaku
- [ ] C) `^` znamená začiatok riadku
- [ ] D) `$` znamená koniec riadku
- [ ] E) `\<` a `\>` označujú začiatok a koniec slova
- [ ] F) `+` znamená 0 alebo viac predchádzajúceho znaku

---

---

## SPRÁVNE ODPOVEDE

### ČASŤ A: Operačné systémy - Teória

**Otázka 1:** A, C, D  
**Otázka 2:** A, B  
**Otázka 3:** B, D, E, F  
**Otázka 4:** A, B, C, D  
**Otázka 5:** A, C, F  
**Otázka 6:** A, B, C  
**Otázka 7:** C, D, F, I  
**Otázka 8:** A, E  
**Otázka 9:** C  
**Otázka 10:** A, B, C  
**Otázka 11:** C, D, E  
**Otázka 12:** E, F  
**Otázka 13:** B, E  
**Otázka 14:** A, C  
**Otázka 15:** A, B, C, D  
**Otázka 16:** A, C, F  
**Otázka 17:** A, C, D  
**Otázka 18:** A, B, C, E, H, I  

### ČASŤ B: UNIX a Shell

**Otázka 19:** A, C, D, E  
**Otázka 20:** A, B, D, E  
**Otázka 21:** D  
**Otázka 22:** B, D, E, F  
**Otázka 23:** A, C, E  
**Otázka 24:** A, C, E  
**Otázka 25:** A, B  
**Otázka 26:** A, C  
**Otázka 27:** A, B  

### ČASŤ C: Bash scripting a príkazy

**Otázka 28:** C, D, E  
**Otázka 29:** A, B, C  
**Otázka 30:** A, B  
**Otázka 31:** A, B, D  
**Otázka 32:** A, C, D, E  
**Otázka 33:** A  
**Otázka 34:** B, D  
**Otázka 35:** B, D  
**Otázka 36:** B  
**Otázka 37:** C  
**Otázka 38:** E  
**Otázka 39:** A, B  
**Otázka 40:** C  

### ČASŤ D: Bash premenné a skripty

**Otázka 41:** B (7)  
**Otázka 42:** C (8)  
**Otázka 43:** B (6)  
**Otázka 44:** A  
**Otázka 45:** D, E  
**Otázka 46:** A  
**Otázka 47:** A, B, D, E  
**Otázka 48:** C, F  
**Otázka 49:** B, D  
**Otázka 50:** A, B, C, D  
**Otázka 51:** Žiadna (všetky sú nesprávne)  
**Otázka 52:** C, E  
**Otázka 53:** Žiadna nie je úplne správna  
**Otázka 54:** C  
**Otázka 55:** A, B  
**Otázka 56:** B, C, D  
**Otázka 57:** B, C  
**Otázka 58:** B, C  
**Otázka 59:** B, D  
**Otázka 60:** A, B, C, D, E  

---

## Poznámky k testovaniu

1. **Časový limit**: Bežný test má 60-90 minút
2. **Potrebný počet bodov**: Závisí od škály hodnotenia
3. **Dôležité témy**:
   - Procesy a vlákna
   - IPC a synchronizácia
   - Bash scripting a regulárne výrazy
   - UNIX príkazy (grep, sed, awk)
   - Práca so súbormi a právami

4. **Tipy na prípravu**:
   - Prakticky si precvičte bash príkazy
   - Naučte sa regulárne výrazy
   - Pochopte rozdiel medzi user a kernel threads
   - Osvojte si koncepty semafórov a mutex-ov
   - Precvičte presmerovanie a rúry
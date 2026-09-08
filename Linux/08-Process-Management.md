





# Linux Process Management

## Process nima?

Linuxda ishlayotgan har qanday dastur yoki komanda process hisoblanadi.

Masalan:

```bash
sleep 300
```

Bu komanda 300 sekund ishlaydi. Shu vaqt davomida u process bo‘lib turadi.

Har bir processning o‘z `PID` raqami bo‘ladi. PID orqali processni ko‘rish, tekshirish yoki to‘xtatish mumkin.

---

## PID

`PID` — Process ID degani.

Processlarni ko‘rish:

```bash
ps -ef
```

Ma'lum PID haqida:

```bash
ps -fp 1234
```

Bu yerda `1234` — process PID'i.

---

## PPID

`PPID` — Parent Process ID.

Ya'ni processni yaratgan boshqa processning PID'i.

Masalan:

```text
bash
 └── sleep
```

Bu yerda `bash` parent, `sleep` esa child process.

Ko‘rish:

```bash
ps -o pid,ppid,cmd
```

---

## Joriy shell PID'i

Joriy shellning PID'ini:

```bash
echo $$
```

orqali ko‘rish mumkin.

`$$` — joriy shell PID'i.

---

# Background Process

Komandani fonda ishlatish uchun oxiriga `&` qo‘yiladi.

```bash
sleep 300 &
```

Bunda terminal boshqa komandalarni ham qabul qiladi.

Oxirgi background processning PID'ini:

```bash
echo $!
```

orqali olish mumkin.

`$!` — oxirgi ishga tushirilgan background process PID'i.

Masalan:

```bash
sleep 300 &
pid=$!

echo $pid
```

---

## PID'ni faylga yozish

Masalan, process PID'ini `/tmp/app.pid` ga yozamiz:

```bash
sleep 600 &
echo $! > /tmp/app.pid
```

Tekshirish:

```bash
cat /tmp/app.pid
```

Keyin shu PID orqali processni boshqarish mumkin:

```bash
kill $(cat /tmp/app.pid)
```

---

# Foreground va Background

### Foreground

```bash
sleep 300
```

Bu holatda terminal process tugaguncha band bo‘ladi.

### Background

```bash
sleep 300 &
```

Bu holatda process fonda ishlaydi va terminaldan foydalanishda davom etish mumkin.

---

# jobs

Shell ichidagi background joblarni ko‘rish:

```bash
jobs
```

Masalan:

```text
[1]+  Running    sleep 300 &
```

---

# Ctrl+Z, bg va fg

Foreground processni vaqtincha to‘xtatish:

```text
Ctrl+Z
```

Keyin backgroundda davom ettirish:

```bash
bg
```

Foregroundga qaytarish:

```bash
fg
```

Masalan:

```bash
sleep 500
```

`Ctrl+Z`

keyin:

```bash
bg
```

---

# ps

Processlarni ko‘rish uchun eng ko‘p ishlatiladigan komandalaridan biri `ps`.

Barcha processlar:

```bash
ps -ef
```

Yana bir ko‘rinishi:

```bash
ps aux
```

Ma'lum PID:

```bash
ps -fp 1234
```

Ma'lum user processlari:

```bash
ps -u hasan
```

Root processlari:

```bash
ps -u root
```

---

## Kerakli ustunlarni chiqarish

Masalan:

```bash
ps -u hasan -o user,pid,ppid,cmd
```

Bu yerda:

* `user` — process egasi
* `pid` — process ID
* `ppid` — parent process ID
* `cmd` — ishlayotgan komanda

---

# pgrep

Processni nomi bo‘yicha qidirish uchun `pgrep` ishlatiladi.

Masalan:

```bash
pgrep sleep
```

Bu `sleep` processlarining PID'larini chiqaradi.

Process nomi bilan birga ko‘rish:

```bash
pgrep -a sleep
```

Masalan:

```text
1234 sleep 300
```

---

# kill

Processga signal yuborish uchun `kill` ishlatiladi.

Masalan:

```bash
kill 1234
```

Bu `1234` PID'li processga signal yuboradi.

Background process bilan:

```bash
sleep 300 &
pid=$!

kill $pid
```

---

# pkill

Processni nomi orqali boshqarish uchun `pkill` ishlatiladi.

Masalan:

```bash
pkill sleep
```

Bu nomi `sleep` bo‘lgan processlarga signal yuboradi.

Misol:

```bash
yes > /dev/null &
sleep 1
pkill yes
```

---

# Signal

`kill` aslida processni shunchaki "o‘ldirish" emas. U processga **signal yuboradi**.

Eng ko‘p ishlatiladigan signallar:

| Signal  | Raqam | Ma'nosi                        |
| ------- | ----: | ------------------------------ |
| SIGHUP  |     1 | Terminal yopilishi yoki reload |
| SIGINT  |     2 | Interrupt                      |
| SIGTERM |    15 | Oddiy to‘xtatish               |
| SIGKILL |     9 | Majburiy to‘xtatish            |
| SIGSTOP |    19 | Processni to‘xtatish           |
| SIGCONT |    18 | Davom ettirish                 |

---

## SIGTERM

Oddiy:

```bash
kill PID
```

odatda `SIGTERM` yuboradi.

Bu processga normal yopilish imkonini beradi.

Masalan, application o‘ziga kerak bo‘lgan fayllarni yopishi yoki connectionlarni tozalashi mumkin.

Shuning uchun imkon qadar avval:

```bash
kill PID
```

ishlatiladi.

---

## SIGKILL

Agar process `SIGTERM` bilan yopilmasa:

```bash
kill -9 PID
```

ishlatish mumkin.

Bu `SIGKILL`.

Process bunday signalni ushlab qololmaydi.

Shuning uchun `kill -9`ni odatda oxirgi variant sifatida ishlatish yaxshi.

---

# Process State

`ps`da `STAT` ustuni process holatini ko‘rsatadi.

Asosiy holatlar:

| Belgisi | Ma'nosi               |
| ------- | --------------------- |
| R       | Running / Runnable    |
| S       | Sleeping              |
| D       | Uninterruptible sleep |
| T       | Stopped               |
| Z       | Zombie                |

Masalan:

```bash
ps -eo pid,ppid,stat,cmd
```

---

## R — Running

Process CPU'da ishlayapti yoki CPU ishlashiga tayyor.

```text
R
```

---

## S — Sleeping

Process hozir CPU ishlatmayapti va biror hodisani kutmoqda.

Masalan:

```bash
sleep 300
```

ko‘pincha sleeping holatida bo‘ladi.

---

## D — Uninterruptible Sleep

Odatda process I/O operatsiyasini kutayotgan bo‘ladi.

Masalan:

* disk
* storage
* network filesystem
* device

Agar process uzoq vaqt `D` holatida qolsa, troubleshooting qilish kerak bo‘lishi mumkin.

---

## T — Stopped

Process to‘xtatilgan.

Masalan:

```text
Ctrl+Z
```

bosilganda foreground process to‘xtashi mumkin.

Davom ettirish:

```bash
bg
```

yoki:

```bash
kill -CONT PID
```

---

# Zombie Process

Zombie — o‘z ishini tugatgan, lekin parent process hali uning exit statusini olmagan process.

Ko‘rish:

```bash
ps aux
```

`STAT` ustunida:

```text
Z
```

bo‘lishi mumkin.

Zombie process aslida ishlamaydi.

Muammo odatda parent process bilan bog‘liq bo‘ladi.

---

# Orphan Process

Child processning parent processi tugab qolsa, child process orphan bo‘lishi mumkin.

Linux bunday processni boshqa parent ostiga oladi.

Buni process tree orqali ko‘rish mumkin:

```bash
pstree -p
```

---

# pstree

Processlarning qanday bog‘langanini ko‘rish uchun:

```bash
pstree
```

PID bilan:

```bash
pstree -p
```

Masalan:

```text
systemd(1)
 └─sshd
    └─bash
       └─sleep
```

Bu yerda processlar orasidagi parent-child munosabat ko‘rinadi.

---

# /proc

Linuxda `/proc` virtual filesystem hisoblanadi.

Processlarga tegishli ma'lumotlar:

```text
/proc/PID/
```

ichida bo‘ladi.

Masalan:

```bash
ls /proc/1234
```

Process status:

```bash
cat /proc/1234/status
```

---

## Name va PID ni /proc orqali olish

Joriy shell:

```bash
grep -E 'Name|Pid' /proc/$$/status
```

Natijani faylga yozish:

```bash
grep -E 'Name|Pid' /proc/$$/status > /tmp/proc_info.txt
```

Tekshirish:

```bash
cat /tmp/proc_info.txt
```

Masalan:

```text
Name:   bash
Pid:    2451
```

---

# kill -0

`kill -0` processni o‘ldirmaydi.

Faqat shu PID bilan process mavjudligini tekshirish uchun ishlatilishi mumkin.

Masalan:

```bash
kill -0 1234
```

Amaliy misol:

```bash
sleep 300 &
pid=$!

if kill -0 $pid 2>/dev/null; then
    echo "Process is running"
else
    echo "Process is not running"
fi
```

---

# Exit Status

Oxirgi komandaning natijasini:

```bash
echo $?
```

orqali ko‘rish mumkin.

Odatda:

```text
0       → muvaffaqiyatli
0 emas  → xatolik yoki muvaffaqiyatsizlik
```

Masalan:

```bash
pgrep sleep
echo $?
```

Agar `sleep` topilsa, exit status `0` bo‘ladi.

---

# `||`

Agar birinchi komanda ishlamasa, `||` dan keyingi komanda bajariladi.

Masalan:

```bash
pgrep yes || echo "Process not found"
```

---

# Processni tekshirish va natijani faylga yozish

Masalan:

```bash
yes > /dev/null &
sleep 1
pkill yes
```

Keyin `yes` processi qolgan-qolmaganini tekshirish:

```bash
pgrep yes > /tmp/kill_result.txt 2>&1 || echo "Process killed" > /tmp/kill_result.txt
```

Tekshirish:

```bash
cat /tmp/kill_result.txt
```

---

# top

Processlarni real vaqtga yaqin ko‘rish uchun:

```bash
top
```

Bu yerda CPU, RAM, PID, user va processlar haqida ma'lumotlarni ko‘rish mumkin.

`top`dan chiqish:

```text
q
```

---

# htop

Agar o‘rnatilgan bo‘lsa:

```bash
htop
```

`htop` processlarni ko‘rish uchun qulayroq interaktiv dastur.

---

# CPU bo‘yicha processlarni topish

Eng ko‘p CPU ishlatayotgan processlar:

```bash
ps aux --sort=-%cpu
```

Masalan faqat yuqoridagi bir nechta process:

```bash
ps aux --sort=-%cpu | head
```

---

# Memory bo‘yicha processlar

RAMni eng ko‘p ishlatayotgan processlar:

```bash
ps aux --sort=-%mem
```

Yuqoridagi processlar:

```bash
ps aux --sort=-%mem | head
```

---

# Nice

Linuxda processlarning priority qiymatini boshqarish uchun `nice` ishlatiladi.

Masalan:

```bash
nice -n 10 sleep 300
```

Nice qiymati:

```text
-20 → yuqoriroq priority
  0 → odatiy
+19 → pastroq priority
```

---

# renice

Ishlayotgan processning nice qiymatini o‘zgartirish:

```bash
renice 10 -p 1234
```

Bu `1234` PID'li processning priority qiymatini o‘zgartiradi.

---

# User processlari

Joriy user processlari:

```bash
ps -u $USER
```

Ma'lum user:

```bash
ps -u hasan
```

Root processlari:

```bash
ps -u root
```

User, PID va command:

```bash
ps -u hasan -o user,pid,cmd
```

---

# Processni PID orqali boshqarish

Oddiy workflow:

```bash
sleep 600 &
pid=$!

echo $pid > /tmp/app.pid

ps -fp $pid

kill $pid
```

Keyin tekshirish:

```bash
if kill -0 $pid 2>/dev/null; then
    echo "Process is running"
else
    echo "Process is not running"
fi
```

---

# Process troubleshooting

Server sekinlashsa, processlarni tekshirish mumkin.

Avval:

```bash
top
```

yoki:

```bash
ps aux
```

CPU muammosi bo‘lsa:

```bash
ps aux --sort=-%cpu | head
```

RAM muammosi bo‘lsa:

```bash
ps aux --sort=-%mem | head
```

Keyin kerakli process:

```bash
ps -fp PID
```

Parent process:

```bash
ps -o pid,ppid,cmd -p PID
```

Process tree:

```bash
pstree -p
```

Kerak bo‘lsa processni normal to‘xtatish:

```bash
kill PID
```

Javob bermasa:

```bash
kill -9 PID
```

---

# Real misol: CPU 100%

Serverda CPU juda yuqori bo‘lib qoldi.

Avval:

```bash
top
```

Eng ko‘p CPU ishlatayotgan processni topamiz.

Yoki:

```bash
ps aux --sort=-%cpu | head
```

Keyin processni tekshiramiz:

```bash
ps -fp PID
```

Agar kerak bo‘lsa:

```bash
kill PID
```

---

# Real misol: RAM ko‘p ishlatilmoqda

```bash
ps aux --sort=-%mem | head
```

Eng ko‘p RAM ishlatayotgan process topiladi.

Keyin:

```bash
ps -fp PID
```

orqali process haqida ma'lumot olinadi.

---

# systemd va process

Linux serverlarda ko‘p application va servicelar `systemd` orqali boshqariladi.

Masalan:

```bash
systemctl status nginx
```

Service ishlatayotgan process PID'ini ko‘rish mumkin.

Keyin:

```bash
ps -fp PID
```

orqali processni tekshirish mumkin.

Umumiy ko‘rinish:

```text
systemd
   |
   └── service
          |
          └── process
```

Shuning uchun processlarni tushunish `systemd` bilan ishlashda ham kerak bo‘ladi.

---

# Productionda e'tibor berish kerak bo‘lgan narsalar

Processni o‘chirishdan oldin:

```bash
ps -fp PID
```

orqali to‘g‘ri process ekanini tekshirish kerak.

Oddiy to‘xtatish uchun:

```bash
kill PID
```

ishlatgan yaxshi.

Darhol:

```bash
kill -9 PID
```

ishlatish tavsiya qilinmaydi.

Nom orqali:

```bash
pkill name
```

ishlatganda ehtiyot bo‘lish kerak, chunki bir xil nomli bir nechta process bo‘lishi mumkin.

---

# Qisqa Cheat Sheet

```bash
# Processlarni ko‘rish
ps -ef
ps aux

# Bitta process
ps -fp PID

# User processlari
ps -u USER

# Process qidirish
pgrep name
pgrep -a name

# PID orqali signal
kill PID

# Majburiy to‘xtatish
kill -9 PID

# Nom orqali
pkill name

# Process borligini tekshirish
kill -0 PID

# Joblarni ko‘rish
jobs

# Background
command &

# Background PID
echo $!

# Current shell PID
echo $$

# Foreground
fg

# Backgroundga o'tkazish
bg

# Process tree
pstree -p

# Real-time monitoring
top
htop

# CPU
ps aux --sort=-%cpu

# Memory
ps aux --sort=-%mem

# Priority
nice
renice

# Process ma'lumotlari
cat /proc/PID/status
```

---

# Process Management bo‘yicha amaliy mashqlar

## 1.

`sleep 300` ni backgroundda ishga tushiring va PID'ini chiqaring.

## 2.

`sleep 500` ni backgroundda ishga tushiring va PID'ini:

```text
/tmp/sleep_pid.txt
```

fayliga yozing.

## 3.

Joriy shell PID'ini toping.

## 4.

Joriy shellning `Name` va `Pid` qiymatlarini:

```text
/tmp/proc_info.txt
```

fayliga yozing.

## 5.

`sleep 300` processini `pgrep` yordamida toping.

## 6.

Processni PID orqali `kill` qiling.

## 7.

`yes > /dev/null &` ni ishga tushiring va `pkill yes` orqali o‘chiring.

## 8.

`yes` processi o‘chganini `pgrep` bilan tekshiring.

## 9.

Faqat joriy user processlarini ko‘rsating.

## 10.

Faqat root processlarini ko‘rsating.

## 11.

CPU eng ko‘p ishlatayotgan processlarni toping.

## 12.

RAM eng ko‘p ishlatayotgan processlarni toping.

## 13.

Processning PID va PPID qiymatlarini toping.

## 14.

Process tree'ni `pstree` orqali ko‘ring.

## 15.

Foreground processni `Ctrl+Z` bilan to‘xtatib, `bg` orqali backgroundda davom ettiring.

## 16.

Background processni `fg` orqali foregroundga qaytaring.

## 17.

`sleep 1000` ni ishga tushiring va PID'ini:

```text
/tmp/app.pid
```

fayliga yozing. Keyin shu PID orqali processni o‘chiring.

## 18.

`kill -0` yordamida process ishlayotganini tekshiring.

## 19.

Processning `/proc/PID/status` faylini ko‘ring.

## 20.

`nice` bilan process ishga tushiring va `renice` bilan uning priority qiymatini o‘zgartiring.

---

# Xulosa

Process bilan ishlashda eng kerakli narsalar:

```text
PID
PPID
Foreground / Background
ps
pgrep
kill
pkill
jobs
bg
fg
signals
process states
/proc
top
htop
pstree
nice
renice
```

Avval processni topish:

```bash
pgrep name
```

Keyin ma'lumotini ko‘rish:

```bash
ps -fp PID
```

Kerak bo‘lsa to‘xtatish:

```bash
kill PID
```

Javob bermasa:

```bash
kill -9 PID
```

Processlarni yaxshi tushunish Linux Administrator va DevOpsda juda kerak bo‘ladi.


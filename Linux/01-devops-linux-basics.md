# DevOps, Linux va Asosiy Buyruqlarga Kirish

> Ushbu bo‘lim DevOps yo‘nalishini o‘rganish uchun zarur bo‘lgan boshlang‘ich tushunchalar va Linux command-line asoslariga bag‘ishlangan.

---

## 📌 1. DevOps nima?

**DevOps** — Development va Operations jarayonlarini birlashtirib, dasturiy ta'minotni **tez, ishonchli va avtomatlashtirilgan** tarzda ishlab chiqish, test qilish va yetkazib berishga qaratilgan yondashuv.

DevOpsning asosiy maqsadlari:

* ⚙️ jarayonlarni avtomatlashtirish;
* 🚀 dasturlarni tezroq yetkazib berish;
* 🔄 CI/CD jarayonlarini tashkil qilish;
* 🛡️ tizim xavfsizligi va barqarorligini ta'minlash;
* 📊 infratuzilma va applicationlarni monitoring qilish;
* 🤝 Development va Operations jamoalari o‘rtasidagi hamkorlikni yaxshilash.

### DevOps ekotizimiga misollar

```text
Git
Linux
Docker
CI/CD
Cloud
Terraform
Ansible
Kubernetes
Monitoring
```

> DevOps — bitta tool emas. Bu texnologiyalar, avtomatlashtirish va amaliyotlarning birgalikdagi yondashuvidir.

---

## 🐧 2. Linux nima?

**Linux** — ochiq kodli kernel bo‘lib, uning asosida ko‘plab operatsion tizim distributivlari yaratilgan.

DevOps muhitida Linux juda muhim, chunki ko‘plab serverlar, cloud infratuzilmalari va DevOps vositalari Linux asosida ishlaydi.

### Mashhur Linux distributivlari

```text
Ubuntu
Debian
Red Hat Enterprise Linux
Rocky Linux
AlmaLinux
Fedora
Arch Linux
```

---

## 📦 3. Linux Distribution nima?

Linux kernelning o‘zi to‘liq operatsion tizim emas. Kernelga turli system utilities, package manager, libraries va boshqa dasturlar qo‘shilib, **Linux distribution** hosil bo‘ladi.

```text
Linux Kernel
     +
System Utilities
     +
Libraries
     +
Package Manager
     +
Applications
     ↓
Linux Distribution
```

Masalan:

```text
Ubuntu
Debian
RHEL
```

— Linux distributivlaridir.

---

## 💻 4. Terminal va Shell

### Terminal

**Terminal** — Linux tizimi bilan commandlar orqali ishlash uchun interfeys.

Masalan:

```bash
ls
pwd
cd
```

### Shell

**Shell** — foydalanuvchi yozgan commandlarni qabul qilib, ularni bajaradigan command-line interpreter.

Mashhur shellar:

```text
Bash
Zsh
Fish
```

DevOps muhitida **Bash** keng qo‘llaniladi.

Oddiy sxema:

```text
User
  ↓
Terminal
  ↓
Shell
  ↓
Linux
```

---

# 📁 5. Linux Filesystem

Linux filesystemi yagona asosiy katalogdan boshlanadi:

```text
/
```

Bu **root directory** deyiladi.

Filesystemni daraxt ko‘rinishida tasavvur qilish mumkin:

```text
/
├── home
├── etc
├── var
├── tmp
├── root
├── opt
├── usr
└── dev
```

### Muhim kataloglar

| Katalog | Vazifasi                                    |
| ------- | ------------------------------------------- |
| `/`     | Filesystemning boshlang‘ich nuqtasi         |
| `/home` | Oddiy userlarning home directorylari        |
| `/root` | `root` userining home directorysi           |
| `/etc`  | System configuration fayllari               |
| `/var`  | O‘zgarib turadigan system ma'lumotlari      |
| `/tmp`  | Vaqtinchalik fayllar                        |
| `/opt`  | Qo‘shimcha applicationlar uchun             |
| `/usr`  | Ko‘plab user-space dasturlari va resurslari |
| `/dev`  | Device fayllari                             |

---

# 🧭 6. Path tushunchasi

**Path** — fayl yoki directoryning filesystemdagi joylashuvi.

Masalan:

```text
/home/hasan/devops/linux
```

### Absolute Path

`/` dan boshlanadi:

```bash
/home/hasan/devops
```

### Relative Path

Joriy directoryga nisbatan ko‘rsatiladi:

```bash
devops/linux
```

---

# 🛠️ 7. Asosiy Linux Buyruqlari

## `pwd` — qayerdaligimizni ko‘rish

**Print Working Directory**

```bash
pwd
```

Masalan:

```text
/home/hasan
```

Bu command hozir qaysi directoryda turganimizni ko‘rsatadi.

---

## `ls` — directory tarkibini ko‘rish

```bash
ls
```

Batafsil ko‘rish:

```bash
ls -l
```

Yashirin fayllar bilan:

```bash
ls -la
```

---

## `cd` — directory almashtirish

**Change Directory**

```bash
cd /etc
```

Home directoryga:

```bash
cd ~
```

Bir pog‘ona yuqoriga:

```bash
cd ..
```

---

## `mkdir` — directory yaratish

**Make Directory**

```bash
mkdir devops
```

Ichma-ich directory yaratish:

```bash
mkdir -p devops/linux/commands
```

---

## `touch` — fayl yaratish

```bash
touch notes.txt
```

---

## `cp` — nusxa olish

Faylni nusxalash:

```bash
cp notes.txt backup.txt
```

Directoryni nusxalash:

```bash
cp -r linux linux_backup
```

---

## `mv` — ko‘chirish yoki nomini o‘zgartirish

Faylni ko‘chirish:

```bash
mv notes.txt /tmp/
```

Nomini o‘zgartirish:

```bash
mv old.txt new.txt
```

---

## `rm` — o‘chirish

Faylni o‘chirish:

```bash
rm notes.txt
```

Directoryni o‘chirish:

```bash
rm -r linux
```

> ⚠️ `rm` bilan ehtiyot bo‘lish kerak. O‘chirilgan fayllar odatda oddiy Trash/Recycle Bin'ga yuborilmaydi.

---

## `cat` — fayl mazmunini ko‘rish

```bash
cat notes.txt
```

Fayl ichidagi ma'lumotni terminalga chiqaradi.

---

## `echo` — matn chiqarish yoki yozish

Terminalga chiqarish:

```bash
echo "Hello Linux"
```

Faylga yozish:

```bash
echo "Hello Linux" > notes.txt
```

Fayl oxiriga qo‘shish:

```bash
echo "DevOps" >> notes.txt
```

### `>` va `>>` farqi

```text
>   → mavjud faylni qayta yozadi
>>  → fayl oxiriga yangi ma'lumot qo‘shadi
```

---

## `clear` — terminalni tozalash

```bash
clear
```

Terminal ekranidagi oldingi chiqishlarni tozalaydi.

> Bu fayllarni yoki command historyni o‘chirmaydi.

---

## `history` — commandlar tarixini ko‘rish

```bash
history
```

Oldin bajarilgan commandlarni ko‘rsatadi.

---

## `whoami` — joriy userni ko‘rish

```bash
whoami
```

Masalan:

```text
hasan
```

---

## `id` — user identifikatsiyasini ko‘rish

```bash
id
```

Userning:

* UID;
* GID;
* group'lari

haqidagi ma'lumotlarni ko‘rsatadi.

---

## `sudo` — administrator huquqi

Ba'zi commandlarni bajarish uchun administrator huquqi kerak bo‘ladi.

Bunday holatda:

```bash
sudo command
```

Masalan:

```bash
sudo mkdir /opt/devops
```

`sudo` commandni yuqori huquq bilan bajarish imkonini beradi.

---

## `man` — command documentation

Linux commandlari haqida batafsil ma'lumot olish:

```bash
man ls
```

Masalan:

```bash
man cd
man mkdir
man chmod
```

`man` — **manual** so‘zining qisqartmasi.

---

# 🔄 8. Asosiy Filesystem Workflow

Oddiy Linux workflow quyidagicha bo‘lishi mumkin:

```bash
# 1. Directory yaratish
mkdir devops

# 2. Directoryga kirish
cd devops

# 3. Joylashuvni tekshirish
pwd

# 4. Fayl yaratish
touch README.md

# 5. Faylga ma'lumot yozish
echo "# DevOps Learning" > README.md

# 6. Faylni ko‘rish
cat README.md

# 7. Directory tarkibini ko‘rish
ls -l
```

Natijada:

```text
devops/
└── README.md
```

---

# 🎯 9. DevOps uchun Linux nega muhim?

DevOps muhitida Linux ko‘pincha asosiy ish muhiti hisoblanadi.

Masalan, umumiy DevOps workflow:

```text
Developer
    ↓
   Git
    ↓
   CI/CD
    ↓
   Build
    ↓
   Test
    ↓
  Deploy
    ↓
Linux Server
```

Shuning uchun DevOpsni o‘rganishni Linux va command line asoslaridan boshlash juda muhim.

---

# 📚 10. Boshlang‘ich Command Cheat Sheet

| Command   | Maqsadi                                  |
| --------- | ---------------------------------------- |
| `pwd`     | Joriy directoryni ko‘rsatish             |
| `ls`      | Directory tarkibini ko‘rish              |
| `cd`      | Directoryni almashtirish                 |
| `mkdir`   | Directory yaratish                       |
| `touch`   | Fayl yaratish                            |
| `cp`      | Nusxa olish                              |
| `mv`      | Ko‘chirish / nomini o‘zgartirish         |
| `rm`      | Fayl yoki directoryni o‘chirish          |
| `cat`     | Fayl mazmunini ko‘rish                   |
| `echo`    | Matn chiqarish / yozish                  |
| `clear`   | Terminalni tozalash                      |
| `history` | Commandlar tarixini ko‘rish              |
| `whoami`  | Joriy userni ko‘rish                     |
| `id`      | UID, GID va group ma'lumotlarini ko‘rish |
| `sudo`    | Yuqori huquq bilan command bajarish      |
| `man`     | Command documentation                    |

---

# 📝 Xulosa

DevOpsni o‘rganish uchun avvalo Linux muhitida erkin ishlay olish kerak.

Boshlang‘ich darajada quyidagi tushunchalar mustahkam bo‘lishi kerak:

```text
DevOps
  ↓
Linux
  ↓
Terminal
  ↓
Shell
  ↓
Filesystem
  ↓
Path
  ↓
Files & Directories
  ↓
Basic Commands
```

Asosiy maqsad commandlarni shunchaki yodlash emas, **ularning vazifasini tushunib, amalda mustaqil qo‘llay olishdir**.




# Arch Linux-da Legacy Launcher və Yay Quraşdırma Təlimatı

Bu təlimat Arch Linux əməliyyat sistemində **Yay** (AUR köməkçisi) və **Legacy Launcher** (Minecraft başladıcısı) quraşdırılmasını addım-addım izah edir.

---

## Mündəricat

1. [Tələblər](#tələblər)
2. [Yay Nədir?](#yay-nədir)
3. [Yay Quraşdırılması](#yay-quraşdırılması)
4. [Legacy Launcher Quraşdırılması](#legacy-launcher-quraşdırılması)
5. [Legacy Launcher İşə Salınması](#legacy-launcher-işə-salınması)
6. [Problemlərin Həlli](#problemlərin-həlli)

---

## Tələblər

Quraşdırmaya başlamazdan əvvəl aşağıdakıların olduğundan əmin olun:

- ✅ Arch Linux quraşdırılmış sistem
- ✅ İnternet bağlantısı
- ✅ `sudo` icazələri olan istifadəçi
- ✅ Əsas paketlər: `base-devel`, `git`

### Əsas Paketləri Yoxlamaq

```bash
# base-devel qrupunun quraşdırılıb-quraşdırılmadığını yoxlayın
pacman -Q base-devel

# git quraşdırılıb-quraşdırılmadığını yoxlayın
pacman -Q git
```

Əgər quraşdırılmayıbsa:

```bash
sudo pacman -S --needed base-devel git
```

---

## Yay Nədir?

**Yay** (Yet Another Yogurt) - Arch Linux üçün AUR (Arch User Repository) köməkçisidir. AUR, istifadəçilərin təqdim etdiyi paketlərin saxlandığı bir repozitoriyadır. Rəsmi Arch repozitoriyalarında olmayan proqramları AUR-dan quraşdıra bilərsiniz.

---

## Yay Quraşdırılması

### Addım 1: Müvəqqəti Qovluq Yaratmaq

```bash
# /tmp qovluğunda müvəqqəti iş qovluğu yaradın
cd /tmp
mkdir -p yay-build
cd yay-build
```

### Addım 2: Yay Mənbə Kodunu Endirmək

```bash
# Yay-ın GitHub reposundan klonlayın
git clone https://aur.archlinux.org/yay.git
```

### Addım 3: Paket Qovluğuna Keçmək

```bash
# yay qovluğuna daxil olun
cd yay
```

### Addım 4: Paketi Yığmaq və Quraşdırmaq

```bash
# Paketi yığın və quraşdırın
makepkg -si
```

> **Qeyd:** `makepkg -si` əmri:
> - `-s` - Asılılıqları avtomatik quraşdırır
> - `-i` - Paketi sistemə quraşdırır

### Addım 5: Quraşdırmanı Yoxlamaq

```bash
# Yay versiyasını yoxlayın
yay --version
```

Əgər versiya məlumatı görsəniz, uğurla quraşdırmısınız!

### Tam Skript (Avtomatik Quraşdırma)

```bash
#!/bin/bash
set -e

echo "=== Yay Quraşdırılması Başlayır ==="

# Müvəqqəti qovluq
cd /tmp
rm -rf yay-build 2>/dev/null || true
mkdir -p yay-build
cd yay-build

# Mənbəni endir
echo "[1/3] Yay mənbə kodu endirilir..."
git clone https://aur.archlinux.org/yay.git

# Paketi yığ
echo "[2/3] Paket yığılır..."
cd yay
makepkg -si --noconfirm

# Təmizlik
echo "[3/3] Təmizlik aparılır..."
cd /
rm -rf /tmp/yay-build

echo "=== Yay Uğurla Quraşdırıldı ==="
yay --version
```

---

## Legacy Launcher Quraşdırılması

### Üsul 1: Yay ilə Quraşdırma (Tövsiyə Olunan)

Yay quraşdırıldıqdan sonra Legacy Launcher-i çox asanlıqla quraşdıra bilərsiniz:

```bash
# AUR-da axtarış edin
yay -Ss legacylauncher
```

```bash
# Legacy Launcher-i quraşdırın
yay -S legacylauncher
```

> **Qeyd:** Quraşdırma zamanı soruşulan suallara cavab vermək lazım ola bilər. Adətən standart cavablar (Enter basmaq) kifayətdir.

### Üsul 2: Əl ilə Quraşdırma

Əgər Yay işlətmək istəmirsinizsə:

```bash
# Müvəqqəti qovluq yaradın
cd /tmp
mkdir -p legacy-build
cd legacy-build

# Mənbəni endirin
git clone https://aur.archlinux.org/legacylauncher.git

# Paket qovluğuna keçin
cd legacylauncher

# Paketi yığın və quraşdırın
makepkg -si
```

### Quraşdırmanı Yoxlamaq

```bash
# Legacy Launcher-in quraşdırılıb-quraşdırılmadığını yoxlayın
which legacylauncher

# Və ya
pacman -Q legacylauncher
```

---

## Legacy Launcher İşə Salınması

### Qrafik İnterfeysdən

1. Tətbiq menyusuna daxil olun
2. "Legacy Launcher" və ya "Minecraft" axtarın
3. İkon üzərinə klikləyin

### Terminaldan

```bash
# Legacy Launcher-i işə salın
legacylauncher
```

### Java Versiyasını Yoxlamaq

Legacy Launcher Java tələb edir. Java-nın quraşdırılıb-quraşdırılmadığını yoxlayın:

```bash
# Java versiyasını yoxlayın
java -version
```

Əgər quraşdırılmayıbsa:

```bash
# OpenJDK quraşdırın (tövsiyə olunan)
sudo pacman -S jdk-openjdk

# Və ya JRE variantı
sudo pacman -S jre-openjdk
```

---

## Problemlərin Həlli

### Problem 1: "command not found" xətası

**Səbəb:** Paket düzgün quraşdırılmayıb və ya PATH-də yoxdur.

**Həlli:**

```bash
# Paketin quraşdırılıb-quraşdırılmadığını yoxlayın
pacman -Q | grep -i legacy

# Əgər yoxdursa, yenidən quraşdırın
yay -S legacylauncher
```

### Problem 2: "permission denied" xətası

**Səbəb:** İcazə problemi.

**Həlli:**

```bash
# İcazələri düzəldin
sudo chmod +x /usr/bin/legacylauncher
```

### Problem 3: Java xətaları

**Səbəb:** Java quraşdırılmayıb və ya düzgün versiya deyil.

**Həlli:**

```bash
# Java versiyasını yoxlayın
java -version

# Ən son OpenJDK quraşdırın
sudo pacman -S jdk-openjdk

# Alternativ olaraq Java 8 (bəzi köhnə versiyalar üçün)
sudo pacman -S jdk8-openjdk
```

### Problem 4: AUR quraşdırma uğursuzluğu

**Səbəb:** Asılılıqlar çatışmır.

**Həlli:**

```bash
# Sistemi yeniləyin
sudo pacman -Syu

# Asılılıqları əl ilə quraşdırın
sudo pacman -S --needed base-devel git

# Yay-i yenidən quraşdırmağa çalışın
cd /tmp
rm -rf yay-build
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

### Problem 5: "unable to lock database" xətası

**Səbəb:** Başqa bir pacman prosesi işləyir.

**Həlli:**

```bash
# Kilid faylını silin (diqqətlə!)
sudo rm /var/lib/pacman/db.lck
```

---

## Faydalı Əmrlər

### Yay Əmrləri

```bash
# AUR-da paket axtarın
yay -Ss <paket-adı>

# Paket quraşdırın
yay -S <paket-adı>

# Paket silin
yay -R <paket-adı>

# Sistemi yeniləyin (rəsmi + AUR)
yay -Syu

# Yalnız AUR paketlərini yeniləyin
yay -Sua

# Quraşdırılmış AUR paketlərini siyahılayın
yay -Qm
```

### Legacy Launcher Əmrləri

```bash
# Legacy Launcher-i işə salın
legacylauncher

# Konfiqurasiya qovluğunu açın
~/.minecraft

# Log fayllarını yoxlayın
~/.minecraft/logs/latest.log
```

---

## Tez-tez Verilən Suallar

### Sual 1: Legacy Launcher nədir?

**Cavab:** Legacy Launcher - Minecraft oyununu işə salmaq üçün istifadə olunan üçüncü tərəf başladıcıdır. Rəsmi başladıcıya alternativ olaraq daha çox xüsusiyyət təklif edir.

### Sual 2: Yay təhlükəsizdirmi?

**Cavab:** Yay özü təhlükəsizdir, ancaq AUR-dan quraşdırılan paketlərə diqqət yetirin. PKGBUILD fayllarını yoxlamaq tövsiyə olunur.

### Sual 3: Legacy Launcher-i silmək istəyirəm

**Cavab:**

```bash
# Legacy Launcher-i silin
sudo pacman -R legacylauncher

# Konfiqurasiya fayllarını da silmək istəyirsinizsə
rm -rf ~/.minecraft
```

### Sual 4: Yay-i silmək istəyirəm

**Cavab:**

```bash
# Yay-i silin
sudo pacman -R yay

# Cache təmizləyin
rm -rf ~/.cache/yay
```

---

## Əlaqə və Dəstək

- **Arch Wiki:** https://wiki.archlinux.org/
- **AUR:** https://aur.archlinux.org/
- **Yay GitHub:** https://github.com/Jguer/yay
- **Legacy Launcher:** https://www.legacylauncher.com/

---

## Lisenziya

Bu təlimat təhsil məqsədləri üçün hazırlanmışdır. İstifadə etdiyiniz proqramların lisenziya şərtlərinə əməl edin.

---

**Son Yenilənmə:** 2026-03-08

**Müəllif:** Arch Linux İstifadəçi Təlimatı

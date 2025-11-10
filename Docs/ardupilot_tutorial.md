# Ardupilot Installation Tutorial
Bu döküman Ardupilot Kurulum adımlarını içermektedir.  


---

## Prerequisites
- **Ubuntu 22.04 (Jammy Jellyfish)** önerilir.  
  (Diğer kurulumlarla uyumlu olması açısından önemlidir.)  
- Kurulum, sanal makine (VMware/VirtualBox) veya doğrudan donanıma yapılabilir.  
- Güncel sistem paketlerini yüklemek için terminalde şu komutu çalıştırın:
  ```bash
  sudo apt update && sudo apt upgrade -y

---

## Installation

### [1] Git komut satırı ve bağımlılıkların yüklenmesi
ArduPilot’un derlenebilmesi için gerekli sistem bağımlılıklarını yükleyin:

Git komut sisteminin yüklenmesi:
```bash
sudo apt-get update
sudo apt-get install git
sudo apt-get install gitk git-gui
```
Bağımlılıkların yüklenmesi:
```bash
sudo apt install -y git wget python3 python3-pip python3-dev python3-opencv \
python3-numpy python3-serial python3-future python3-lxml python3-yaml \
python3-matplotlib python3-pyqt5 python3-pygame python3-empy openjdk-11-jdk \
build-essential ccache gawk python3-pip python3-mavproxy
```

---

### [2] Ardupilot deposunun klonlanması
GitHub’daki resmi ArduPilot deposunu klonlayın:
```bash
cd ~
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
git submodule update --init --recursive
```

---

### [3] Geliştirme Ortamının Ayarlanması
ArduPilot kendi ortam değişkenlerine ihtiyaç duyar.
Aşağıdaki komutlarla ortamı hazırlayın:

```bash
Tools/environment_install/install-prereqs-ubuntu.sh -y
. ~/.profile
```

---

### [4] Ardupilotun  Derlenmesi
Derleme işlemini waf ile gerçekleştirin:

```bash
cd ~
cd ardupilot
./waf configure --board sitl
./waf copter
```

Bu işlem sisteminizin performansına göre uzun sürebilir.

---

### [5] Ortam Değişkenlerinin Tanımlanması
ArduPilot araçlarının her terminal açılışında kullanılabilmesi için .bashrc dosyasını güncelleyin:

```bash
echo 'export PATH=$PATH:$HOME/ardupilot/Tools/autotest' >> ~/.bashrc
echo 'export PATH=/usr/lib/ccache:$PATH' >> ~/.bashrc
source ~/.bashrc
```

Alternatif olarak .bashrc dosyasına manuel olarak da ekleyebilirsiniz:

```bash
export PATH=$PATH:$HOME/ardupilot/Tools/autotest
export PATH=/usr/lib/ccache:$PATH
```

---

## Kurulumun Test Et

Aşağıdaki komut ile simülasyon ortamını başlatın:

```bash
cd ~/ardupilot/ArduCopter
sim_vehicle.py -v ArduCopter --console --map

```

Eğer başarılı bir şekilde çalışıyorsa sonraki adıma geçebilirsiniz

---


## Kaynaklar
- https://ardupilot.org/dev/docs/building-setup-linux.html
---



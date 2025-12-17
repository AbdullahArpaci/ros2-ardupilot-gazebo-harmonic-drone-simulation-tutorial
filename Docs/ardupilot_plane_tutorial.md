# Ardupilot (ArduPlane) Installation & SITL Tutorial
Bu döküman ArduPlane (sabit kanat) için ArduPilot kurulum ve SITL test adımlarını içerir.

---

## Prerequisites
- **Ubuntu 22.04 (Jammy Jellyfish)** önerilir.
- Kurulum VM veya doğrudan donanıma yapılabilir.
- Güncel paketler:

```bash
sudo apt update && sudo apt upgrade -y
```

---

## Installation

### [1] Git ve bağımlılıkların yüklenmesi

```bash
sudo apt-get update
sudo apt-get install -y git gitk git-gui
```

```bash
sudo apt install -y git wget python3 python3-pip python3-dev python3-opencv \
python3-numpy python3-serial python3-future python3-lxml python3-yaml \
python3-matplotlib python3-pyqt5 python3-pygame python3-empy openjdk-11-jdk \
build-essential ccache gawk python3-mavproxy
```

> Not: Bazı sistemlerde `python3-mavproxy` paket adı farklı olabilir. Bu repo zaten MAVProxy’yi genelde pip ile de kurabiliyor.

---

### [2] ArduPilot deposunun klonlanması

```bash
cd ~
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
git submodule update --init --recursive
```

---

### [3] Geliştirme ortamının ayarlanması

```bash
Tools/environment_install/install-prereqs-ubuntu.sh -y
. ~/.profile
```

---

### [4] ArduPlane’in derlenmesi (SITL)
Copter yerine Plane derleyeceğiz.

```bash
cd ~/ardupilot
./waf configure --board sitl
./waf plane
```

---

### [5] Ortam değişkenlerinin tanımlanması

```bash
echo 'export PATH=$PATH:$HOME/ardupilot/Tools/autotest' >> ~/.bashrc
echo 'export PATH=/usr/lib/ccache:$PATH' >> ~/.bashrc
source ~/.bashrc
```

---

## Kurulumun test edilmesi (ArduPlane SITL)

### Temel çalıştırma
Copter dokümanındaki komut burada değişir:
- `ArduCopter` yerine **`ArduPlane`**
- `-v ArduCopter` yerine **`-v ArduPlane`**

```bash
cd ~/ardupilot/ArduPlane
sim_vehicle.py -v ArduPlane --console --map
```

### Faydalı başlangıç seçenekleri
- Hazır bir airframe/plane modeli seçmek için (örnek):

```bash
sim_vehicle.py -v ArduPlane -f plane --console --map
```

> `-f` profilleri ortamda farklı isimlerde olabilir; `sim_vehicle.py -h` ile mevcutları görebilirsin.

---

## Plane’e özgü “ne değişir?” özeti
Bu repo seviyesinde **kurulum adımlarının çoğu aynı**; farklar daha çok derleme hedefi ve test/parametre tarafında.

### 1) Derleme hedefi
- Copter: `./waf copter`
- Plane: `./waf plane`

### 2) Test çalıştırma yolu
- Copter: `cd ~/ardupilot/ArduCopter` + `sim_vehicle.py -v ArduCopter ...`
- Plane: `cd ~/ardupilot/ArduPlane` + `sim_vehicle.py -v ArduPlane ...`

### 3) Uçuş mantığı / modlar
Plane’de tipik modlar ve beklentiler:
- **MANUAL**: Stabilizasyon yok (tam pilotaj)
- **FBWA/FBWB**: Stabilize + bank/pitch limitleri (sabit kanatta çok kullanılır)
- **CRUISE**: Hız/heading tutma
- **AUTO**: Görev (mission) uçuşu
- **RTL**: Eve dönüş

### 4) Kalkış/iniş ve “TECS”
Plane’de enerji yönetimi ve throttle/pitch ilişkisi önemli:
- TECS (Total Energy Control System) Plane’e özgü temel kontrol mantığıdır.
- Copter’daki “altitude hold” yaklaşımından farklıdır.

### 5) Sensör / kontrol yüzeyleri
Plane tarafında servo çıkışları (aileron/elevator/rudder/throttle) ve yönler kritik:
- Servo reversals
- Trim ayarları
- Kanat tipi (flying wing vs classic)

---

## Kaynaklar
- https://ardupilot.org/dev/docs/building-setup-linux.html
- https://ardupilot.org/plane/

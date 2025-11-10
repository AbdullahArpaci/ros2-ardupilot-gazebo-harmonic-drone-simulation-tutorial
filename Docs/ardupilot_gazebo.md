# Ardupilot + Gazebo Harmonic
Bu döküman, ArduPilot ile Gazebo Harmonic arasında bağlantı kurmak isteyen kullanıcılar içindir.
ROS 2 entegrasyonu olmadan da kullanılabilir.
---

## Prerequisites
- ArduPilot kurulum adımları tamamlanmış olmalıdır.[ArduPilot Kurulum Adımları](./ardupilot_tutorial.md)
- Gazebo Harmonic daha önce kurulmuş olmalıdır.[Gazebo Harmonic Kurulum Adımları](./gazebo_harmonic_tutorial.md)
- Ubuntu 22.04 (Jammy Jellyfish) önerilir.  
- Kurulum, sanal makine (VMware/VirtualBox) veya doğrudan donanıma yapılabilir.
- ~/.bashrc içinde ArduPilot Tools/autotest dizininin PATH’e eklendiğinden emin olun
```bash
echo 'export PATH=$PATH:$HOME/ardupilot/Tools/autotest' >> ~/.bashrc
source ~/.bashrc
```
- Güncel sistem paketleri için terminalde şu komutu çalıştırın:
```bash
sudo apt update && sudo apt upgrade -y
```

---

## Installation

### [1] Ek kaynakların kurulumu 
Gazebo Harmonic ile ArduPilot arasında iletişim sağlamak için gerekli bağımlılıklar yüklenir.
```bash
sudo apt update
sudo apt install libgz-sim8-dev rapidjson-dev
sudo apt install libopencv-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl
```

---

### [2] Gazebo çalışma alanının kurulması
Gazebo ile ArduPilot arasında köprü görevi görecek olan ardupilot_gazebo eklentisini derlemek için ayrı bir çalışma alanı oluşturun:
```bash
mkdir -p gz_ws/src && cd gz_ws/src
git clone https://github.com/ArduPilot/ardupilot_gazebo
```

---

### [3] Derleme
ArduPilot-Gazebo eklentisini derleyin. Bu işlem, Gazebo’nun sistem sürümünü (ör. Harmonic) tespit eder ve uygun API ile bağlantı kurar.
```bash
export GZ_VERSION=harmonic
cd ardupilot_gazebo
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo
make -j4
```

---


### [4] Ortam Değişkenlerinin Tanımlanması
Gazebo’nun ArduPilot eklentisini ve modellerini tanıyabilmesi için bu dizinleri ortam değişkenlerine ekleyin.
```bash
echo 'export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/gz_ws/src/ardupilot_gazebo/build:${GZ_SIM_SYSTEM_PLUGIN_PATH}' >> ~/.bashrc
echo 'export GZ_SIM_RESOURCE_PATH=$HOME/gz_ws/src/ardupilot_gazebo/models:$HOME/gz_ws/src/ardupilot_gazebo/worlds:${GZ_SIM_RESOURCE_PATH}' >> ~/.bashrc
source ~/.bashrc
```

Veya manuel olarak kendinizde .bashrc dosyasına girerek aşağıdaki dosya yolunu ekleyebilirsiniz:

```bash
export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/gz_ws/src/ardupilot_gazebo/build:$GZ_SIM_SYSTEM_PLUGIN_PATH
export GZ_SIM_RESOURCE_PATH=$HOME/gz_ws/src/ardupilot_gazebo/models:$HOME/gz_ws/src/ardupilot_gazebo/worlds:$GZ_SIM_RESOURCE_PATH
```

---

### [5] Gazebo Entegrasyon Testi

Aşağıdaki adımlar ArduPilot ve Gazebo Harmonic entegrasyonunun başarılı olduğunu doğrular:

```bash
# Terminal 1
gz sim -v4 -r iris_runway.sdf
```
```bash
# Terminal 2
sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON --map --console
```

---


### Kaynaklar
- https://github.com/ArduPilot/ardupilot_gazebo  
- https://gazebosim.org/docs/harmonic/  
- https://ardupilot.org/dev/docs/sitl-with-gazebo.html  
---



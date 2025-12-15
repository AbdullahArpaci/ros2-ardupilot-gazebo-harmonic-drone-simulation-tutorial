# ArduPilot + Gazebo Harmonic + ROS 2 Simülasyon Kurulumu

Bu döküman; **ArduPilot** uçuş yığını, **Gazebo Harmonic** simülasyon ortamı ve **ROS 2 (Humble)** entegrasyonunun uçtan uca kurulumunu açıklar.

> ℹ️ **Doküman Yapısı**
>
> 1. ArduPilot + ROS 2 DDS altyapısının kurulması
> 2. Gazebo kullanılmadan ROS 2 ↔ SITL bağlantısının doğrulanması
> 3. Gazebo Harmonic görsel simülasyon entegrasyonu

---

## 🛠️ Gereksinimler (Prerequisites)

Kuruluma başlamadan önce aşağıdaki adımların tamamlandığından emin olun:

* [x] [ArduPilot Kurulum Adımları](./ardupilot_tutorial.md)
* [x] [Gazebo Harmonic Kurulum Adımları](./gazebo_harmonic_tutorial.md)
* [x] [ROS 2 Humble Kurulum Adımları](./ros2_tutorial.md)
* **İşletim Sistemi**: Ubuntu 22.04 LTS (Jammy Jellyfish)

> [!WARNING]
> **Sanal Makine Kullanıcıları İçin**
> Gazebo Harmonic, GPU hızlandırma gerektirir. VMware / VirtualBox üzerinde düşük FPS veya çökme yaşanabilir. Mümkünse **dual‑boot** veya doğrudan donanım kurulumu önerilir.

---

## 🏗️ Çalışma Alanı Kurulumu (Workspace Setup)

Bu aşamada ArduPilot ve ROS 2 entegrasyonu için `ardu_ws` çalışma alanı oluşturulacaktır.

### [1] Çalışma Alanının Oluşturulması

```bash
mkdir -p ~/ardu_ws/src
cd ~/ardu_ws
vcs import --recursive --input  https://raw.githubusercontent.com/ArduPilot/ardupilot/master/Tools/ros2/ros2.repos src
```

---

### [2] Bağımlılıkların Yüklenmesi (rosdep)

```bash
cd ~/ardu_ws
sudo apt update
rosdep update
source /opt/ros/humble/setup.bash
rosdep install --from-paths src --ignore-src -r -y
```

---

### [3] Micro‑XRCE‑DDS‑Gen Kurulumu

ArduPilot ↔ ROS 2 DDS mesajlaşması için gereklidir.

```bash
sudo apt install default-jre
cd ~/ardu_ws
git clone --recurse-submodules https://github.com/ardupilot/Micro-XRCE-DDS-Gen.git
cd Micro-XRCE-DDS-Gen
./gradlew assemble
echo "export PATH=\$PATH:$PWD/scripts" >> ~/.bashrc
source ~/.bashrc
```

---

### [4] Kurulumun Doğrulanması

```bash
microxrceddsgen -help
```

---

### [5] Workspace Derleme

> ℹ️ Bu adım, yalnızca ArduPilot DDS entegrasyonu için gerekli minimum paketleri derler.

```bash
cd ~/ardu_ws
colcon build --packages-up-to ardupilot_dds_tests
```

Hata durumunda:

```bash
colcon build --packages-up-to ardupilot_dds_tests --event-handlers=console_cohesion+
```

---

## 🔗 ROS 2 ↔ SITL Entegrasyon Testi (Gazebo’suz)

### [1] Ortamın Hazırlanması

```bash
source /opt/ros/humble/setup.bash
cd ~/ardu_ws/
colcon build --packages-up-to ardupilot_sitl
source install/setup.bash
```

---

### [2] SITL Başlatma

```bash
ros2 launch ardupilot_sitl sitl_dds_udp.launch.py \
transport:=udp4 \
synthetic_clock:=True \
wipe:=False \
model:=quad \
speedup:=1 \
slave:=0 \
instance:=0 \
defaults:=$(ros2 pkg prefix ardupilot_sitl)/share/ardupilot_sitl/config/default_params/copter.parm,$(ros2 pkg prefix ardupilot_sitl)/share/ardupilot_sitl/config/default_params/dds_udp.parm \
sim_address:=127.0.0.1 \
master:=tcp:127.0.0.1:5760 \
sitl:=127.0.0.1:5501
```

> ℹ️ **Minimal Test (Önerilen)**

```bash
ros2 launch ardupilot_sitl sitl_dds_udp.launch.py model:=quad
```

---

### [3] Veri Akışının Doğrulanması

```bash
source ~/ardu_ws/install/setup.bash
ros2 node list
ros2 node info /ap
ros2 topic echo /ap/geopose/filtered
```

---

## 🧯 Sorun Giderme (Troubleshooting)

### ROS 2 Topic’lerinden Veri Gelmiyor

```bash
echo 'export PATH=$PATH:$HOME/ardu_ws/src/ardupilot/Tools/autotest' >> ~/.bashrc
source ~/.bashrc
sim_vehicle.py -w -v ArduCopter --console -DG --enable-dds
```

MAVProxy konsolunda:

```bash
param set DDS_ENABLE 1
param save
reboot
```

---

### Alternatif Bağlantı – MAVProxy

> ROS 2 çalışmasa bile ArduPilot’un ayakta olduğunu doğrulamak için kullanılır.

```bash
mavproxy.py --console --map --aircraft test --master=:14550
```

---

## Gazebo Harmonic Entegrasyonu (ardupilot_gz)

### [1] Gazebo Paketlerinin İndirilmesi

```bash
cd ~/ardu_ws
vcs import --input https://raw.githubusercontent.com/ArduPilot/ardupilot_gz/main/ros2_gz.repos --recursive src
echo 'export GZ_VERSION=harmonic' >> ~/.bashrc
source ~/.bashrc
```

---

### [2] Gazebo APT Kaynakları

```bash
sudo apt install wget
wget https://packages.osrfoundation.org/gazebo.gpg -O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
sudo apt update
```

---

### [3] rosdep Gazebo Harmonic Eşleşmesi

> ℹ️ ROS 2 Humble varsayılan olarak Gazebo Classic kullanır. Bu adım Harmonic uyumu sağlar.

```bash
sudo wget https://raw.githubusercontent.com/osrf/osrf-rosdep/master/gz/00-gazebo.list -O /etc/ros/rosdep/sources.list.d/00-gazebo.list
rosdep update
```

---

### [4] Bağımlılıkların Yüklenmesi

```bash
cd ~/ardu_ws
source /opt/ros/humble/setup.bash
sudo apt update
rosdep update
rosdep install --from-paths src --ignore-src -y
```

---

## Derleme ve Gazebo Testleri

```bash
cd ~/ardu_ws
colcon build --packages-up-to ardupilot_gz_bringup
source install/setup.bash
```

### Örnek Simülasyonlar

* **iris_runway** – Açık alan pist (ilk test için önerilir)

```bash
ros2 launch ardupilot_gz_bringup iris_runway.launch.py
```

* **iris_maze** – Kapalı alan / engelli ortam

```bash
ros2 launch ardupilot_gz_bringup iris_maze.launch.py
```

* **wildthumper_playpen** – Kara aracı (UGV)

```bash
ros2 launch ardupilot_gz_bringup wildthumper_playpen.launch.py
```

---

## Bu Doküman Kimler İçin?

* ROS 2 Humble temel bilgisine sahip olanlar
* ArduPilot SITL kullanmış olanlar
* Gazebo Classic yerine **Gazebo Harmonic** kullanmak isteyenler
* DDS tabanlı ArduPilot ↔ ROS 2 entegrasyonu kurmak isteyenler

---

## Kaynaklar (References)

Bu doküman hazırlanırken aşağıdaki resmi ve güvenilir kaynaklardan yararlanılmıştır:

* **ArduPilot ROS 2 Resmi Dokümantasyonu**
  [https://ardupilot.org/dev/docs/ros2.html](https://ardupilot.org/dev/docs/ros2.html)

* **ArduPilot Developer Documentation (ROS 2 Bölümü)**
  [https://ardupilot.org/dev/docs/ros2.html](https://ardupilot.org/dev/docs/ros2.html)

---

## Not

Bu döküman **MAVROS yerine DDS tabanlı modern ArduPilot entegrasyonunu** hedefler ve araştırma / akademik projeler için uygundur.

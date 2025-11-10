# ROS 2 Humble Installation Tutorial
Bu döküman ROS 2 + Ardupilot + Gazebo Entegrasyonu için gereken ilk kurlum adımıdır.

---

## Prerequisites
- Ubuntu 22.04 (Jammy Jellyfish) önerilir.  
  (ROS 2 Humble, yalnızca Ubuntu 22.04 sürümünde resmi olarak desteklenir.)  
- Kurulum, sanal makine (VMware/VirtualBox) veya doğrudan donanıma yapılabilir.  
- Güncel sistem paketleri için terminalde şu komutu çalıştırın:
```bash
sudo apt update && sudo apt upgrade -y
```

---

## Installation

### [1] Lokal konfigurasyonu
ROS 2'nin düzgün çalışabilmesi için sistemin UTF-8 formatında olması gerekmektedir:

```bash
locale
```
Eğer Çıktı UTF-8 değilse aşağıdaki kodları çalıştırın UTF 8 ise Adım 2 ye geçebilirsiniz.

```bash
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale
```

---

### [2] ROS 2 APT Deposunun Eklenmesi
ROS 2 paketlerini sistemimize yükleyebilmek için resmi APT deposunu ekliyoruz.

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install curl -y

export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```

---

### [3] Geliştirme ve Test Araçlarının Kurulumu
ROS 2 projelerinin derlenmesi, test edilmesi ve kalite kontrolü için gerekli Python araçlarını yükleyin:

```bash
sudo apt update && sudo apt install -y   python3-flake8-docstrings   python3-pip   python3-pytest-cov   ros-dev-tools

sudo apt install -y   python3-flake8-blind-except   python3-flake8-builtins   python3-flake8-class-newline   python3-flake8-comprehensions   python3-flake8-deprecated   python3-flake8-import-order   python3-flake8-quotes   python3-pytest-repeat   python3-pytest-rerunfailures
```

---

### [4] ROS 2 Kaynak Kodlarının İndirilmesi
Kurulum dizinini oluşturup ROS 2 kaynak kodlarını içe aktarın:

```bash
mkdir -p ~/ros2_humble/src
cd ~/ros2_humble
vcs import --input https://raw.githubusercontent.com/ros2/ros2/humble/ros2.repos src
```

Tüm sistem paketlerini güncelleyin::

```bash
sudo apt upgrade
```

---

### [5] Bağımlılıkların Kurulumu (rosdep)
rosdep aracı, ROS 2’nin sistem bağımlılıklarını otomatik olarak kurar.

```bash
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
```

---

### [6] ROS 2'nin Derlenmesi
Derleme işlemini colcon ile gerçekleştirin:

```bash
cd ~/ros2_humble/
colcon build --symlink-install
```

Bu işlem sisteminizin performansına göre uzun sürebilir.

---

### [7] Ortam Değişkenlerinin Tanımlanması
ROS 2 ortamının her terminal açılışında otomatik yüklenmesi için aşağıdaki satırı .bashrc dosyasına ekleyin:

```bash
echo '. ~/ros2_humble/install/local_setup.bash' >> ~/.bashrc
source ~/.bashrc
```

Veya manuel olarak kendinizde .bashrc dosyasına girerek aşağıdaki dosya yolunu ekleyebilirsiniz:

```bash
. ~/ros2_humble/install/local_setup.bash
```

---

## Kurulumun Test Et


```bash
ros2 --version
```
Yukarıdaki satırı terminalde çalıştırdığında çıktı aşağıdaki gibi değilse kurulumda bir hata vardır.

```
ros2 humble
```

Ros2 iletişim testi:

```bash
# Terminal 1
ros2 run demo_nodes_cpp talker

# Terminal 2
ros2 run demo_nodes_cpp listener
```

Eğer başarılı bir şekilde çalışıyorsa sonraki adıma geçebilirsiniz

---


## Kaynaklar
- https://docs.ros.org/en/humble/Installation.html  
- https://github.com/ros2/ros2  
- https://index.ros.org/doc/ros2/  

---



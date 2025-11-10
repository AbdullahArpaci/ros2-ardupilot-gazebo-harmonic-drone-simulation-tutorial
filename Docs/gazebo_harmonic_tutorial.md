# Gazebo Installation Tutorial
Bu döküman, ROS 2 + Ardupilot + Gazebo entegrasyonu için gereken **ilk kurulum adımıdır**.  
(Not: İlk kurulum olarak ROS 2 kurulmuş olması da uyumluluk açısından sorun yaratmaz.)

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

### [1] Bağımlılıkların yüklenmesi
Gazebo için gerekli temel bağımlılıkları yükleyin:

```bash
sudo apt-get update
sudo apt-get install curl lsb-release gnupg
```


---

### [2] Gazebonun kurulması
Gazebo’nun (Harmonic) güncel sürümünü yükleyin:

```bash
sudo curl https://packages.osrfoundation.org/gazebo.gpg --output /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] https://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
sudo apt-get update
sudo apt-get install gz-harmonic
```

---

### [3] Kurulumu test etme
Kurulumu Test Etme:

```bash
gz sim
```


## Kaynaklar
- https://gazebosim.org/docs/harmonic/ 
---



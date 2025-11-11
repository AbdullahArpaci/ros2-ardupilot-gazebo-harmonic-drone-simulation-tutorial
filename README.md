# Drone Simulation & Integration Tutorials

Bu depo, **ROS 2 Humble**, **Gazebo Harmonic**, **ArduPilot** ve **ArduPilot + Gazebo entegrasyonu** konularında adım adım kurulum ve yapılandırma rehberlerini içerir.  
Her bir rehber, sistemin belirli bir bileşeninin kurulumu veya entegrasyonuna odaklanır.  
Tüm dökümanlar düzenli olarak güncellenmekte ve yeni bileşenlerle genişletilmeye devam etmektedir. 🚀

---

## İçerik Rehberi

### 🔹 1. ROS 2 Humble Kurulumu
ROS 2 ortamını Ubuntu 22.04 üzerinde baştan sona kurmak için gerekli adımlar.  
> **Kapsam:** locale ayarları, bağımlılıklar, colcon build, ortam değişkenleri.  
📄 [Kurulum Dökümanı → `docs/ros2_humble_tutorial.md`](./Docs/ros2_tutorial.md)

---

### 🔹 2. Gazebo Harmonic Kurulumu
Gazebo Harmonic’in kurulumu ve temel testleri.  
> **Kapsam:** bağımlılık kurulumu, depo ekleme, GUI testleri (`gz sim`).  
📄 [Kurulum Dökümanı → `docs/gazebo_harmonic_tutorial.md`](./Docs/gazebo_harmonic_tutorial.md)

---

### 🔹 3. ArduPilot Kurulumu
ArduPilot’un SITL (Software-In-The-Loop) modunda derlenmesi ve test edilmesi.  
> **Kapsam:** bağımlılıklar, depo klonlama, waf derleme, ortam değişkenleri, MAVProxy testi.  
📄 [Kurulum Dökümanı → `docs/ardupilot_tutorial.md`](./Docs/ardupilot_tutorial.md)

---

### 🔹 4. ArduPilot + Gazebo Harmonic Entegrasyonu
ArduPilot ve Gazebo Harmonic arasında bağlantı kurulumu.  
> **Kapsam:** ardupilot_gazebo plugin kurulumu, ortam değişkenleri, entegrasyon testi (`sim_vehicle.py + gz sim`).  
📄 [Kurulum Dökümanı → `docs/ardupilot_gazebo_tutorial.md`](./docs/ardupilot_gazebo.md)

---

## Ek Bilgiler
- Tüm rehberler **Ubuntu 22.04 (Jammy Jellyfish)** için hazırlanmıştır.  
- Her adım, **ROS 2 Humble**, **Gazebo Harmonic** ve **ArduPilot** arasındaki uyumluluk gözetilerek test edilmiştir.  
- Rehberlerdeki komutlar bash terminali üzerinden uygulanmalıdır.

---

## Geliştirme Durumu
Bu proje hâlâ gelişim aşamasındadır.  
Yakında eklenecek bölümler:
- 🔸 Çoklu UAV (Swarm) senaryoları  
- 🔸 ROS 2 – ArduPilot DDS entegrasyonu  
- 🔸 Gerçek zamanlı uçuş veri analizi  

Katkıda bulunmak veya hata bildirmek isterseniz PR (Pull Request) gönderebilirsiniz. 🤝

---


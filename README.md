# ROS2 ArduPilot Gazebo Harmonic Drone Simulation Tutorial (Ubuntu 22.04)

![ROS 2](https://img.shields.io/badge/ROS_2-Humble-blue) ![Gazebo](https://img.shields.io/badge/Gazebo-Harmonic-orange) ![ArduPilot](https://img.shields.io/badge/ArduPilot-SITL-red) ![License](https://img.shields.io/badge/License-MIT-green)

This repository provides a **step-by-step tutorial** for setting up **ROS2 Humble, ArduPilot SITL, and Gazebo Harmonic drone simulations on Ubuntu 22.04**. It is designed for developers working on **autonomous drones, robotics, and multi-UAV swarm simulations**.

Bu depo, **Ubuntu 22.04** üzerinde **ROS 2 Humble**, **Gazebo Harmonic** ve **ArduPilot** kullanarak **İHA (Drone) simülasyonu** geliştirmek isteyenler için kapsamlı, adım adım kurulum ve yapılandırma rehberlerini içerir.

> **🇬🇧 English Summary:** This repository provides step-by-step tutorials for installing and integrating **ROS 2 Humble**, **Gazebo Harmonic**, and **ArduPilot** on Ubuntu 22.04. It covers SITL setups, multi-UAV swarm simulations, and environment configurations.

Bu projede; **SITL (Software In The Loop)** testi, **Multi-UAV (Çoklu Drone/Sürü)** simülasyonu ve **Mavlink** haberleşmesi gibi ileri seviye konular detaylandırılmıştır. Otonom sistemler, gömülü yazılım ve robotik alanında çalışan geliştiriciler için referans niteliğindedir.

---

## Simülasyon Önizlemesi
![Multi-UAV Simulation Example](/Images/multi_uav.png)

---

## İçerik Rehberi

### 🔹 1. ROS 2 Humble Kurulumu (Ubuntu 22.04)
Robot İşletim Sistemi (ROS 2) ortamının eksiksiz kurulumu.
> **Kapsam:** Locale ayarları, ROS 2 key ekleme, `colcon build` kullanımı, `.bashrc` ve ortam değişkenleri yapılandırması.  
📄 [Kurulum Dökümanı → `docs/ros2_humble_tutorial.md`](./Docs/ros2_tutorial.md)

---

### 🔹 2. Gazebo Harmonic Kurulumu
Yeni nesil Gazebo simülasyon ortamının kurulumu ve `gz sim` testleri.
> **Kapsam:** Gazebo Garden/Harmonic farkları, bağımlılık kurulumu, GUI testleri.  
📄 [Kurulum Dökümanı → `docs/gazebo_harmonic_tutorial.md`](./Docs/gazebo_harmonic_tutorial.md)

---

### 🔹 3. ArduPilot SITL Kurulumu
ArduPilot uçuş kontrolcüsünün simülasyon modunda (SITL) derlenmesi.
> **Kapsam:** Waf build sistemi, ArduCopter derleme, MAVProxy ile bağlantı testi.  
📄 [Kurulum Dökümanı → `docs/ardupilot_tutorial.md`](./Docs/ardupilot_tutorial.md)

---

### 🔹 4. ArduPilot + Gazebo Harmonic Entegrasyonu (Plugin)
ArduPilot ve Gazebo'nun `ardupilot_gazebo` eklentisi ile haberleşmesi.
> **Kapsam:** JSON model yapılandırması, `sim_vehicle.py` parametreleri, entegrasyon testi.  
[Kurulum Dökümanı → `docs/ardupilot_gazebo_tutorial.md`](./Docs/ardupilot_gazebo.md)

---

### 🔹 5. Çoklu Dron (Swarm) Simülasyonu
Aynı anda birden fazla İHA'nın simüle edilmesi ve sürü algoritmaları için altyapı.
> **Kapsam:** Model çoğaltma (spawning), SDF dünya dosyası düzenleme, fizik optimizasyonları, çoklu MAVLink port yönetimi.  
[Kurulum Dökümanı → `docs/multi_uav_simulation.md`](./Docs/multi_uav_ardupilot_gazebo.md)

---

## Teknik Detaylar ve Uyumluluk
* **İşletim Sistemi:** Ubuntu 22.04 LTS (Jammy Jellyfish)
* **ROS Sürümü:** ROS 2 Humble Hawksbill
* **Simülatör:** Gazebo Harmonic
* **Uçuş Kontrol:** ArduPilot (Copter & Plane)

---


## Katkıda Bulunma
Hata bildirmek veya yeni bir özellik eklemek isterseniz lütfen bir **Issue** açın veya **Pull Request** gönderin.

**Anahtar Kelimeler:** *ROS 2 Tutorial, Gazebo Harmonic, ArduPilot SITL, Drone Simulation, Ubuntu 22.04, İHA Simülasyon, Swarm Intelligence, Robotik Kodlama.*

# ROS 2 + ArduPilot + Gazebo Harmonic Drone Simulation Tutorial (Ubuntu 22.04)

![ROS 2](https://img.shields.io/badge/ROS_2-Humble-blue)
![Gazebo](https://img.shields.io/badge/Gazebo-Harmonic-orange)
![ArduPilot](https://img.shields.io/badge/ArduPilot-SITL-red)
![License](https://img.shields.io/badge/License-MIT-green)

Bu doküman, **Ubuntu 22.04** üzerinde **ROS 2 Humble**, **ArduPilot (SITL)** ve **Gazebo Harmonic** kullanarak **drone / İHA simülasyonu** kurmak ve çalıştırmak isteyenler için hazırlanmıştır. Doküman; tekli ve çoklu İHA (swarm) senaryoları, DDS tabanlı ROS 2 entegrasyonu ve Gazebo görsel simülasyonunu kapsar.

> 🇬🇧 **English Summary**
> This document provides a step-by-step guide for integrating **ROS 2 Humble**, **ArduPilot SITL**, and **Gazebo Harmonic** on Ubuntu 22.04. It covers DDS-based communication, single and multi-UAV simulations, and Gazebo visualization.

---

## 🎥 Simülasyon Önizlemesi

![Multi-UAV Simulation Example](../Images/multi_uav.png)

---

## 📚 İçerik Rehberi

### 🔹 1. ROS 2 Humble Kurulumu (Ubuntu 22.04)

ROS 2 çalışma ortamının eksiksiz kurulumu ve yapılandırılması.

📄 **Doküman:**
[`ros2_tutorial.md`](./ros2_tutorial.md)

---

### 🔹 2. Gazebo Harmonic Kurulumu

Yeni nesil Gazebo simülasyon ortamının kurulumu ve doğrulanması.

📄 **Doküman:**
[`gazebo_harmonic_tutorial.md`](./gazebo_harmonic_tutorial.md)

---

### 🔹 3. ArduPilot SITL Kurulumu

ArduPilot uçuş kontrol yazılımının simülasyon modunda derlenmesi.


📄 **Doküman:**
[`ardupilot_tutorial.md`](./ardupilot_tutorial.md)

---

### 🔹 4. ArduPilot + Gazebo Harmonic Entegrasyonu

ArduPilot’un **ardupilot_gz** köprü paketleri ile Gazebo Harmonic ortamında çalıştırılması.

📄 **Doküman:**
[`ardupilot_gazebo.md`](./ardupilot_gazebo.md)

---

### 🔹 5. Çoklu İHA (Swarm) Simülasyonu

Aynı anda birden fazla drone ile sürü simülasyonlarının kurulması.

---

### 🔹 6. ROS 2 + ArduPilot DDS Entegrasyonu

MAVROS kullanılmadan, **DDS tabanlı modern ROS 2 entegrasyonu**.


📄 **Doküman:**
[`ardupilot_ros2_gazebo.md`](./ardupilot_ros2_gazebo.md)

---

## Teknik Detaylar ve Uyumluluk

* **İşletim Sistemi:** Ubuntu 22.04 LTS (Jammy Jellyfish)
* **ROS Dağıtımı:** ROS 2 Humble Hawksbill
* **Simülatör:** Gazebo Harmonic
* **Uçuş Kontrolcüsü:** ArduPilot (Copter & Plane)
* **Haberleşme:** DDS (Micro XRCE-DDS), MAVLink

---

## Kimler İçin Uygun?

* ROS 2 ile otonom sistem geliştirenler
* Drone / İHA simülasyonu yapmak isteyenler
* Gazebo Classic yerine **Gazebo Harmonic** kullanmak isteyenler
* Multi-UAV / swarm algoritmaları üzerinde çalışanlar

---

## Katkıda Bulunma

Katkılar memnuniyetle karşılanır.

* Hata bildirmek için **Issue** açabilirsiniz
* Yeni özellikler için **Pull Request** gönderebilirsiniz

---

## Kaynaklar

* ArduPilot ROS 2 Resmi Dokümantasyonu
  [https://ardupilot.org/dev/docs/ros2.html](https://ardupilot.org/dev/docs/ros2.html)

---

**Anahtar Kelimeler:** ROS 2 Tutorial, Gazebo Harmonic, ArduPilot SITL, Drone Simulation, Ubuntu 22.04, UAV Simulation, Multi-UAV, Swarm Robotics

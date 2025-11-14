# ArduPilot + Gazebo Harmonic ile Çoklu Dron Simülasyonu

Bu döküman, **ArduPilot** ve **Gazebo Harmonic** kullanarak **çoklu dron simülasyonu** oluşturma adımlarını detaylı olarak anlatmaktadır.

---

## Gereksinimler (Prerequisites)

- [ArduPilot kurulum adımları](./ardupilot_tutorial.md) tamamlanmış olmalıdır.
- [Gazebo Harmonic kurulum adımları](./gazebo_harmonic_tutorial.md) tamamlanmış olmalıdır.
- [ArduPilot - Gazebo entegrasyonu](./ardupilot_gazebo.md) yapılmış olmalıdır.
- **Önerilen İşletim Sistemi**: Ubuntu 22.04 (Jammy Jellyfish)
- Kurulum **sanal makine (VMware/VirtualBox)** veya **doğrudan donanıma** yapılabilir.

### PATH Ayarı
`~/.bashrc` dosyasına aşağıdaki satırı ekleyin ve terminalde çalıştırın:

```bash
echo 'export PATH=$PATH:$HOME/ardupilot/Tools/autotest' >> ~/.bashrc
source ~/.bashrc
```
### Sistem Güncelleme
- Güncel sistem paketleri için terminalde şu komutu çalıştırın:
```bash
sudo apt update && sudo apt upgrade -y
```

---

## Installation

### [1] Model dosyaları oluşturma
1. gz_ws/src/ardupilot_gazebo/models dizinine gidin.
2. iris_ardupilot klasörünü kopyalayın.
![models klasörü](/Images/models.png)
3. İsterseniz masaüstüne MultiUav klasörü oluşturun veya bulunduğunuz dizinde işlemere devam edin.
Tavsiye: Ayrı klasörde tutmak ileride düzenleme yaparken büyük kolaylık sağlar.
4. Kopyaladığınız klasörü 3 kez yapıştırın ve isimlerini şu şekilde değiştirin(Ek klasör oluşturmadıysanız models dizininde yapın bu işlemleri):
Drone1
Drone2
Drone3
![Model dosyaları](/Images/models2.png)
5. Her bir DroneX klasöründeki model.sdf dosyasını açın ve en üstteki <model name="..."> satırını klasör ismiyle eşleşecek şekilde güncelleyin.
![Model isim güncelleme](/Images/models3.png)
![Model port Ayarı](/Images/models4.png)
6. Port Çakışmasını Önlemek İçin:
    Her dron, ArduPilot ile Gazebo arasında farklı portlar üzerinden haberleşmelidir.
    Her model.sdf dosyasındaki <plugin name="ArduPilotPlugin"> bölümünü aşağıdaki gibi düzenleyin:
    Dron,fdm_addr,fdm_port_in,fdm_port_out
    Drone1,127.0.0.1,9002,9003
    Drone2,127.0.0.1,9012,9013
    Drone3,127.0.0.1,9022,9023

```bash
<plugin name="ArduPilotPlugin" filename="libArduPilotPlugin.so">
  <fdm_addr>127.0.0.1</fdm_addr>
  <fdm_port_in>9002</fdm_port_in>
  <fdm_port_out>9003</fdm_port_out>
```
Eğer <fdm_port_out> yoksa kendiniz eklemelisiniz.
Her Dron için portları doğru bir şekilde ayarladığınızdan emin olun.


---


### [1] Dünya Dosyası oluşturma
1. gz_ws/src/ardupilot_gazebo/worlds klasörüne gidin.
![Worlds klasörü](/Images/world1.png)
2. iris_runway.sdf dosyasını kopyalayın ve eğer ek bir klasör oluşturduysanız oluşturulan klasöre oluşturulmadı ise bulunulan words dizinine dosyayı yapıştırın 
3. Dosya ismini multi_iris_runway.sdf olarak değiştirin.
![World dosyası isim güncelleme](/Images/worlds2.png)
4. multi_iris_runway.sdf dosyasını açın ve içeriğini aşağıdaki gibi düzenleyin:
- Kopyalanan dünya dosyası tek bir dron içermektedir öncelikle dosyaya daha önceden oluşturmuş olduğumuz Dron modellerini ekleyin.

Dünya dosyamızın en alt kısmını aşağıdaki gibi düzenleyin:
```bash
<include>
    <uri>model://runway</uri>
    <pose>-29 545 0 0 0 0</pose>
</include>

<!-- ***** DRONES (HIGH SPAWN HEIGHT) ***** -->
<include>
    <uri>model://Drone1</uri>
    <pose degrees="true">0 0 0.195 0 0 90</pose>
</include>

<include>
    <uri>model://Drone2</uri>
    <pose degrees="true">4 0 0.195 0 0 90</pose>
</include>

<include>
    <uri>model://Drone3</uri>
    <pose degrees="true">8 0 0.195 0 0 90</pose>
</include>
```
Bu işlem dünya dosyasına modellerin eklenmesini sağlar
![World dosyası isim güncelleme](/Images/worlds3.png)
5. Gazebo CPU tabanlı bir uygulama olmasından kaynaklı sistemi çok fazla yorabilmektedir bu durum simülasyonun doğru bir şekilde çalışmasını engellemektedir bu yüzden bazı optimizasyon ayarları yapmamız gerekir

- Sistemin Physics kısmını aşağıdaki gibi güncelleyin:
```bash
<?xml version="1.0" ?>
<sdf version="1.9">
<world name="iris_runway">

<!-- ***** PHYSICS FIXED ***** -->
<physics name="ode_physics" type="ode">
    <max_step_size>0.001</max_step_size>
    <real_time_factor>1.0</real_time_factor>       
    <real_time_update_rate>1000</real_time_update_rate>
    <ode>
    <solver>
        <type>quick</type>
        <iters>60</iters>
        <sor>1.0</sor>
    </solver>
    <constraints>
        <cfm>0.0</cfm>
        <erp>0.8</erp>
    </constraints>
    </ode>
</physics>

<!-- ***** SYSTEM PLUGINS ***** -->
<plugin filename="gz-sim-physics-system"
    name="gz::sim::systems::Physics" />

<plugin filename="gz-sim-sensors-system"
    name="gz::sim::systems::Sensors">
    <render_engine>ogre</render_engine>
</plugin>

<plugin filename="gz-sim-user-commands-system"
    name="gz::sim::systems::UserCommands" />

<plugin filename="gz-sim-scene-broadcaster-system"
    name="gz::sim::systems::SceneBroadcaster" />

<plugin filename="gz-sim-imu-system"
    name="gz::sim::systems::Imu" />

<plugin filename="gz-sim-navsat-system"
    name="gz::sim::systems::NavSat" />
```
| Parametre                        | Açıklama                                                                                                     |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **max_step_size = 0.001**        | Fizik çözünürlüğünü artırır → titremeyi ciddi azaltır                                                        |
| **real_time_update_rate = 1000** | Simülasyonun saniyede 1000 fizik adımı hesaplamasını sağlar                                                  |
| **ode physics**                  | Harmonic varsayılan fizik motoru DART ve ODE karışık çalışır. ODE daha stabil olduğu için tercih edilmiştir. |
| **solver quick + iters 60**      | Dron titreşimlerini azaltır                                                                                  |
| **ERP 0.8**                      | Nesnelerin yere gömülmesini veya zıplamasını engeller                                                        |

![World dosyası isim güncelleme](/Images/worlds4.png)
---

### [3] Sistem Bağımlılıklarını güncelleme
- Bu kısım eğer ek bir klasör oluşturduysanız Gazebonun dosya yolunu bilmesi için önem arz etmektedir eğer yapmazsanın dünya dosyası açılmayacaktır.

- Eğer ek bir klsaör oluşturmadıysanız yapılan işlemleri bulunduğunuz dizinde uyguladıysanız bu kısmı geçebilirsiniz

~/.bashrc dosyasını açın 
```bash
gedit ~/.bashrc
```
- Dosyanın en alt kısmına aşağıdaki komutları ekleyin ve dosyayı kaydedin
```bash
export GZ_SIM_RESOURCE_PATH=$HOME/Desktop/MultiUav:${GZ_SIM_RESOURCE_PATH}
export GZ_SIM_MODEL_PATH=$HOME/Desktop/MultiUav:${GZ_SIM_MODEL_PATH}

```
-Bu sayede Gazebo:

    Drone1 / Drone2 / Drone3 modellerini

    multi_iris_runway.sdf dosyasını

    otomatik olarak bulabilir.

---
### [4] Simülasyonu Çalıştırma

Terminal 1 – Drone1
```bash
sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON -I0

```
Terminal 2 – Drone2
```bash
sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON -I1

```
Terminal 3 – Drone3
```bash
sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON -I2
```
Terminal 4 – Gazebo dünyayı başlatma
```bash
gz sim -v4 -r multi_iris_runway.sdf
```
![Örnek çıktı](/Images/multi_uav.png)

<!--- 

本文档图片使用以下CARLA设置拍摄：

城镇：Town 10

车辆位置：Transform(Location(x=-46.885479, y=20.083447, z=-0.002633), Rotation(pitch=-0.000034, yaw=141.974243, roll=0.000000))

摄像机位置（小型车辆）：Transform(Location(x=-47.696186, y=24.049326, z=1.471929), Rotation(pitch=-10.843717, yaw=-77.215683, roll=0.000139))
摄像机位置（标准车辆）：Transform(Location(x=-48.672256, y=24.830288, z=1.722733), Rotation(pitch=-13.396630, yaw=-75.692039, roll=0.000119))
摄像机位置（大型车辆）：Transform(Location(x=-49.470921, y=27.835310, z=2.931721), Rotation(pitch=-13.396630, yaw=-75.691978, roll=0.000119))

天气设置：
weather.sun_altitude_angle = 50
weather.sun_azimuth_angle = 260
weather.wetness = 10
weather.precipitation = 10
weather.scattering_intensity = 5
weather.mie_scattering_scale = 0.5
weather.rayleigh_scattering_scale = 0.1

摄像机设置：
camera_bp = bp_lib.find('sensor.camera.rgb')
camera_bp.set_attribute('image_size_x', '1920')
camera_bp.set_attribute('image_size_y', '1080')
camera_bp.set_attribute('fstop', '6.0')

车辆设置：
control = vehicle.get_control()
control.steer = -0.25
vehicle.apply_control(control)

--->

# 车辆目录

* __轿车__
	* [__奥迪__ - TT](#奥迪-tt)
	* [__道奇__ - Charger](#道奇-charger)
	* [__道奇__ - 警用Charger](#道奇-警用charger)
	* [__福特__ - Crown（出租车）](#福特-crown出租车)
	* [__林肯__ - MKZ](#林肯-mkz)
	* [__奔驰__ - 轿跑](#奔驰-轿跑)
	* [__Mini__ - Cooper](#mini-cooper-s)
	* [__日产__ - Patrol](#日产-patrol)
* __卡车__
	* [__CARLA Motors__ - 消防车](#carla-motors-消防车)
	* [__CARLA Motors__ - CarlaCola](#carla-motors-carlacola)
* __厢式货车__
	* [__福特__ - 救护车](#福特-救护车)
	* [__奔驰__ - Sprinter](#奔驰-sprinter)
* __巴士__
	* [__三菱__ - Fusorosa](#三菱-fusorosa)

---

## 在模拟器中查看车辆

要检查目录中的车辆，请使用以下代码，从下方车辆详情中获取蓝图ID并粘贴到`bp_lib.find('蓝图ID')`行：

```py
client = carla.Client('localhost', 2000)
world = client.get_world()
bp_lib = world.get_blueprint_library()
spectator = world.get_spectator()

# 设置车辆变换
vehicle_loc = carla.Location(x=-46.9, y=20.0, z=0.2)
vehicle_rot = carla.Rotation(pitch=0.0, yaw=142.0, roll=0.0)
vehicle_trans = carla.Transform(vehicle_loc,vehicle_rot)

# 在此处粘贴蓝图ID：
vehicle_bp = bp_lib.find('vehicle.lincoln.mkz') 

# 设置视角变换
camera_loc = carla.Location(x=-48.7, y=24.8, z=1.7)
camera_rot = carla.Rotation(pitch=-13.4, yaw=-75.7, roll=0.0)
camera_trans = carla.Transform(camera_loc,camera_rot)

# 生成车辆
vehicle = world.spawn_actor(vehicle_bp, vehicle_trans)

# 移动观察者视角
spectator.set_transform(camera_trans)
```

生成新车辆前请记得销毁当前车辆以避免碰撞：
```py
vehicle.destroy()
```

---

## 轿车
### 奥迪 - TT

![audi_tt](../img/catalogue/vehicles/audi_tt.png)

* __制造商__：奥迪
* __型号__：TT
* __类别__：标准
* __世代编号__：1
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.ue4.audi.tt</span>

* __基础类型__：轿车
* __配备车灯__：<span style="color:#f16c6c;">否</span>
* __可开车门__：<span style="color:#f16c6c;">否</span>

### 宝马 - Gran Tourer

![bmw_grandtourer](../img/catalogue/vehicles/bmw_grantourer.png)

* __制造商__：宝马
* __型号__：Gran Tourer
* __类别__：紧凑型
* __世代编号__：1
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.ue4.bmw.grantourer</span>

* __基础类型__：轿车
* __配备车灯__：<span style="color:#f16c6c;">否</span>
* __可开车门__：<span style="color:#f16c6c;">否</span>

### 雪佛兰 - Impala

![chevrolet_impala](../img/catalogue/vehicles/chevrolet_impala.png)

* __制造商__：雪佛兰
* __型号__：Impala
* __类别__：标准
* __世代编号__：1
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.ue4.chevrolet.impala</span>

* __基础类型__：轿车
* __配备车灯__：<span style="color:#f16c6c;">否</span>
* __可开车门__：<span style="color:#f16c6c;">否</span>

### 道奇 - Charger

![dodge_charger_2020](../img/catalogue/vehicles/dodge_charger.png)

* __制造商__：道奇
* __型号__：Charger 2020
* __类别__：标准
* __世代编号__：2
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.dodge.charger</span>

* __基础类型__：轿车
* __配备车灯__：<span style="color:#99c635;">是</span>
* __可开车门__：<span style="color:#99c635;">是</span>

### 道奇 - 警用Charger

![dodge_charger_police](../img/catalogue/vehicles/dodge_charger_police.png)

* __制造商__：道奇
* __型号__：警用Charger 2020
* __类别__：标准
* __世代编号__：2
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.dodgecop.charger</span>

* __基础类型__：轿车
* __特殊类型__：紧急车辆
* __配备车灯__：<span style="color:#99c635;">是</span>
* __可开车门__：<span style="color:#99c635;">是</span>

### 福特 - Crown（出租车）

![ford_crown](../img/catalogue/vehicles/taxi_ford.png)

* __制造商__：福特
* __型号__：Crown（出租车）
* __类别__：标准
* __世代编号__：2
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.taxi.ford</span>

* __基础类型__：轿车
* __特殊类型__：出租车
* __配备车灯__：<span style="color:#99c635;">是</span>
* __可开车门__：<span style="color:#99c635;">是</span>

### 福特 - Mustang

![ford_mustang](../img/catalogue/vehicles/ford_mustang.png)

* __制造商__：福特
* __型号__：Mustang
* __类别__：标准
* __世代编号__：1
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.ue4.ford.mustang</span>

* __基础类型__：轿车
* __配备车灯__：<span style="color:#f16c6c;">否</span>
* __可开车门__：<span style="color:#f16c6c;">否</span>

### 林肯 - MKZ

![lincoln_mkz](../img/catalogue/vehicles/lincoln_mkz.png)

* __制造商__：林肯
* __型号__：MKZ
* __类别__：标准
* __世代编号__：2
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.lincoln.mkz</span>

* __基础类型__：轿车
* __配备车灯__：<span style="color:#99c635;">是</span>
* __可开车门__：<span style="color:#99c635;">是</span>

### 奔驰 - 轿跑

![mercedes_coupe](../img/catalogue/vehicles/mercedes_coupe.png)

* __制造商__：奔驰
* __型号__：轿跑
* __类别__：标准
* __世代编号__：1
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.ue4.mercedes.ccc</span>

* __基础类型__：轿车
* __配备车灯__：<span style="color:#f16c6c;">否</span>
* __可开车门__：<span style="color:#f16c6c;">否</span>

### Mini - Cooper S

![mini_cooper](../img/catalogue/vehicles/mini_cooper.png)

* __制造商__：Mini
* __型号__：Cooper S 2021
* __类别__：标准
* __世代编号__：2
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.mini.cooper</span>

* __基础类型__：轿车
* __配备车灯__：<span style="color:#99c635;">是</span>
* __可开车门__：<span style="color:#99c635;">是</span>

### 日产 - Patrol

![nissan_patrol](../img/catalogue/vehicles/nissan_patrol.png)

* __制造商__：日产
* __型号__：Patrol
* __类别__：SUV
* __世代编号__：2
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.nissan.patrol</span>

* __基础类型__：轿车
* __配备车灯__：<span style="color:#99c635;">是</span>
* __可开车门__：<span style="color:#99c635;">是</span>

---

## 卡车
### CARLA Motors - CarlaCola

![carlamotors_carlacola](../img/catalogue/vehicles/carlacola.png)

* __制造商__：CARLA Motors
* __型号__：CarlaCola
* __类别__：卡车
* __世代编号__：1
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.carlacola.actors</span>

* __基础类型__：卡车
* __配备车灯__：<span style="color:#f16c6c;">否</span>
* __可开车门__：<span style="color:#f16c6c;">否</span>

### CARLA Motors - 消防车

![carlamotors_firetruck](../img/catalogue/vehicles/firetruck.png)

* __制造商__：CARLA Motors
* __型号__：消防车
* __类别__：卡车
* __世代编号__：2
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.firetruck.actors</span>

* __基础类型__：卡车
* __特殊类型__：紧急车辆
* __配备车灯__：<span style="color:#99c635;">是</span>
* __可开车门__：<span style="color:#99c635;">是</span>

---

## 厢式货车
### 福特 - 救护车

![ford_ambulance](../img/catalogue/vehicles/ambulance_ford.png)

* __制造商__：福特
* __型号__：救护车
* __类别__：厢式货车
* __世代编号__：2
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.ambulance.ford</span>

* __基础类型__：厢式货车
* __特殊类型__：紧急车辆
* __配备车灯__：<span style="color:#99c635;">是</span>
* __可开车门__：<span style="color:#99c635;">是</span>

### 奔驰 - Sprinter

![mercedes_sprinter](../img/catalogue/vehicles/mercedes_sprinter.png)

* __制造商__：奔驰
* __型号__：Sprinter
* __类别__：厢式货车
* __世代编号__：2
* __蓝图ID__：<span style="color:#00a6ed;">vehicle.sprinter.mercedes</span>

* __基础类型__：厢式货车
* __配备车灯__：<span style="color:#99c635;">是</span>
* __可开车门__：<span style="color:#99c635;">是</span>

---

## 巴士
### 三菱 - Fusorosa

![mitsubishi_fusorosa](../img/catalogue/vehicles/mitsubishi_fusorosa.png)

* __制造商__：三菱
* __型号__：Fusorosa
* __类别__：巴士
* __世代编号__：2
* __蓝图ID__：<span style="color:#00a6ed;">ehicle.fuso.mitsubishi</span>

* __基础类型__：巴士
* __配备车灯__：<span style="color:#99c635;">是</span>
* __可开车门__：<span style="color:#f16c6c;">否</span>

---
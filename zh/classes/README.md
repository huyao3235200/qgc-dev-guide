# 类层次结构（高级）

## (LinkManager)链接管理器类，(LinkInterface)链接接口类

QGC中的“链接”是与载具的特定类型的通信管道，例如串行端口或基于WiFi的UDP。 所有链接的基类都是LinkInterface这个类。 每个链接都在它自己的线程上运行，并将字节发送到MAVLinkProtocol。

LinkManager类所生成对象管理系统中的所有打开链接。 `LinkManager`还通过串行和UDP链接管理自动连接

## MAVLink协议类

系统中有一个MAVLink协议对象。 它的功能是从链接获取传入的字节并将它们转换为MAVLink消息。 MAVLink HEARTBEAT消息是被分发到Multi Vehicle Manager(多机管理类)。 所有MAVLink消息都将关联到与链接相对应的载具。

## (MultiVehicleManager)多机管理类

系统中有一个MultiVehicleManager多机管理类生成的对象, 当它接收到一个新的心跳包(通过心跳包里面的系统ID识别)，它会自动生成一个载具对象，来表示一个新的载具加入到系统。 `MultiVehicleManager`还可以管理系统中所有载具的信息，并处理从一辆主动车辆到另一辆车辆的数据显示与控制切换。

## (Vehicle)载具类

Vehicle类所生成的对象是QGC代码与物理载具通信的主要接口。

注意：还有一个与每个Vehicle相关联的UAS对象，这是一个已弃用的类，并且正逐渐被逐步淘汰，所有功能都转移到Vehicle类。 这里不应该添加新代码。

## (FirmwarePlugin)固件插件类，( FirmwarePluginManager)固件插件管理器类

FirmwarePlugin类用作固件插件的基类。 固件插件包含固件特定代码，因此Vehicle对象相对于它是识别的，支持UI的单个标准接口。

FirmwarePluginManager是一个工厂类，它根据Vehicle类的成员MAV_AUTOPILOT / MAV_TYPE组合创建FirmwarePlugin类的实例。

> **Note**AutoPilotPlugin和AutoPilotPluginManager是不推荐使用的类，它还包含特定于固件的代码。 其中的所有功能都将移至较新的FirmwarePlugin和FirmwarePluginManager实现。 这里不应该添加新代码。
# **dmidecode**

## 1. dmidecode简介

dmidecode允许你在Linux系统下获取有关硬件方面的信息。dmidecode遵循SMBIOS/DMI标准，其输出的信息包括BIOS、系统、主板、处理器、内存、缓存等等。

DMI（Desktop Management Interface,DMI）就是帮助收集电脑系统信息的管理系统，DMI信息的收集必须在严格遵照SMBIOS规范的前提下进行。SMBIOS（System Management BIOS）是主板或系统制造者以标准格式显示产品管理信息所需遵循的统一规范。SMBIOS和DMI是由行业指导机构Desktop Management Task Force(DMTF)起草的开放性的技术标准，其中DMI设计适用于任何的平台和操作系统。

DMI充当了管理工具和系统层之间接口的角色。它建立了标准的可管理系统更加方便了电脑厂商和用户对系统的了解。DMI的主要组成部分是Management Information Format(MIF)数据库。这个数据库包括了所有有关电脑系统和配件的信息。通过DMI，用户可以获取序列号、电脑厂商、串口信息以及其它系统配件信息


## 2. dmidecode的作用

dmidecode的作用是将DMI数据库中的信息解码，以可读的文本方式显示。由于DMI信息可以人为修改，因此里面的信息不一定是系统准确的信息。



## 3. dmidecode命令用法

不带选项执行dmidecode通常会输出所有的硬件信息。dmidecode有个很有用的选项-t，加上"-t type_num"或者"-t keywords"可以查看某个类型信息。假如要获得处理器方面的信息，则可以执行：dmidecode -t processor



	1）type num 全部编码列表
	Type Information
	—————————————
	 0 BIOS
	 1 System
	 2 Base Board
	 3 Chassis
	 4 Processor
	 5 Memory Controller
	 6 Memory Module
	 7 Cache
	 8 Port Connector
	 9 System Slots
	 10 On Board Devices
	 11 OEM Strings
	 12 System Configuration Options
	 13 BIOS Language
	 14 Group Associations
	 15 System Event Log
	 16 Physical Memory Array
	 17 Memory Device
	 18 32-bit Memory Error
	 19 Memory Array Mapped Address
	 20 Memory Device Mapped Address
	 21 Built-in Pointing Device
	 22 Portable Battery
	 23 System Reset
	 24 Hardware Security
	 25 System Power Controls
	 26 Voltage Probe
	 27 Cooling Device
	 28 Temperature Probe
	 29 Electrical Current Probe
	 30 Out-of-band Remote Access
	 31 Boot Integrity Services
	 32 System Boot
	 33 64-bit Memory Error
	 34 Management Device
	 35 Management Device Component
	 36 Management Device Threshold Data
	 37 Memory Channel
	 38 IPMI Device
	 39 Power Supply
	 40 Additional Information
	 41 Onboard Device

	2）全部 type keywords:
	
	 bios
	 system
	 baseboard
	 chassis
	 processor
	 memory
	 cache
	 connector
	 slot

	3)关键字查看信息：

	[root@ccd-nfs ~]# dmidecode -s --help
	Invalid string keyword: --help
	Valid string keywords are:
	  bios-vendor
	  bios-version
	  bios-release-date
	  system-manufacturer
	  system-product-name
	  system-version
	  system-serial-number
	  system-uuid
	  baseboard-manufacturer
	  baseboard-product-name
	  baseboard-version
	  baseboard-serial-number
	  baseboard-asset-tag
	  chassis-manufacturer
	  chassis-type
	  chassis-version
	  chassis-serial-number
	  chassis-asset-tag
	  processor-family
	  processor-manufacturer
	  processor-version
	  processor-frequency

	[root@ccd-nfs ~]# dmidecode -s  bios-vendor
	HP
	[root@ccd-nfs ~]# dmidecode -s  bios-version
	P70



- 例如：
- dmidecode -t 17 [查看memory信息]
- 使用dmidecode命令时，如果不加任何参数，则打印出所有类型的信息；而加上“-t type_num”或者“-t keywords”可以查看某个类型信息。
- dmidecode
- dmidecode -t 1
- dmidecode -t system

- [root@ccd-nfs ~]# dmidecode -t chassis
- # dmidecode 3.0
- Scanning /dev/mem for entry point.
- SMBIOS 2.8 present.

- Handle 0x0300, DMI type 3, 21 bytes
- Chassis Information
-         Manufacturer: HP
-         Type: Rack Mount Chassis
-         Lock: Not Present
-         Version: Not Specified
-         Serial Number: 6CU44634B5
-         Asset Tag:
-         Boot-up State: Safe
-         Power Supply State: Safe
-         Thermal State: Safe
-         Security Status: Unknown
-         OEM Information: 0x00000000
-         Height: 2 U
-         Number Of Power Cords: 2
-         Contained Elements: 0

## 4. 参考文献

1.[http://blog.csdn.net/xj626852095/article/details/37819337](http://blog.csdn.net/xj626852095/article/details/37819337)

2.[http://www.linuxidc.com/Linux/2015-12/126814.htm](http://www.linuxidc.com/Linux/2015-12/126814.htm)
#libvirt使用小报告#
by luodan danolphoenix@163.com	2014.12
##1.了解libvirt的结构##
libvirt是以一组API的形式存在，用于管理的应用程序。通过一种特定于虚拟机监控程序的机制与每个有效的虚拟机监控程序进行通信，以完成API请求。

在libvirt中，将物理主机称为节点（node），将客户机操作系统称为域（domain），libvirt及其应用程序在宿主操作系统中进行。支持各种虚拟机监控程序，包括Xen，KVM，以及QEMU和用于其他操作系统的许多虚拟产品。

libvirt的在线网站：[libvirt API](http://www.libvirt.org/)

###不使用libvirt和使用libvirt管理虚拟机的方式###

![libvirt所处的位置](http://www.ibm.com/developerworks/cn/linux/l-libvirt/figure1.gif)

###基于驱动程序的libvirt架构###
![libvirt架构](http://www.ibm.com/developerworks/cn/linux/l-libvirt/figure3.gif)

其中libvirtd为可用于远程控制虚拟机所用的特殊守护进程


##2.理解libvirt的domain的XML格式##
XML节点树结构如下

    General metadata
    Operating system booting
        BIOS bootloader
        Host bootloader
        Direct kernel boot
        Container boot 
    SMBIOS System Information
    CPU Allocation
    IOThreads Allocation
    CPU Tuning
    Memory Allocation
    Memory Backing
    Memory Tuning
    NUMA Node Tuning
    Block I/O Tuning
    Resource partitioning
    CPU model and topology
    Events configuration
    Power Management
    Hypervisor features
    Time keeping
    Devices
        Hard drives, floppy disks, CDROMs
        Filesystems
        Device Addresses
        Controllers
        Device leases
        Host device assignment
            USB / PCI / SCSI devices
            Block / character devices 
        Redirected devices
        Smartcard devices
        Network interfaces
            Virtual network
            Bridge to LAN
            Userspace SLIRP stack
            Generic ethernet connection
            Direct attachment to physical interface
            PCI Passthrough
            Multicast tunnel
            TCP tunnel
            Setting the NIC model
            Setting NIC driver-specific options
            Setting network backend-specific options
            Overriding the target element
            Specifying boot order
            Interface ROM BIOS configuration
            Quality of service
            Setting VLAN tag (on supported network types only)
            Modifying virtual link state
            vhost-user interface 
        Input devices
        Hub devices
        Graphical framebuffers
        Video devices
        Consoles, serial, parallel & channel devices
            Guest interface
                Parallel port
                Serial port
                Console
                Channel 
            Host interface
                Domain logfile
                Device logfile
                Virtual console
                Null device
                Pseudo TTY
                Host device proxy
                Named pipe
                TCP client/server
                UDP network console
                UNIX domain socket client/server
                Spice channel
                Nmdm device 
        Sound devices
        Watchdog device
        Memory balloon device
        Random number generator device
        TPM device
        NVRAM device
        panic device
        Shared memory device 




###2.1通用元数据XML格式###

    <domain type='xen' id='3'>
      <name>fv0</name>
      <uuid>4dea22b31d52d8f32516782e98ab3fa0</uuid>
      <title>A short description - title - of the domain</title>
      <description>Some human readable description</description>
      <metadata>
        <app1:foo xmlns:app1="http://app1.org/app1/">..</app1:foo>
        <app2:bar xmlns:app2="http://app1.org/app2/">..</app2:bar>
      </metadata>
   


    <domain>节点

所有虚拟机的根节点称为域（domain），根节点有两个属性：第一个属性是类型（type），用于指定运行域所用的Hypervisor，其值应该为特定的驱动程序，但也可以为“Xen”，“KVM”，“QEMU”,"LXC","KQEMU",第二个属性是id，它是用于标识客户机的唯一的整数标识符，无效的机器没有id值。

    <name>节点   

  为虚拟机的简称，智能由数字，字母组成，并且需要在一台主机中具有唯一性，主要时在存储永久性配置文件时用于生成文件名。

    <uuuid>节点    
为虚拟机全局唯一标识符，其格式需要符合RFC4122的标准。如果在定义或者创建虚拟机时忘记设置uuid值，系统会随机生成一个uuuid。


    <title>节点
可选节点，提供对域的简短描述，该元素的内容中不能包含任何换行字符

    <description>节点
该元素为虚拟机提供一个可读的描述，此数据不会被libvirt使用，可以由用户自定义。

    <metada>节点
用户数据。用户可以在xml中存储自己的数据。应用程序使用该节点存储的XML格式的用户元数据。应用程序在XML节点中使用自定义的命名空间，且此命名空间中只有一个顶层元素（如果应用程序需要具有结构，则它们的命名空间元素需要有子元素)

###2.2操作系统启动数据###
有多种方法启动虚拟机，各有优劣

####2.2.1bios的启动载入方法####
通过BIOS启动的方法可以用于支持全虚拟化的hypervisors。dev属性的值可以是：fd、hd、cdrom、network，在这种情况下，BIOS具有启动优先权（软驱，硬盘，光驱，网络）来决定了到何处去获得一个启动镜像
     
    <os><!--虚拟机启动参数设置-->
        <type>hvm</type><!--hvm表示全虚拟化-->
        <loader readonly='yes' type='rom'>/usr/lib/xen/boot/hvmloader</loader>
        <nvram template='/usr/share/OVMF/OVMF_VARS.fd'>/var/lib/libvirt/nvram/guest_VARS.fd</nvram>
        <boot dev='hd'/><!--启动顺序。dev内容："fd", "hd", "cdrom" or "network"-->
        <boot dev='cdrom'/>
        <bootmenu enable='yes' timeout='3000'/><!--启动时是否显示用户交互界面-->
        <smbios mode='sysinfo'/>
        <bios useserial='yes' rebootTimeout='0'/>
     </os>
    
   <type>节点
包括虚拟机中将要启动的操作系统的类型，huv以为这操作系统是设计用于裸机部署的，所以需要全虚拟化，linux（真是个烂名字啊）意味着该操作系统支持Xen 3 Hypervisor来宾ABI，该节点还有两个可选属性，arch制定CPU的虚拟架构，machine表示机器类型，在[capabilities XML](http://www.libvirt.org/formatcaps.html)中对此两属性有详细叙述。

    <loader>节点
该节点为可选节点，表示用于协助域创建进程的固件对象。由Xen用来全虚拟化域，同样也可以由QEMU/KVM来用于设置QEMU的BIOS文件路径。该节点有两个可选属性：readonly（值为yes 或者 no）,来用于表征镜像是否是可写的或者只读的。type（值为rom或者pflash），用于告诉hypervisor，文件需要被映射到来宾内存何处。例如，如果loader path指向的时UEFI镜像，type就应该值为pflash。
//太长了。。放弃全文翻译官方文档，改为熟悉网上已有的xml文档

  
###2.3 SMBIOS System Information ###
暂时空

###2.4 CPU分配 ###
 vcpu节点的内容是为虚拟机最多分配几个cpu，值处于1~maxcpu之间，可选属性：
  cpuset属性：指定虚拟cpu可以映射到那些物理cpu上，物理cpu用逗号分开，单个数字的标示单个cpu，也可以用range符号标示多个cpu，数字前面的脱字符标示排除这个cpu
  current属性：指定虚拟机最少
  placement属性：指定一个domain的cpu的分配模式，值可以是static、auto。
    <domain>
      ...
      <vcpu placement='static' cpuset="1-4,^3,6" current="1">2</vcpu><!--CPU配置一至四表示物理CPU列表，虚拟CPU和这些物理CPU绑定-->
        <!--Placement：static表示根据后面的定义配置cpu。Auto表示忽略自定义，系统自动配置--> 
      ...
    </domain>

###2.5 IO线程分配###
暂时空

###2.6 CPU调谐###
暂时空

###2.7 内存分配###

     <domain>
      ...
      <memory unit='KiB'>524288</memory>
      <currentMemory unit='KiB'>524288</currentMemory>
      ...
    </domain>

。
    <memory>节点
启动时分配的内存最大值，定义客户端启动时可以分配到的最大内存，内存单位由unit定义，单位可以是：K、KiB、M、MiB、G、GiB、T、TiB。默认是KiB。
    <currentMemory>节点
实际分配的内存，可以小于最大值。如果没有定义，值和memory一致。

###2.8 页定义memory backing###
    <domain>
      ...
      <memoryBacking>
        <hugepages>
          <page size="1" unit="G" nodeset="0-3,5"/>
          <page size="2" unit="M" nodeset="4"/>
        </hugepages>
        <nosharepages/>
        <locked/>
      </memoryBacking>
      ...
    </domain>

可选节点，影响虚拟内存的页置换策略。

###2.9 内存调谐###
有待补充

###2.10 NUMA节点调谐###
有待补充

###2.11 阻塞IO调谐###
有待补充

###2.12 资源划分###
有待补充

###2.13 CPU model and topology###
有待补充

###2.14 Events configuration 事件配置###
    <domain> 
    ...
      <on_poweroff>destroy</on_poweroff>
      <on_reboot>restart</on_reboot>
      <on_crash>restart</on_crash>
      <on_lockfailure>poweroff</on_lockfailure>
      ...
    </domain>
   当一个客户端的OS触发lifecycle时，它将采取新动作覆盖默认操作，具体状态参数如下：

on_poweroff：当客户端请求poweroff时执行特定的动作

on_reboot：当客户端请求reboot时执行特定的动作

on_crash：当客户端崩溃时执行的动作


每种状态下可以允许指定如下四种行为：
destory：domain将会被完全终止，domain的所有资源会被释放

restart：domain会被终止，然后以相同的配置重新启动

preserver：domain会被终止，它的资源会被保留用来分析

rename-restart：domain会被终止，然后以一个新名字被重新启动


QEMU/KVM支持使用destroy和restart动作行为来进行 on_poweroff on_reboot事件处理，on_reboot事件的preserve动作被作为destroy来处理，on_poweroff的rename-restart动作被当作restart事件处理。

###2.15 电源管理###
有待补充

###2.16 Hyperisor 特性###
  ...
  <features>
    <pae/>
    <acpi/>
    <apic/>
    <hap/>
    <privnet/>
    <hyperv>
      <relaxed state='on'/>
      <vapic state='on'/>
      <spinlocks state='on' retries='4096'/>
    </hyperv>
    <kvm>
      <hidden state='on'/>
    </kvm>
    <pvspinlock/>

  </features>
  ...
Hypervisors允许特定的CPU/机器特性打开或关闭，所有的特性都在fearures元素中，在全虚拟化中常用的标记有：
pae：扩展物理地址模式，使32位的客户端支持大于4GB的内存
acpi：用于电源管理


###2.17时间维持###

 ...
  <clock offset='localtime'>
    <timer name='rtc' tickpolicy='catchup' track='guest'>
      <catchup threshold='123' slew='120' limit='10000'/>
    </timer>
    <timer name='pit' tickpolicy='delay'/>
  </clock>
  ...
客户端的时间初始化来自宿主机的时间，大多数操作系统期望硬件时钟保持UTC格式，UTC也是默认格式，然而Windows机器却期望它是'localtime'
clock的offset属性支持四种格式的时间：UTC localtime timezone variable

###2.18设备###

<devices><!--所有的设备都是一个名为devices元素的子设备-->
<emulator>/usr/bin/kvm</emulator><!--emulator元素指定模拟设备二进制文件的全路径--> 
<disk type='block' device='disk'>
<driver name='qemu' cache='none'/>
<source dev='/dev/cciss/c0d0p6'/>
<target dev='vda' bus='virtio'/>
</disk>
<disk type='block' device='cdrom'>
<target dev='hdc' bus='ide'/>
<readonly/>
</disk>
####2.18.1硬盘，光盘和软盘####
所有看起来就像一个disk、floppy、cdrom或者一个 paravirtualized driver的设备都可以通过一个disk元素指定。

disk是一个描述disks的主要容器，
type属性包括:file,block,dir,network。
device属性描述disk如何暴露给客户端OS的，特性包括：floppy、disk、cdrom、lun，默认是disk。

snapshot属性表明在磁盘做快照的时候的默认行为，snapshot=internal 表明在snapshot的时候可以存储改变的数据。snapshot = external表明在snapshot时，将活动的数据分开。snapshot=no表明disk不参加快照，只读磁盘默认是no。

source属性：在disk的type是file时，file属性指定一个合格的全路径文件映像作为客户端的磁盘，在disk的type是block时，dev属性指定一个主机设备的路径作为disk，在disk的type是dir时，dir属性指定一个全路径的目录作为disk，在disk的type是network时，protocol属性指定协议用来访问镜像，镜像的值可以是：nbd，rbd，sheepdog。当protocal的属性值是rbd或者sheepdog时，必须用一个额外的name属性指定使用那个镜像，当type的值是network时，source可以有0个或者多个host字属性指定连接哪些主机。

          target元素：控制总线设备在某个磁盘被选为客户端的OS时，dev属性表明本地磁盘在客户端上的实际名称，因为实际设备的名称指定并不能保证映射到客户端OS上的设备。bus属性指定了哪种类型的磁盘被模拟，值主要有：ide、scsi、virtio、xen、usb、sata。如果省略，总线类型从设备名来推断，例如设备名是sda，则使用scsi类型的总线。tray属性指定可移动磁盘的状态，例如cdrom或者floppy，它的值是open或closed，默认是closed。

          driver允许更进一步的指定hypervisor driver的相关细节。如果hypervisor支持多个后端驱动程序，name属性选择一个主要的后端驱动的名称，可选type参数可以指定一个子类型，例如：xen支持的名称包括tap、tap2、phy、file，qemu只支持qemu名称，但是多类型的包括raw、bochs、qcow2、qed等。cache属性控制cache机制，值可以是：default、none、writethtough、writeback、directsync、unsafe。error_policy属性指定当hypervisor在读写磁盘出现错误时的行为，值可以是：stop、report、ignored、enospace，默认值是report。io属性控制IO策略，qemu客户端支持threads、native。

         readonly元素：指定客户端不能修改设备。当一个disk含有type=cdrom，readonly则是默认值。

         host元素：有两个属性name和port，分别指定了hostname和port。

###2.19文件系统###

来宾可以直接进入的主机目录

###2.20设备地址###

###2.21控制器###

###2.22设备租用契约###

###2.23主机设备分配###

   USB/PCI/SCSI设备
   
   块设备与特征设备
   
   重定向设备
   
   智能卡设备
   




 ###2.24网络接口###

<devices>
    <interface type='direct' trustGuestRxFilters='yes'>
      <source dev='eth0'/>
      <mac address='52:54:00:5d:c7:9e'/>
      <boot order='1'/>
      <rom bar='off'/>
    </interface>
  </devices>
      有好几种网络接口访问客户端:Virtual network、Bridge to LAN、Userspace SLIRP stack、Generic ethernet connection、Direct attachment to physical interface。
      Virtual network：这种推荐配置一般是对使用动态/无线网络环境访问客户端的情况。
      Bridge to LAN：这种推荐配置一般是使用静态有限网络连接客户端的情况。

。。。需要时再继续查找。。。




##3.使用libvirt创建一个虚拟机，设置虚拟机的内存，CPU，磁盘，网络设备，然后用virsh命令停止/开启，休眠/启动虚拟机##

使用QEMU创建一个磁盘，但是不安装操作系统

rodin@rodin-R458:~$ qemu-img create -f qcow firstDisk.img 1G
当前命令在哪个文件夹下执行就创建到哪个文件夹，



我使用的XML配置文件，

    <?xml version="1.0"?>
    <domain type='qemu'>
      <name>DSL-on-QEMU-Rodin</name><!--虚拟机名称-->
      <uuid></uuid>
      <memory>500000</memory><!--最大内存-->
      <currentMemory>500000</currentMemory><!--可用内存-->
      <vcpu>1</vcpu><!--虚拟cpu个数-->
      <os>
        <type arch='i686' machine='pc'>hvm</type>  
        <boot dev='hd'/>
        <boot dev='cdrom'/>
      </os>
      <devices>
        <emulator>/usr/bin/qemu-system-i386</emulator>
        <disk type='file' device='cdrom'>
          <source file ='/home/rodin/Downloads/dsl-4.4.9_gpxz.iso'/>
          <target dev='hdc'/>
          <readonly/>
        </disk> 
        <disk type='file' device='disk'>
          <source file='/home/rodin/firstDisk.img'/>
          <target dev='hda'/>
        </disk>
        <interface type='network'>
          <source network='default'/>
        </interface>
        <graphics type='vnc' port='-1'/> 
      </devices>
    </domain>

  
###libvirtQemuConfig.xml###


使用xml文件作为libvirt的domain配置文件，挂载了这个磁盘firstDisk，和一个虚拟光驱，光驱里面还有个iso镜像文件 

使用virsh工具启动新domain

root@rodin-R458:/home/rodin# virsh create libvirtQemuConfig.xml

Domain DSL-on-QEMU-Rodin created from libvirtQemuConfig.xml

可以使用virsh -list 查看创建状态，

root@rodin-R458:/home/rodin# virsh list
 
Id      Name                 State

----------------------------------
  7 DSL-on-QEMU-Rodin    running

创建成功，domain的id为7.如果要删掉该domain，可以使用 virsh destroy <domain的id号>来进行

使用
virsh create < xml配置文件> 启动新domain

virsh destory < domain的id号> 停止该domain

virsh suspend < domain的id号> 休眠该domain

virsh resume  < domain的id号> 重启恢复该domain


 



 



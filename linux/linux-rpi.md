# RaspberryPi commands

## Cheatsheet
| Description | Command
--------------| ---------
| Login	| User = pi |
| Go to sysadmin/root mode	| sudo su 
| Change password	| passwd  
| Main configuration and update	| raspi-config 
| Get IP adderess	| ifconfig
| Shutdown	| shutdown -h now
| restart	| sudo reboot
| Network restart	| /etc/init.d/networking restart
| Manual/documentation for “tree” package	| man tree 
| Update all packages	| apt-get update
| Upgrade all packages	| apt-get upgrade 
| Scripts / BAT files	| bash go.sh
| Text editor	| nano abc.txt
| Delete file 	| rm abc.txt
| Delete folder and content	| rm -rf folder1/
| Find file by mask     | find . -name "libpi4j.so"
| Linux version	| uname -a
| Services and status	| service --status-all
| Download | wget http://server.lv/filename.zip 


## Update
```
apt-get update
apt-get upgrade   (upgrade all packages)
```

# Adjustments
## Extend RPi image size
```
qemu-img resize <imagename> +2G
fdisk /dev/sda
```
Remember start block, delete partition 2, add partition with start block
```
reboot
resize2fs /dev/sda2
```

## Allow SSH (connect via Putty)
References:
- http://www.maketecheasier.com/static-ip-address-setup-ssh-on-raspberry-pi/
- http://raspberrypi4dummies.wordpress.com/2013/03/17/connect-to-the-raspberry-pi-via-ssh-putty/

```
sudo nano /etc/network/interfaces
    iface eth0 inet static
    address 192.168.1.20
    netmask 255.255.255.0
    gateway 192.168.1.1	

sudo raspi-config 
```
Then enable SSH server in configuration

## Share folder
Refernce: http://www.howtogeek.com/176471/how-to-share-files-between-windows-and-linux/
```
sudo apt-get install samba
apt-get install samba-common-bin
smbpasswd -a pi
sudo nano /etc/samba/smb.conf
```

Add to the end:
```
[share]
path = /home/pi/share
available = yes
valid users = pi
read only = no
browsable = yes
public = yes
writable = yes

[root_share]
path = /home/root/share
available = yes
valid users = root
read only = no
browsable = yes
public = yes
writable = yes
```
Then
```
sudo service samba restart
```

## Increase Current Limiter (~1 Amp)
Reference: http://dumbpcs.blogspot.com/2014/07/usb-power-adapter-cables-whats-best-for.html

```
nano  /boot/config.txt
    max_usb_current=1
    safe_mode_gpio=4
```
When you plug in your external HDD, look at the RED LED of your Pi, if it is flickering, it means that you may NOT have enough voltage/current. The RED LED goes off when the voltage drops below 4.6V (under voltage indicator).

## Power off HDMI to save power
```
/opt/vc/bin/tvservice --off
```



# Development Environment

## Install Wiring Pi
Reference:
- https://projects.drogon.net/raspberry-pi/wiringpi/download-and-install/
- http://wiringpi.com/download-and-install/  

If git not installed (usually it's not installed on Raspberry Pi), then install it:
```
apt-get install git-core
```

Clone repository and build it:
```
git clone git://git.drogon.net/wiringPi
cd wiringPi
git pull origin
./build
gpio readall
```

## Install Maven
Reference: http://maven.apache.org/download.cgi#Installation

## Install C++ for RPi
Reference: http://www.airspayce.com/mikem/bcm2835/
```
wget http://www.airspayce.com/mikem/bcm2835/bcm2835-1.37.tar.gz
tar zxvf bcm2835-1.xx.tar.gz
cd bcm2835-1.xx
./configure
make
sudo make check
sudo make install
```

Sample of C++ class compilation (with library):
```
gcc -o sonar sonar.c -l wiringPi
```

## Install Java
```
apt-get update
apt-get install oracle-java7-jdk
```


## Install P4J
```
curl -s get.pi4j.com | sudo bash
```

## Java Remote Debug
Reference: http://remotevm.abstracthorizon.org/eclipse-tutorial.html
### On Client:
- Add to lib (pi4j+client)
- Add to build path pi4j-core and client
- Create lunch config:
    - Class = org.ah.java.remotevmlauncher.client.LaunchRemote
    - Arguments: 192.168.0.103:8999
    - sandbox.remotedebug.GpioRemote

### On Server:
```
wget http://repository.abstracthorizon.org/maven2/abstracthorizon.snapshot/org/ah/java/remotevmlauncher/remotevmlauncher-agent/1.0-SNAPSHOT/remotevmlauncher-agent-1.0-20140103.103618-5.jar

java -jar remotevmlauncher-agent-1.0-20140103.103618-5.jar -d 1
java -jar remotevmlauncher-agent-1.0-SNAPSHOT.jar -d 3

sudo su
chmod -R ugo+rw .remotevm
chmod -R ugo+rw /opt/pi4j/lib/
```

## Autostart Java Application
Reference: http://stackoverflow.com/questions/11809191/linux-launch-java-program-on-startup-ec2-instance


# Devices
## Magnetometer MPU-6050
Reference: http://blog.bitify.co.uk/2013/11/interfacing-raspberry-pi-and-mpu-6050.html
```
nano /etc/modules
    i2c-bcm2708
    i2c-dev
```

```
nano /etc/modprobe.d/raspi-blacklist.conf
    #blacklist spi-bcm2708
    #blacklist i2c-bcm2708
```

Then
```
apt-get install i2c-tools
```
Sample of debugging:
```
i2cdetect -y 1
i2cget -y 1 0x1e 0x75
```


## Multiple I2C
```
sudo nano /boot/config.txt
    dtparam=i2c_arm=on
    dtparam=spi=on
    dtparam=i2s=on
    dtparam=i2c=on
    dtparam=i2c1=on
    dtparam=i2c0=on
```



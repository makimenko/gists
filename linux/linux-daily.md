# Linux Daily (Frequently Used Commands)

## Basic
| Description | Command |
--------------| ---------
| Go to sysadmin/root mode	| sudo su
| Change password	| passwd
| Services and status	| service --status-all
| Sessions and processes	| top 
| Find processes by name	| top -p \`pgrep "java"\`
| Shutdown	| shutdown -h now
| restart	| shutdown -r now<br>sudo reboot
| Update all packages |	apt-get update
| Upgrade all packages	| apt-get upgrade   
| Linux version	| uname -a

## Network
| Description | Command
--------------| ---------
| Network load	| iftop
| Get IP adderess	| ifconfig
| Network restart	| /etc/init.d/networking restart

## Disk and File IO
| Description | Command
--------------| ---------
| Disk usage	| df
| Text editor	| nano abc.txt
| Delete file |	rm abc.txt
| Delete folder and content	| rm -rf folder1/
| Find file by mask	| find . -name "libpi4j.so"
| Find text in file	 | grep 'ERROR' app.log
	
## Scripts
| Description | Command
--------------| ---------
| Scripts / BAT files | bash go.sh

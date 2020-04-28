# 1. Install the Ubuntu

### 1.1 Install the Ubuntu 16.04 LTS 64BIT (Download URL)
: http://www.ubuntu.com/download/desktop

### 1.2 Set Network IP (recommended)
Super Key >> network connection >> Add >> Ethernet or Wi-Fi >> Naming >> 
IPv4 Settings >> Manual >> Set network below

* Address 192.168.4.xxx
* Netmask 255.255.255.0
* Gateway 192.168.4.2
* DNS servers 133.5.6.1
* Search domains ait.kyushu-u.ac.jp

### 1.3 Setup NTP (Network Time Protocol) Optional

: Clock synchronization is important for ROS. :)

1) Change NTP server
```sh
sudo sed -i 's/#NTP=/NTP=ntp.nict.jp/g' /etc/systemd/timesyncd.conf 
```

2) Check NTP

```sh
systemctl -l status systemd-timesyncd 
```

# 2. Install ROS

### 2.1 Install the ROS Kinetic

wiki page URL for Installation the ROS
: http://wiki.ros.org/kinetic/Installation/Ubuntu

or

```sh
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install ros-kinetic-desktop-full
sudo apt-get install ros-kinetic-rqt-*
sudo rosdep init
rosdep update
source /opt/ros/kinetic/setup.bash
sudo apt-get install python-rosinstall
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
cd ~/catkin_ws/
catkin_make
```

if you want to delete the ROS, run the purge command.

```sh
sudo apt-get purge ros-kinetic-*
```

### 2.2 Set the .bashrc file for ROS

```sh
gedit ~/.bashrc
```

Add script below.

```sh
################## 
## USER SETTING ## 
################## 
alias rm='rm -rf' 
alias eb='gedit ~/.bashrc' 
alias sb='source ~/.bashrc'
alias agi='sudo apt-get install'  
alias m='make -j4 -l4'  
alias gs='git status'  
alias gp='git pull'

# Set ROS Kinetic
source /opt/ros/kinetic/setup.bash 
source ~/catkin_ws/devel/setup.bash 
alias cw='cd ~/catkin_ws' 
alias cs='cd ~/catkin_ws/src' 
alias rt='cd ~/catkin_ws/src/ros_tms' 
alias cm='cd ~/catkin_ws && catkin_make' 

# Set ROS Network 
export ROS_MASTER_URI=http://192.168.4.170:11311 
export ROS_HOSTNAME=YOUR_IP 

###################### 
## USER SETTING END ## 
######################
```

Reload the .bashrc file

```sh
source ~/.bashrc
```

# 3. Pull ROS-TMS pkg using git clone

```sh
sudo apt-get install git
```

## Git Default Setting

```sh
git config --global user.name  YOUR_NAME
git config --global user.email YOUR_EMAIL
git config --global push.default simple
git config --global --add color.ui true
git config --global --list
```

:high_voltage_sign: Don`t forget to create your SSH Keys!

```sh
ssh-keygen -t rsa -C YOUR_EMAIL
cat ~/.ssh/id_rsa.pub
ssh-add ~/.ssh/id_rsa
```

https://github.com/settings/ssh

:high_voltage_sign: Use it if you used private PC.
```sh
cd ~/catkin_ws/src/
git clone git@github.com:irvs/ros_tms.git
```

:high_voltage_sign: Use it if you used public PC.
```sh
cd ~/catkin_ws/src/
git clone https://github.com/irvs/ros_tms.git
```



# 4. Execute the installation script

```sh
cd ~/catkin_ws/src/ros_tms/dependency
```

```sh
./install_dependencies_pkg.sh
```

# 5. Run the catkin_make

```sh
cd ~/catkin_ws/ && catkin_make
```
or

```sh
cm
```

Try catkin_make few times if you encounter some errors.

If you want to compile a specific package, use "--pkg" option.

```sh
cd ~/catkin_ws/
catkin_make --pkg PKGNAME
```


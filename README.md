# Sox The Cat Document
edited using [dillinger](https://dillinger.io/)
## Installation
Burn Rapberry Pi Lite 64-bit to micro sd card using [raspberry pi imager](https://www.raspberrypi.com/software/) and follow instructions

## Configuration And Basic Setting
Using command line to edit config file
```
sudo raspi-congig
```
Select fist tab **System Options** to update wifi setting
Select forth tab **Localisation Options** to update language and timezone
Update remote control options upon personal preference with the **Interface Options**
> Note: we will install `TeamViewer` as our remote control tool, no need to set up vnc here

Using command line to get update
```sh
sudo apt-get update
sudo apt-get upgrade
```

#### Git
Using command line to install git 
```sh
sudo apt-get install git
```
> Note: Lite version raspberry pi os doesn't have built-in `git` command

#### Python3.10
Before you begin, run a quick update to ensure your system is up-to-date to avoid conflicts during the tutorial and good system maintenance.
```sh
sudo su
cd ../../
cd ./etc
```
```sh
sudo apt update && sudo apt upgrade
```
First off, install the essential dependencies required for building Python 3.10 from source as we shall later see.
```sh
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev
```
Download the Gzipped source tarball file using the wget command as shown.
```sh
sudo wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz
```

Now we are ready to install Python 3.10 on Debian 11. First, navigate into the python 3.10 directory.
```sh
tar -xvf Python-3.10.0.tgz
```

Then execute the configure command to confirm if all the dependencies for the installation of Python 3.10 are met.
```sh
sudo ./configure --enable-optimizations
```
To start the build process. Execute the following make command. This takes quite a while, so be patient.
```sh
sudo make -j 2
```
The value  ‘2′ represents the number of CPU cores that can be confirmed using the nproc command. Adjust this value according to the number of CPU cores present on your system.
```sh
nproc
```
Finally, run the command below to install python binaries once the build process is complete:
```sh 
sudo make altinstall
```
Once the installation is complete, verify that python 3.10 is installed in your machine. Run:
```sh
python3.10 --version
```

run command line to update python alternatives
```sh
sudo update-alternatives --install /usr/bin/python python /usr/local/bin/python3.10 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.10 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.9 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 2
```
```sh
python3 --version
python --version
```


> Note: if python version is not 3.1, run command `sudo find / -type f -name python3.10` and copy python3.10 path, then run `alias python=<PATH>` , `alias python3=<PATH>` , `alias pip=<PATH>`, and `pip3=<PATH>`

get python3 pip
```sh
sudo apt-get install python3-pip
sudo python3 -m pip install pip --upgrade pip
```
we may encounter a `lsb_release` issue here, run command line to edit lsb_release file
```sh
sudo nano /usr/bin/lsb_release
```
edit the first line to the following:
```sh
#! /usr/bin/python3.10  
```

> Note: more info please refer to this [link](https://cloudcone.com/docs/article/how-to-install-python-3-10-on-debian-11/)


install `venv`
```sh
sudo apt-get install python3-venv
```

#### Numpy1.22.4
install Numpy1.21.6 in the venv using the command line
```sh
pip install numpy==1.22.4
```

## GUI 
#### XFCE4
Turn on your Pi and log in. We will install Xorg. To do this type in:
```sh
sudo apt-get install xserver-xorg
```
If you only install xserver-xorg, you will not have the ability to launch the Xorg Display Server from the command line. This would be a problem if you are not planning on installing a login manager. Otherwise, you do not need to do this. If this is the case, you may want to also install xinit by typing in:
```sh
sudo apt-get install xinit
```

When you install XFCE, some essentials such as settings and file manager are included. By default, XFCE uses XFCE4 Terminal. This is optional to install, however if you choose not to install it, then XTerm will be the default Terminal app.. To install XFCE with XFCE4 Terminal, type in:
```sh
sudo apt-get install xfce4 xfce4-terminal
```
 To clean up leftover packages, type in:
```sh
sudo apt-get clean
```
> Note: DON'T USE `startx` YET!!! UNLESS YOU WANT TO SET UP YOUR OWN REMOTE CONTROL.
>> More info about alternative GUI please refer this [link](https://forums.raspberrypi.com/viewtopic.php?t=133691)

## Apps
highly recommand using these apps to control and edit files on Pi
#### TeamViewer
I used TV to remote control my Pi, easy setup and doesn't require a monitor
A .deb file is a package file designed for the Debian systems package management system. The .deb file is an archive containing all the files that we need for TeamViewer.
```sh
wget https://download.teamviewer.com/download/linux/teamviewer-host_armhf.deb
```
To install the TeamViewer deb package, we will be making use of the `dpkg utility` which is the base of the Debian package management software.
```sh
sudo dpkg -i teamviewer-host_armhf.deb
```
clean up the mess with the following command
```sh
sudo apt --fix-broken install
```
Connect to Teamviewer with account and password, so that every time Raspberry Pi boit up, it shows in the TeamViewer account's online device list, then could be eaisly connect
```sh
sudo teamviewer passwd <PASSWORD>
sudo teamviewer setup
```
> Note: `<PASSWORD>` must start with number or letter, and must include special character.

use `raspi-config` to set auto login to GUI
```sh
raspi-config
```
choose `System Options` and select `Boot / Auto Login`, then select the last option
reboot, and we will be direct to the desktop environment
```sh
sudo reboot
```
After the reboot, we can use TeamViewer to remote control.
> Note: Some device needs some customization to the `config.txt` file from the Raspberry Pi Boot loader to achive proper headless remote control performance. More info please refer to this [link](https://forums.raspberrypi.com/viewtopic.php?t=323294)

#### Pi-Apps
I use Pi-Apps to install other apps we need for this project
```sh
git clone https://github.com/botspot/pi-apps
~/pi-apps/install
```
#### Visual studio code
use pi-apps to instal vs-code on Raspberry-Pi

#### Chromium
use pi-apps to instal Chromium on Raspberry-Pi

## Voice Assistance
Install mycroft as the voice assistance with Coqui TTS YourTTS model

##### MyCroft
You can find the mycroft-core repo from [here](https://github.com/MycroftAI/mycroft-core)
```sh
mkdir VA
cd ~/VA
python3 -m venv .
source ./bin/activate
git clone https://github.com/MycroftAI/mycroft-core.git
cd mycroft-core
bash dev_setup.sh
deactivate
```
##### Coqui
installing Coqui TTS server in the main directory
```sh 
cd ./../
mkdir Coqui
cd Coqui
python3 -m venv .
source ./bin/activate
```
now we enter the `venv`, we need to set uo the TTS server here
```sh
pip install pip --upgrade
pip install tts --upgrade
```
>  Note: Only do this if you want customized voice
>- Find the `server.py` file under TTS directory by running this line 
> ```sh 
> sudo find ./ -type f -name server.py 
> ```
>- Edit using command
>```
>sudo nano <PATH>/server.py
>```
>- find `synthesize.tts(...)`, add `language_name = "en",` inside the parante parantesis

> Note: if encounter _lzma module not found, run command `sudo apt-get install liblzma-dev` and `pip install backports.lzma`, then enter `sudo nano /usr/local/lib/python3.10/lzma.py`, change line 27 and 28 to ""

initiate the server by running the folowing line
```sh
tts-server --model_name tts_models/multilingual/multi-dataset/your_tts --use_cuda=false 
```



# MyUbuntu for neuroimaging analysis


myubuntu is a docker container based on Ubuntu 18.04LTS. Several common neuroimaging softwares are installed and ready to use.

It also contains R and Python2/3 for basic computationas. And they can be called by other neuroimaging analysis tools (e.g. 3dMVM requires R for ANOVA analysis).

## When do you need this container:
 
* You work with Windows.
  
* You have trouble with installation/configuration of specific neuroimaging softwares.
  
* You work with high performance computer(HPC) which does not allow you to install your libraries freely.
  
* You want a clean/stable/reproducible computing environment.
  

## *Full list of main software and version:*

* Freesurfer:      7.2.0
* ANTs:         2.4.0 SHA:04a018d
* AFNI:         AFNI_22.2.02 'Marcus * Aurelius'
* MRtrix3:      3.0.3
* FSL:          6.0.2
* Python:       2.7.17/3.6.9
* R:            3.6.3
* MATLAB Runtime: 2014b(8.4)
  
## *Installation*

There are two ways to install this container on your personal machine or high performance computer(HPC)

### A. Build from Dockerfile

To build the container from Dockerfile, you should have docker engine installed on your machine.

    $ git clone  https://github.com/MengxingLiu/myubuntu.git
    $ cd myubuntu/
    $ docker build -t myubuntu:0.1 .
This usually takes 2-3 hours, as building to from dockerfile basically equals compiling the softwares from fresh. 

**Please be alert**, if you choose to build from Dockerfile, some dependences might be installed as the lastest version by your installation time (e.g. some libraries). But the neuroimaging tools will be in the exact same version as shown above.

### B. Pull from docker hub

If you choose to pull the container from docker hub instead of building from Dockerfile, you can have either **docker** or **singularity** installed on your machine (as some HPCs do not allow docker usage, singularity is a good alternative).

Pulling with **Docker**:

    $ docker pull lmengxing/myubuntu:0.1 

Pulling with **Singularity**:

    $ singularity build myubuntu_0.1.sif docker://lmengxing/myubuntu:0.1


## *Usage*

The idea of this container is to provide an neuroimaging enviroment that allows users to run data analysis with their own data and pipeline interactively.

Simplest example:

Running with **Docker**:

    $ docker run -it lmengxing/myubuntu:0.1 

or 

    $ sudo docker run -it lmengxing/myubuntu:0.1  # depends on your user permission

Running with **Singularity**:

    $ singularity shell myubuntu_0.1.sif 


Formal usage example:

Usually you would like to mount your data on the container, in order to access and process your data after you enter the docker container environment. In MyUbuntu, there is a directory "work" under the home directory /root, where you can mount your data on.

Running with **Docker**:

    $  docker run -it -v /home/username/project:/root/work lmengxing/myubuntu:0.1

Runing with **Singularity**:

    $ singularity shell --bind /home/username/project:/root/work myubuntu_0.1.sif

With this command, after you enter the docker container, you will find your data under /root/work. You can also try to mount multiple directories on your host to multiple destinations in the docker container, even the directories do not exisit in the docker container before mounting.


## *Using GUI with myubuntu*

Linux:

simple way is to forward x11 to your local machine so that the container can render to the correct display by reading and writing through the X11 unix socket.

First adjust the permission X server host with 

    $ xhost +local:root # this adds the container username to your x server access list

This is not safe as someone could display something on your screen (although not likely).

After adding access, you can run myubuntu with a few more parameters:

    sudo docker run --rm -ti \
        --user=0 \
        -v /tmp/.X11-unix:/tmp/.X11-unix \
        -e DISPLAY=${DISPLAY} \
        lmengxing/myubuntu:0.1 /bin/bash

Once you finish your work, and if you are concerned about the X server host security, you can remove "root" from your access list by:

    xhost -local:root

MacOs:

    Coming...

Windows:

    Use VsXsrv. This method is not stable, fsleyes in my computer will report an error (added and tested by @Ernest861)

1. Install VcXsrv Windows X Server (path a or b is same)，

    a. download exe and install（[https://sourceforge.net/projects/vcxsrv/]);
    b. **OR** use Chocolatery to install ([https://chocolatey.org/install]).

```PowerShell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
choco install vcxsrv
```

2. Set Xlauch
    2.1 run Xlaunch from the start menu;
    2.2 Display settings: Multiple windows and Display number -1;
    2.3 Client startup: Start no client;
    2.4 Extra settings: √Clipboard-Primary Selection, Native opngl, disable access control;
    2.5 if Firewall alarm just allows.

3. Open Docker
    3.1 get local host ip; 3.2 run docker; 3.3 export the freesurfer license;3.4 set DISPLAY value;3.5 open freesurfer.

```PowerShell
ipconfig
docker run -it -v YOURWORKPATH:/root/work lmengxing/myubuntu:0.1
```
```Bash
export FS_LICENSE='/root/work/license.txt'
export DISPLAY='YOURIP:0.0'
freeview
```
This method is not stable in my win10 and seems to be related to the amd graph set (libGL error: No matching fbConfigs or visuals found
libGL error: failed to load driver: swrast).

## *Contributing*

Thank you for your interest in contributing to *MyUbuntu*! If you would like to have specific tools added in this container, feel free to open an issue. Or better, you can fork this repository and add the tools you like and do a pull request. 

Enjoy.





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

## *Installation*

There are two ways to install this container on your personal machine or high performance computer(HPC)

### A. Build from Dockerfile

To build the container from Dockerfile, you should have docker engine installed on your machine.

    $ git clone  https://github.com/MengxingLiu/myubuntu.git
    $ cd myubuntu/
    $ docker build -t myubuntu:1.0 .
This usually takes 2-3 hours, as building to from dockerfile basically equals compiling the softwares from fresh. 

**Please be alert**, if you choose to build from Dockerfile, some dependences might be installed as the lastes version by your installation time (e.g. some libraries). But the neuroimaging tools will be in the exact same version as shown above.

### B. Pull from docker hub

If you choose to pull the container from docker hub instead of building from Dockerfile, you can have either **docker** or **singularity** installed on your machine (as some HPCs do not allow docker usage, singularity is a good alternative).

Pulling with **Docker**:

    $ docker pull lmengxing/myubuntu:0.1 

Pulling with **Singularity**:

    $ singularity build myubuntu_0.1.sif docker://lmengxing/myubuntu:0.1


## *Usage*

The idea of this container is to provide an neuroimaging enviroment that allows users to run data analysis interactively with their own data and pipeline interactively.

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

With this command, after you enter the docker container, you will find your data under /root/work. You can also try to mount multiple directory on your host to multiple destinations in the docker container, even the directories don't exisit in the docker container before mounting.

## *Contributing*

Thank you for your interest in contributing to *MyUbuntu*! If you would like to have specific tools added in this container, feel free to open an issue. Or better, you can fork this repository and add the tools you like and do a pull request. 

Enjoy.





# Tutorial for the newcomers to ImagingLab@ZJU - 001

## 为什么是Linux？

**Linux** 能够满足我们实验室 **多人** **同时** 使用服务器的需求。更多的信息可以参见 [9 reasons Linux is a popular choice for servers](https://www.logicmonitor.com/blog/9-reasons-linux-is-a-popular-choice-for-servers#:~:text=Linux%20has%20been%20a%20popular,quickly%20be%20identified%20and%20patched.)。

## Linux 系统操作和基础命令教程

* 首先推荐各位新同学先在自己电脑上配置一个Linux系统熟悉环境，可以使用双系统或虚拟机，但我们更推荐使用 [Docker](https://docs.docker.com/desktop/) 在Windows上搭建Linux环境，具体操作参见 [中文教程](https://blog.csdn.net/qq_45491068/article/details/112840902)。

* 构建好Linux系统（在你自己的电脑中）后，下面1-5步是超级账户建立新用户的操作（服务器端这些操作会由管理员完成）。

**Please PAY ATTENTION that the `${}` represents an variable, and it shouldn't be typed in your command.**

1. Log in server with the ROOT user
```shell
ssh zj_servers2@10.11.64.218 # zj_servers2 是超级账户名，10.11.64.218 是服务器的IP地址
```

2. Enter the passwd of sudoer `(ask the manager for the passwd of the root)`
```shell
passwd:xxxxxxxxxx
```

3. Add a new user, such as `chensq`
```shell
sudo useradd ${username} # such as `sudo useradd chensq`

sudo passwd ${username}  # such as `sudo passwd chensq`
```

4. Set the default shell for the user
```shell
# 声明登陆后的shell
sudo usermod -s /bin/bash ${username} # such as `sudo usermod -s /bin/bash chensq` 
```

5. Set your default folder. PLEASE put it on HDD.
```shell
cd ${DIR of HDD} # such as `cd /raid` 进入某一个文件夹路径

sudo mkdir ${username} # such as `sudo mkdir chensq` 建立你自己的文件夹

#声明你这个用户每一次登录以后初始化的文件夹
sudo usermod -d ${DIR of HDD}/${username} ${username} # such as `sudo usermod -d /raid/chensq chensq` 

# 声明这个文件夹的所有权归于于你这个用户，使用这个命令的时候必须小心，不能输错！往年有很多同学都会搞错这个命令，将所有人的文件都归属到自己账户下！
sudo chown -R ${username} ${DIR of HDD}/${username} # such as `sudo chown -R chensq /raid/chensq` 
```

* 下面的操作就是在你的个人账户中建立可运行的计算环境（Python，CUDA...）

6. Set your System Environment, copy .bashrc into your folder

bashrc需要仔细介绍，具体内容：[什么是.bashrc，有什么用?](https://blog.csdn.net/Heyyellman/article/details/111565781)
```shell
# log in as your own user
su ${username} # such as `su chensq`

# copy the bashrc to your folder
cp /home/zj_servers2/.bashrc ~/ # such as `cp /home/zj_servers2/.bashrc ~/`

# activate the system parameters in .bashrc
source .bashrc

# confirm the environment is activate (including cuda, cudnn, and so on)
nvcc -V
```

7. Set up your Python Environment
```shell
# copy the anaconda into your folder
cp /raid/readme/Anaconda3-2023.03-1-Linux-x86_64.sh ~/

# install the anaconda into your environment
bash Anaconda3-2023.03-1-Linux-x86_64.sh

# check if the path of anaconda is in your bashrc
vim .bashrc

# if not add the line below in the bottom
export PATH=${Anaconda}/bin:$PATH # such as `export PATH=/raid/chensq/anaconda3/bin:$PATH`

# reactivate the environment
source .bashrc

# confirm the python and conda
which python
conda list
```

8. Set the Mirror of your anaconda and pip
```shell
# Mirror of anaconda, please check the detail of this link
https://mirror.tuna.tsinghua.edu.cn/help/anaconda/

# Mirror of pip, please check the detail of this link
https://mirror.tuna.tsinghua.edu.cn/help/pypi/
```

9. Set up the virtual environment of Python
```shell
# create a virtual environment with the env_name and the specific python version
conda create -n ${env_name} python=${__version__} # such as `conda create -n pytorch python=3.9.6`

# download the package of python
pip install numpy

# ATTENTION: for pytorch, tensorflow, and JAX... We recommend you to first download these frameworks.
# Because other packages, such as numpy..., have the dependence on these frameworks. SO CHECK IT OUT FIRST! 
```

0. Other tool for you to use
```shell
# tmux (a tool to multiple your terminal, Use tmux, turn off your PC!) 
[tmux tutorial](see http://louiszhai.github.io/2017/09/30/tmux/)

# htop (check the cpu and the pid of processes)
htop

# watch
watch -n ${refresh_time} nvidia-smi # such as watch -n 1 nvidia-smi (means every 1s print the GPU-info)

# check the storage of the disk
df -h

# check the data I/O of the disk
iostat -x ${refresh_time} # such as iostat -x 1 (means every 1s print the data I/O)

# this doc is write in markdown
[markdown tutorial](see https://github.com/guodongxiaren/README)
```

Create by Chen Shiqi at 2023.06.28
Edit by Chen Shiqi at 2023.09.18
# 配置Ubuntu计算环境流程

## 1.安装Windows+Ubuntu双系统

①  [UltralSO软件](https://cn.ultraiso.net/xiazai.html) + 大于8G的U盘 + [Ubuntu官网]( https://ubuntu.com/download/alternative-downloads )下载的系统 + [刻录](https://jingyan.baidu.com/article/5225f26b0bb45fe6fa0908bc.html) -> 启动U盘

② [双系统教程](https://www.jianshu.com/p/38e6be8efecf)  

*选择在安装过程中更新使用的是Ubuntu官方源，下载较慢，建议安装完成后使用国内源更新*

*注意安装完成重启时要进入界面再拔除U盘*

## 2.配置国内数据源以加快后续工作

### apt-get接入国内源

1. Ubuntu 的软件源配置文件是 /etc/apt/sources.list，首先备份：

   ```shell
   sudo cp /etc/apt/sources.list  /etc/apt/sources.list.bak
   ```

2. 编辑sources.list文件添加内容

   ```shell
   sudo gedit /etc/apt/sources.list 
   ```

3. [按版本选择清华镜像]( https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/ ) 复制内容到上述文件

4. ```shell
	sudo apt-get update
	```

5. 更新所有软件包：

   ```shell
   sudo apt-get upgrade
   ```

### Anaconda3接入国内源

#### 1.安装Anaconda3

[清华下载地址]( https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/ )  -> 根据系统版本选择 **.sh** 安装包

终端中切换到下载目录

```shell
bash 文件名
```

根据提示进行安装即可

#### 2.修改环境写入权限

Anaconda3报权限错误：

```shell
sudo chown -R 你的用户名 anaconda3
```

~~#### 3. conda加入国内源(清华)~~现因中止授权而关闭服务
已经安装了国内源的，可以使用以下指令恢复默认：
```shell
conda config --remove-key channels
```

### pip接入国内源

安装pip

 ```shell sudo apt install python-pip 
 sudo apt install python-pip 
 ```

首先在当前用户目录下建立文件夹.pip，然后在文件夹中创建pip.conf文件，再将源地址加进去即可。

```shell
mkdir ~/.pip
gedit ~/.pip/pip.conf
```

然后将下面这两行复制进去就好了

```
[global]
index-url = https://mirrors.aliyun.com/pypi/simple
```

```shell
  # 国内其他pip源
  清华：https://pypi.tuna.tsinghua.edu.cn/simple
  中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
  华中理工大学：http://pypi.hustunique.com/
  山东理工大学：http://pypi.sdutlinux.org/
  豆瓣：http://pypi.douban.com/simple/
```

 ## 3.安装CUDA/Cudnn
### 1. 安装Nvidia驱动

软件和更新->附加驱动，自动搜索显卡驱动，选择其中一个驱动版本；点击应用更改等待安装完成，重启。

### 2. 安装CUDA

[CUDA全部安装包官方下载地址(先安装gcc)](https://developer.nvidia.com/cuda-toolkit-archive)

安装CUDA需要注意与Cudnn版本的匹配：

![CUDA_Download](https://github.com/HUALIMUGU/Ubuntu-Settings/blob/master/images/CUDA_Download.png)

参照下图进行类似的操作

![Cudnn_Download](https://github.com/HUALIMUGU/Ubuntu-Settings/blob/master/images/Cudnn_Download.png)

1. 安装时不选中驱动安装

2. 配置CUDA环境变量

   ```shell
   sudo gedit ~/.bashrc
   ```

   在.bashrc末尾添加以下两行环境变量

   ```shell
   export PATH=$PATH:$/usr/local/cuda-10.2/bin  #根据CUDA版本更换路径
   export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}  #根据CUDA版本更换路径
   ```

   执行下面的指令使配置生效

   ```shell
   source ~/.bashrc
   ```

3. 重启

4. CUDA测试

   ```shell
   cd /usr/local/cuda/samples/1_Utilities/deviceQuery 
   sudo make
   ./deviceQuery
   ```

   安装Cudnn

### 安装Cudnn
[Cudnn全部安装包官方下载地址](https://developer.nvidia.com/rdp/cudnn-download)需要提前自行注册

下载下图的三个文件并按照次序安装安装指令：```dpkg -i 文件名```

![Cudnn](https://github.com/HUALIMUGU/Ubuntu-Settings/blob/master/images/Cudnn.png)

测试安装是否成功
```shell
dpkg -l | grep cudnn
```
出现li开头的3条数据即是安装完成





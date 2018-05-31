---
title: my blog
date: 2018-05-31 10:48:43
tags: this is my first blog


---


### Onlyoffice部署到181服务器操作流程以及汉化流程

参照网址：https://blog.csdn.net/hotqin888/article/details/79338286

进入192.168.16.181服务器:

cd /home 切换到181home

新建onlyoffice目录： sudo mkdir onlyoffice

切换到onlyoffice目录下： cd onlyoffice

新建 logs data lib db 文件夹 ：sudo mkdir logs data lib db

 执行sudo vi run.sh:

编辑内容：

docker run -d -p 8687:80 --name onlyoffice \

-v /home/onlyoffice/logs:/var/log/onlyoffice  \

-v /home/onlyoffice/data:/var/www/onlyoffice/Data  \

-v /home/onlyoffice/lib:/var/lib/onlyoffice \

-v /home/onlyoffice/db:/var/lib/postgresql  onlyoffice/documentserver

(注意行与行之间不能有空格) 然后运行命令：sh run.sh 脚本生成docker项目。

界面汉化：

进入docker当中的onlyoffice项目；docker exec -it onlyoffice sh;

Cd 到 /usr/share/fonts   ls   truetype  X11

执行命令：rm -R dir X11 删除//删除文件夹X11

进入truetype目录 ；执行命令：rm -R dir *.*

                              rm -R dir *;

目录下只剩下custom

执行一下命令，拷贝下载好的字体压缩包到truetype目录下：

wget http://192.168.16.118:5000/winfont.tar.gz  //压缩包存放在本地启动的一个服务当中

然后解压字体压缩包：执行命令：tar -xzvf winfont.tar.gz

移动字体到truetype目录下： mv /usr/share/fonts/truetype/winfont/* .

删除压缩包和winfont目录：

 rm -rf winfont  ； rm winfont.tar.gz（还要手动删除arial.ttf字体，这个字体乱码）

然后退出exit

接着继续进入 docker exec -it onlyoffice /bin/bash

依次执行：

sudo mkfontscale

sudo mkfontdir

sudo fc-cache -fv

然后退出exit

执行：docker exec onlyoffice /usr/bin/documentserver-generate-allfonts.sh

最后重启docker ：docker start onlyoffice （清除浏览器缓存 设置-高级-清除浏览器缓存）

最后将html配置改成：

          "editorConfig": {

                    "url": "http://192.168.16.118:5000",

                    "lang": "zh-CN",

                }

完成

 

 

 

 

 

 

 

 

 



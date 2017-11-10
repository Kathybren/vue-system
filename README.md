## 环境搭建
1.安装Node.js,从[node.js官网](https://nodejs.org/zh-cn/)根据自己的需求对应下载并安装，很简单过程不赘述，安装完成之后，打开命令行工具(win+r，然后输入cmd)，输入 node -v，如下图，如果出现相应的版本号，则说明安装成功。
  
  ![](https://raw.githubusercontent.com/Kathybren/img/master/images/3868852-e27ffe7726909c64.png)
  
说明一下官网下载安装node.js后，就已经自带npm（包管理工具）了，另需要注意的是node版本要高于4.0，npm的版本最好是3.x.x以上，以免对后续产生影响。
  
  ![](https://raw.githubusercontent.com/Kathybren/img/master/images/3868852-e27ffe7726909c64.png)

2.安装webpack，打开命令行工具输入：npm install webpack -g，安装完成之后输入 webpack -v，如下图，如果出现相应的版本号，则说明安装成功。
说明一下，由于很多都是国外的，如果网速比较慢可以安装淘宝镜像（不懂可自行百度）
  
  ![](https://raw.githubusercontent.com/Kathybren/img/master/images/3868852-78ae4207e9848e99.png)

3.安装vue-cli脚手架构建工具，打开命令行工具输入：npm install vue-cli -g，安装完成之后输入 vue -V（注意这里是大写的“V”），如下图，如果出现相应的版本号，则说明安装成功。
  
  ![](https://raw.githubusercontent.com/Kathybren/img/master/images/3868852-6efbfe25b7a6f757.png)
  
## 准备工作完成后就可以开始vue-cli项目
在硬盘上找一个文件夹放工程用的。这里有两种方式指定到相关目录：①cd 目录路径 ②如果以安装git的，在相关目录右键选择Git Bash Here
1.vue init webpack vue-system 通过webpack初始化一个项目，根据提示做出相应的选择，个人建议安装vue-router、ESLint(代码规范，建议养成习惯)
2.
```
cd vue-system
npm install
npm run dev
```
  
  ![](https://raw.githubusercontent.com/Kathybren/img/master/images/QQ%E6%88%AA%E5%9B%BE20171110155148.png)

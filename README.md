## 简介
### 基于Vue+Element实现真实后台管理系统
本教程只是vue全家桶简单的一部分，主要为了在实际开发中快速上手，同时能够快速掌握Vue构建工具。

由于本人水平有限，如果有不足或者错误的地方，希望能提出意见和建议。

这是鄙人第一个教程，所有您的鼓励对我尤为重要，所有不要吝惜你的star。
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

3.恭喜你已经成功一大步
## 配置脚手架

1.打开.eslintrc.js文件，在rules下配置
```
'eol-last': 0, // 不检测文件末尾有空行
'space-before-function-paren': 0
```
2.如果需要使用scss可
```
npm install node-sass --save-dev
npm install sass-loader --save-dev
```
3.引入Element ui
```
npm i element-ui -S
```

配置element-ui。

在src下的main.js引入element-ui

```src/main.js```

```
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

Vue.use(ElementUI)
```
4.重置样式
在src新建common文件夹，放一些公共样式配置等
```
cd common
mkdir css
touch ./css/reset.css
```
在reset.css里面写重置样式
然后在main.js引入
```
src/main.js
```
```
import './common/css/reset.css'
```
先配置这么多，后面面需要什么再根据自己的需求就行配置
## 从登陆页开始
### 创建登陆页面

```
cd src
mkdir pages
touch ./pages/Login.vue
```

```src/pages/Login.vue```

```
<template>
    <div>
        Login
    </div>
</template>
```

注意template下只能有一个根
  
  2.配置路由

进入src下的App.vue，删除脚手架自带的组件，剩余如下

```
<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>
```

然后进入router下的index.js删除多余项剩下

```src/router/index.js```
```
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export default new Router({
  routes: [
  ]
})
```
ok一切准备就绪

引入Login组件，然后配置路由

```
import Login from '../pages/Login'

routes: [
    {
      path: '/',
      redirect: '/login'
    },
    {
      path: '/login',
      name: 'Login',
      component: Login
    }
  ]
```

然后 ```npm run dev```你会发现浏览器出现Login

填充登陆页面

template部分
```
<div class="login">
  <div class="login-wrap">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="demo-ruleForm">
      <el-form-item prop="username">
          <el-input v-model="ruleForm.username" placeholder="username"></el-input>
      </el-form-item>
      <el-form-item prop="password">
          <el-input type="password" placeholder="password" v-model="ruleForm.password" @keyup.enter.native="submitForm('ruleForm')"></el-input>
      </el-form-item>
      <div class="login-btn">
          <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
      </div>
      <p style="font-size:12px;line-height:30px;color:#999;">Tips : 用户名和密码随便填。</p>
    </el-form>
  </div>
</div>
```
script部分
```
<script>
export default {
  data() {
    return {
      ruleForm: {
        username: '',
        password: ''
      },
      rules: {
        username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
        password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
      }
    }
  },
  methods: {
    submitForm(formName) {
      const self = this
      self.$refs[formName].validate(valid => {
        if (valid) {
          localStorage.setItem('username', self.ruleForm.username)
          self.$router.push('/home')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>
```
样式部分
```
<style lang="scss" scoped>
	.login{
		position: absolute;
		width:100%;
		height:100%;
		display: flex;
		justify-content: center;
		align-items: center;
		.login-wrap{
			width:300px;
		  height:160px;
			padding:40px;
			border-radius: 5px;
			background: #fff;
			.login-btn{
        text-align: center;
    }
    .login-btn button{
        width:100%;
        height:36px;
    	}
    }
  }
</style>
```
这时浏览器会出现

![](https://raw.githubusercontent.com/Kathybren/img/master/images/QQ%E6%88%AA%E5%9B%BE20171113131331.png)

### 开始内容部分
首先内容区会分成3个部分头部、侧边栏和显示内容区域
在src下的components建立Header、Home、Slider组件
```
touch ./src/components/Header.vue
touch ./src/components/Home.vue
touch ./src/components/Slider.vue
```
编写Header组件
```src/components/Header.vue```

template部分
```
<template>
   <div class="header">
    <div class="logo">后台管理系统V1.0</div>
    <div class="user-info">
      <el-dropdown trigger="click" @command="handleCommand">
        <span class="el-dropdown-link">
          <span style="margin-left:8px;margin-right:10px">{{role}} {{name}}</span>
          <i class="el-icon-caret-bottom"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item command="personInfo">个人信息</el-dropdown-item>
          <el-dropdown-item command="loginout">退出</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>
```
script部分
```
<script>
export default {
  data() {
    return {
      role: '管理员',
      name: localStorage.getItem('username')
    }
  },
  methods: {
    handleCommand(command) {
      if (command === 'loginout') {
        localStorage.removeItem('username')
        this.$router.push('/login')
      }
    }
  }
}
</script>
```
样式部分
```
<style lang="scss" scoped>
.header {
  position: relative;
  box-sizing: border-box;
  width: 100%;
  height: 70px;
  font-size: 22px;
  line-height: 70px;
  color: #fff;
  background: #2E363F;
  .logo {
    float: left;
    width: 250px;
    text-align: center;
    font-size: 16px;
  }
  .user-info {
    float: right;
    padding-right: 50px;
    font-size: 16px;
    span {
      color: #fff;
    }
  }
}
</style>
```
2.编写Slider组件
```src/components/Slider.vue```

template部分
```
<template>
  <div class="slider">
    <el-col :span="12">
      <el-menu
        default-active="2"
        class="el-menu-vertical-demo"
        @open="handleOpen"
        @close="handleClose"
        background-color="#545c64"
        text-color="#fff"
        active-text-color="#ffd04b"
        unique-opened router
        >
        <template v-for="item in items" >  
        <el-menu-item :index="item.index" :key="item.index" v-if="!item.subs">
          <i class="el-icon-menu"></i>
          <span slot="title">{{ item.title }}</span>
        </el-menu-item>
        <el-submenu v-else :index="item.index" :title="item.title" :key="item.index">
          <template slot="title">
            <i class="el-icon-location"></i>
            <span>{{ item.title }}</span>
          </template>
            <el-menu-item class="seam"v-for="(subItem,i) in item.subs" :key="i" :index="subItem.index">{{ subItem.title }}</el-menu-item>
        </el-submenu>
        </template> 
      </el-menu>
    </el-col>
  </div>
</template>
```
script部分
```
<script>
  export default {
    data() {
      return {
        role: '',
        titles: [],
        items: [{
          icon: 'el-icon-date',
          index: '/home',
          title: '简介'
        },
        {
          icon: 'el-icon-date',
          index: 'axios',
          title: 'Axios'
        },
        {
          icon: 'el-icon-date',
          index: '2',
          title: '基础',
          subs: [
            {
              icon: 'el-icon-date',
              index: 'base',
              title: '基础设置'
            }
          ]
        }]
      }
    },
    methods: {
      handleOpen(key, keyPath) {
        console.log(key, keyPath)
      },
      handleClose(key, keyPath) {
        console.log(key, keyPath)
      }
    }
  }
</script>
```
说明一下items是想要展示的选项

样式部分
```
<style lang="scss">
.slider {
  display: block;
  position: absolute;
  width: 200px;
  left: 0;
  top: 70px;
  bottom: 0;
  background: #2E363F;
  .el-col-12{
    width: 100%;
  }
}
</style>
```
3.编写Home组件
```src/components/Home.vue```

template部分
```
<template>
  <div class="home">
    <v-head></v-head>
    <v-sidebar></v-sidebar>
    <div class="content">
      <transition name="move" mode="out-in">
        <router-view></router-view>
      </transition>
    </div>
  </div>
</template>
```
script部分
```
<script>
import vHead from './Header.vue'
import vSidebar from './Slider.vue'
export default {
  components: {
    vHead, vSidebar
  }
}
</script>
```
样式部分
```
<style lang="scss" scoped>
.home {
  font-size: 13px;
  .content {
    background: none repeat scroll 0 0 #f2f2f2;
    position: absolute;
    left: 200px;
    right: 0;
    top: 70px;
    bottom: 0;
    width: auto;
    box-sizing: border-box;
    overflow-y: scroll;
  }
}
</style>
```
配置路由
```
import Home from '../components/Home'
{
  path: '/home',
  name: 'Home',
  component: Home
}
```
重启服务然后登陆

你会发现

![](https://raw.githubusercontent.com/Kathybren/img/master/images/QQ%E6%88%AA%E5%9B%BE20171113162555.png)

### 新建各个选项页面
```src/pages```
```
mkdir Base
mkdir Introduction
mkdir Axios
touch ./pages/Base/Base.vue
touch ./pages/Introduction/Introduction.vue
touch ./pages/Axios/Axios.vue
```
配置路由
```src/router/index.js```
```
import Introduction from '../pages/Introduction/Introduction'
import Base from '../pages/Base/Base'
import Axios from '../pages/Axios/Axios'
```
```
 routes: [
     {
      path: '/',
      redirect: '/login'
    },
    {
      path: '/login',
      name: 'Login',
      component: Login
    },
    {
      path: '/home',
      name: 'Home',
      component: Home,
      children: [
        {
          path: '/',
          name: 'Introduction',
          component: Introduction
        },
        {
          path: '/base',
          name: 'Base',
          component: Base
        },
        {
          path: '/axios',
          name: 'Axios',
          component: Axios
        }
      ]
    }
  ]
```
  配置完成。可以在各个页面填写内容

  其他就不赘述了，想要什么功能，直接去element-ui拷贝

  下面主要说明一下Axios

1.安装axios

  ```npm install axios```
  
一般数据请求这块，会统一做个封装

在src新建api文件夹,然后分别建index.js、confing.js
```
mkdir api
touch ./api/index.js
touch ./api/config.js
```
配置axios

```src/api/config.js```
```
import axios from 'axios'
import qs from 'qs' // POST传参序列化
axios.defaults.timeout = 5000
// axios.defaults.headers.post['Content-Type'] = 'application/x-www=form-urlencoded'

export default {
  fetchGet(url, params) {
    return new Promise((resolve, reject) => {
      axios.get(url, params).then(res => {
        resolve(res.data)
      }).catch(error => {
        reject(error)
      })
    })
  },
  fetchPost(url, data = {}) {
    return new Promise((resolve, reject) => {
      axios.post(url, qs.stringify(data)).then(res => {
        resolve(res.data)
      }).catch(error => {
        reject(error)
      })
    })
  }
}
```
2.如果请求一个接口怎么办？

以下是在qq音乐上随便找的一个接口

因为存在跨域，这里用代理解决，

打开根目录下的fonfig/index配置一下proxyTable
```
proxyTable: {
      '/api': {
        target: 'https://c.y.qq.com',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    }
```
3.打开src下的api/index.js

```src/api/index.js```
```
import http from './config'
export const getList = (params) => {
  return http.fetchPost('/api/v8/fcg-bin/v8.fcg', params)
}
```
4.请求数据

打开Axios文件

```src/pages/Axios/Axios.vue```
```
<template>
  <div class="axios">
  <button @click="test">点我看控制台</button>
  </div>
</template>
<script>
import { getList } from '../../api/index'
export default {
  data() {
    return {
      list: []
    }
  },
  methods: {
    test() {
      // 传递参数
      let params = {
        channel: 'singer',
        page: 'list',
        key: 'all_all_all',
        pagesize: 100,
        pagenum: 1,
        g_tk: 5381,
        jsonpCallback: 'GetSingerListCallback',
        loginUin: 0,
        hostUin: 0,
        format: 'jsonp',
        inCharset: 'utf8',
        outCharset: 'utf-8',
        notice: 0,
        platform: 'yqq',
        needNewCode: 0
      }
      getList(params).then(res => {
        console.log(res)
      })
    }
  }
}
</script>
<style lang="scss" scoped>
.axios{
  margin: 50px 50px;
}
</style>
```
点一下看看控制台。

OK!基础教程到此结束，如果对你有些许帮助，请不要吝惜你的star
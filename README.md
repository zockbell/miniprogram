## 小程序介绍

### **1. 小程序发展史**

#### **1.1 Native App**

在智能机刚兴起的时代，网络还不是很发达，网页浏览速度也很慢，以文字为主。市面上的应用以 Native App 为主。

Native App 是基于 iOS 或者安卓的原生应用，特点是开发成本高，迭代慢，但是性能和体验很好，消息推送及时，比如 qq，微信等。

![01](https://github.com/zockbell/miniprogram/blob/master/imgs/01.webp)

---

#### 1.2 **H5**

2014 年 HTML5 完成标准定制，他的设计目的是为了在移动设备上支持多媒体，引入了 Video、Audio 等技术。

在网页上浏览视频变得很方便，特点是开发和发布成本低，打开方便，无需下载到本地，但是性能受浏览器的处理能力的限制，相较于原生 App 来说差了一些，消息推送不及时。

---

**1.3 Hybrid App**

Hybrid App 就是混合式的 App，也就是在移动端原生应用的基础上，通过 JSBrdige 等方法，访问原生应用的 API 进行 JS 的交互，并通过 WebView 等技术实现 HTML 与 CSS 的渲染。

WebView 可以理解为嵌套了一个浏览器内核（比如 webkit）的移动端组件。

这种技术实现的应用一般都是跨平台的，并且维护起来比较容易，性能介于 H5 和原生应用之间。

---

**1.4 小程序**

> 小程序是一种不需要下载安装即可使用的应用，它实现了应用 “触手可及” 的梦想，用户扫一扫或者搜一下即可打开应用。也体现了 “用完即走” 的理念，用户不用关心是否安装太多应用的问题。应用将无处不在，随时可用，但又无需安装卸载。

自 2017 年 1 月微信小程序正式发布以来，目前互联网上已经有多种小程序：

| **微信小程序**                                           | **百度智能小程序**                                       | **支付宝小程序**                                         | **QQ 小程序**                                           |
| :------------------------------------------------------- | :------------------------------------------------------- | :------------------------------------------------------- | :------------------------------------------------------ |
| ![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\02.jpg) | ![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\04.jpg) | ![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\03.jpg) | ![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\5.png) |
| 2017.1                                                   | 2018.7                                                   | 2018.9                                                   | 2019.6                                                  |

基于小程序几乎相同的技术原理，以及小程序的方便快捷的特性，还衍生出了多款小程序，比如抖音小程序、快手小程序、京东小程序、美团小程序等，帮助各大厂商更好的为用户提供便捷的服务。

2017 年初微信率先推出小程序，其他互联网公司纷纷开发出自己的小程序，经过四年发展目前全网小程序已超过 700 万，微信小程序 DAU 超过 4.5 亿。

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\20.png)

目前主流小程序平台有 10 多家，包括腾讯的微信小程序、QQ 小程序；阿里的支付宝小程序、淘宝轻店铺；字节跳动的头条小程序、抖音小程序；百度小程序等；不同平台的实现标准各不相同，开发者需要学习不同平台的开发规范做定制化开发。

  2018 年微信小程序 “跳一跳” 爆火，记得当年食堂排队打饭的时候很多同学都在玩，助力了微信小程序在用户中的扩张，也激发了其他厂商开发小程序的热潮。

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\06.png)

### 2. 原理分析

#### **2.1 双线程模型**

无论是微信小程序还是支付宝小程序还是百度智能小程序等等，他们的总体架构都是基于双线程的。

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\07.jpg)

其中用于处理业务逻辑的 JS 代码运行在单独的线程里，渲染层（template、css）则运行在另外一个单独的线程里。

以微信小程序为例：

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\08.png)

双线程模型不同于单线程模型，逻辑层与渲染层的数据交互需要通过 JSBridge，二者是通过发布订阅，基于当前比较比较著名的 MVVM，来实现数据的双向绑定的，从而实现数据通信。

这样我们在微信小程序中通过在逻辑层中 setData 来改变 Model 层的数据就能够实现视图数据的异步更新。

### ![09](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\09.png)

以下是微信小程序的生命周期：

![10](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\10.png)

#### **2.2 整体架构**

![11](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\11.png)

##### 2.2.1 **WAWebview**

小程序视图层基础库，提供视图层的基础能力：

- `Foundation`：基础模块
- `WeixinJSBridge`：消息通信模块
- `exparser`：组件系统模块
- `__virtualDOM__`：Virtual DOM 模块
- `__webViewSDK__`：WebView SDK 模块
- `Reporter`：日志上报模块 (异常和性能统计数据)

##### 2.2.2 **WAService**

小程序逻辑层基础库，提供逻辑层基础能力：

- `Foundation`：基础模块
- `WeixinJSBridge`：消息通信模块
- `WeixinNativeBuffer`：原生 Buffer
- `WeixinWorker`：Worker 线程
- `JSContext`：JS Engine Context
- `Protect`：JS 保护的对象
- `__subContextEngine__`：提供 App、Page、Component、Behavior、getApp、getCurrentPages 等方法

##### 2.2.3 **虚拟 DOM**

微信小程序在 WAService 里面实现了小程序的 `__virtualDOM__`，通过 `__virtualDOM__` 模块，可以实现 JS 对象到 DOM 对象的映射。

但是这个虚拟 DOM 通过 diff 和 patch 后并不是转换成原生的 DOM 元素，而是微信小程序里面自定义的 DOM 元素，这些 DOM 元素的操作通过 Exparser 模块来统一管理：

在 WAWebview 中包含了所有的 wx 自定义标签：

![12](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\12.png)

同时，`__virtualDOM__` 模块提供了很多的基础 API，比如：

- getAll：获取所有 Node
- getNodeById：根据 Id 获取 Node
- getNodeId：获取 NodeId
- addNode：添加节点
- removeNode：删除节点
- getExparser：获取 Exparser 对象（基于 WebComponent 的 shadow DOM 模型，可以在 JS 环境中运行，所有与节点树相关的操作都依赖于他）

##### 2.2.4 **WeiXinJSBridge**

WeixinJSBridge 提供了视图层 JS 与 Native、视图层与逻辑层之间消息通信的机制，提供了如下几个方法：

![13](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\13.png)

里面最重要的便是 on 和 invoke，通过 on 来注册事件，通过 invoke 来触发相应的事件。

#### **2.3 微信开发者工具**

微信开发者工具中的小程序是跑在 NW.js 中的，这里是他的官方 API 文档：[https://nwjs.readthedocs.io/en/latest/](https://nwjs.readthedocs.io/en/latest/)

他是基于 `Chromium` 和 `Node.js` 的，因此我们编译后的虚拟 DOM 转换成真实 DOM 后，通过他来运行。

##### 2.3.1 **一些反编译技巧**

我们可以通过开发者工具，在 Devtools 里输入 help 可以得到很多指令：

![14](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\14.png)

其中比较有用的是 openVendor。这个函数可以打开当前项目的源码，其实也就是包含了 wcc 和 wcsc 编译工具的一个文件夹：

有了这些文件之后，对我们之后的分析会很有帮助。

我们可以将这些文件拷贝到一个单独的目录，在 VSCode 中打开该项目，并安装以下插件：

![15](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\15.png)

这个插件可以将微信开发者工具中的所有以 .wxvpkg 结尾的文件进行解压缩。

同时通过他来将 **quickstart** 中的 **miniProgramJs.wxvpkg** 进行解压，得到我们在开发者工具中的源码文件。

##### 2.3.2 **编译原理**

* #### **wcc 编译 wxml**

  微信小程序提供了 wcc 工具来编译 wxml 代码。通过上面得到的代码，我们可以实现对 wxml 的编译，以开发者工具创建的 Demo 项目中的首页为例：

  ```html
  <view class="container">
    <view class="userinfo">
      <block wx:if="{{canIUseOpenData}}">
        <view class="userinfo-avatar" bindtap="bindViewTap">
          <open-data type="userAvatarUrl"></open-data>
        </view>
        <open-data type="userNickName"></open-data>
      </block>
      <block wx:elif="{{!hasUserInfo}}">
        <button wx:if="{{canIUseGetUserProfile}}" bindtap="getUserProfile"> 获取头像昵称 </button>
        <button wx:elif="{{canIUse}}" open-type="getUserInfo" bindgetuserinfo="getUserInfo"> 获取头像昵称 </button>
        <view wx:else> 请使用1.4.4及以上版本基础库 </view>
      </block>
      <block wx:else>
        <image bindtap="bindViewTap" class="userinfo-avatar" src="{{userInfo.avatarUrl}}" mode="cover"></image>
        <text class="userinfo-nickname">{{userInfo.nickName}}</text>
      </block>
    </view>
    <view class="usermotto">
      <text class="user-motto">{{motto}}</text>
    </view>
  </view>
  ```

  经过以下指令编译：

  ```node
  ./wcc ./quickstart/miniProgramJs.unpack/pages/index/index.wxml > index.js
  ```

  会得到 JS 描述文件：

  ![16](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\16.png)

  它会声明一个 $gwx 函数，通过它可以得到 Virtual DOM。接着我们在这个文件里添加几行代码去调用它，并通过 Node.js 或者 NW.js 执行这个文件：

  ```javascript
  var data = $gwx('./quickstart/miniProgramJs.unpack/pages/index/index.wxml')();
  console.log(JSON.stringify(data, null, 2));
  ```

  可以得到我们想要的最终的 Virtual DOM 结构：

  ```json
  {
    "tag": "wx-page",
    "children": [
      {
        "tag": "wx-view",
        "attr": {
          "class": "container"
        },
        "children": [
          {
            "tag": "wx-view",
            "attr": {
              "class": "userinfo"
            },
            "children": [
              {
                "tag": "wx-view",
                "attr": {},
                "children": [
                  " 请使用1.4.4及以上版本基础库 "
                ],
                "raw": {},
                "generics": {}
              }
            ],
            "raw": {},
            "generics": {}
          },
          {
            "tag": "wx-view",
            "attr": {
              "class": "usermotto"
            },
            "children": [
              {
                "tag": "wx-text",
                "attr": {
                  "class": "user-motto"
                },
                "children": [
                  ""
                ],
                "raw": {},
                "generics": {}
              }
            ],
            "raw": {},
            "generics": {}
          }
        ],
        "raw": {},
        "generics": {}
      }
    ]
  }
  ```

  然后通过 window.exparser.registerElemtent 方法将这些 tag 转换成真实 DOM：

  ![17](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\17.png)

  比如说，以上的 wx-text 就会被转换成类似于以下 DOM：

  ```html
  <span>
     <span style="display: none"></span>
     <span>{{这里是具体的文字内容}}</span>
  </span>
  ```

* **wcc 编译 wxml**

  同样的，以 Demo 项目里的 index.wxss 为例，运行一下指令：

  ```node
  ./wcsc ./quickstart/miniProgramJs.unpack/pages/index/index.wxss -js -o ./css.js
  ```

  可以将该文件从 wxss 格式的内容，转换成 JS 的内容：

  ![18](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\18.png)

  这里面会生成 setCssToHead 方法，用于将相应的 css 转换后（如 rpx 转 px 等等），通过 style 标签插入到文档的 head 里面。



#### 2.4 **通信原理**

小程序逻辑层和渲染层的通信会由 Native （微信客户端）做中转，逻辑层发送网络请求也经由 Native 转发。

**视图层组件**：

内置组件中有部分组件是利用到客户端原生提供的能力，既然需要客户端原生提供的能力，那就会涉及到视图层与客户端的交互通信。这层通信机制在 iOS 和安卓系统的实现方式并不一样，iOS 是利用了 WKWebView 的提供 messageHandlers 特性，而在安卓则是往 WebView 的 window 对象注入一个原生方法，最终会封装成 WeiXinJSBridge 这样一个兼容层，主要提供了调用（invoke）和监听（on）这两种方法。

**逻辑层接口**：

逻辑层与客户端原生通信机制与渲染层类似，不同在于，iOS 平台可以往 JavaScriptCore 框架注入一个全局的原生方法，而安卓方面则是跟渲染层一致的。

无论是视图层还是逻辑层，开发者都是间接地调用到与客户端原生通信的底层接口。一般微信小程序会对逻辑层接口做层封装后，才暴露给开发者，封装的细节可能是统一入参、做些参数校验、兼容各平台或版本问题等等。

#### 2.5 **启动机制**

小程序有冷启动与热启动两种方式：

- 假如用户已经打开过某小程序，然后在一定时间内再次打开该小程序，此时无需重新启动，只需将后台态的小程序切换到前台，这个过程就是热启动。
- 冷启动指的是用户首次打开或小程序被微信主动销毁后再次打开的情况，此时小程序需要重新加载启动。

小程序没有重启的概念：

- 当小程序进入后台，客户端会维持一段时间的运行状态，超过一定时间后（目前是 5 分钟）会被微信主动销毁。
- 当短时间内（5s）连续收到两次以上收到系统内存告警，会进行小程序的销毁。

启动流程：

![19](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\19.png)

---

### **3. UI组件库**

开发微信小程序的过程中，选择一款好用的组件库，可以达到事半功倍的效果。自从微信小程序面世以来，不断有一些开源组件库出来，下面7款就是排名比较靠前，用户使用量与关注度比较高的小程序UI组件库。

#### 3.1 WeUI组件库

WeUI WXSS是腾讯官方UI组件库WeUI的小程序版，提供了跟微信界面风格一致的用户体验。同微信原生视觉体验一致的UI组件库，由微信官方设计团队和小程序团队为微信小程序量身设计，令用户的使用感知更加统一。

> 支持[扩展库](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#useExtendedLib)引入，不占用小程序包体积

项目地址 ：https://github.com/wechat-miniprogram/weui-miniprogram

npm下载：`npm i weui-wxss`

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\21.png)

#### **3.2 iView WeApp**

iView是TalkingData发布的一款高质量的基于Vue.js组件库，而iView weapp则是它们的小程序版本。

GitHub地址：https://github.com/TalkingData/iview-weapp

npm下载：`npm i iview-weapp`

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\22.png)

#### **3.3 Vant WeApp**

Vant WeApp之前是ZanUI WeApp。是**有赞前端团队**开源的移动端组件库，于 2017 年开源，已持续维护 4 年时间。Vant 对内承载了有赞所有核心业务，对外服务十多万开发者，是业界主流的移动端组件库之一。

目前 Vant 官方提供了 [Vue 2 版本](https://vant-contrib.gitee.io/vant)、[Vue 3 版本](https://vant-contrib.gitee.io/vant/v3)和[微信小程序版本](http://vant-contrib.gitee.io/vant-weapp)，并由社区团队维护 [React 版本](https://github.com/mxdi9i7/vant-react)和[支付宝小程序版本](https://github.com/ant-move/Vant-Aliapp)。

现已包含 badge、btn、card、cell、dialog、icon、label、noticebar、panel、popup、switch、tab、toast、tooltips 等组件或元素。

GitHub地址：https://github.com/youzan/vant-weapp

npm下载：```npm i @vant/weapp -S --production```

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\23.png)

#### **3.4 Wux WeApp**

Wux WeApp也是一个非常不错的微信小程序自定义 UI 组件库，组件比较丰富，值得使用。

GitHub地址：https://github.com/wux-weapp/wux-weapp

npm下载：`npm i wux-weapp`

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\24.png)

#### **3.5 ColorUI**

ColorUI是一个Css类的UI组件库！不是一个Js框架。相比于同类小程序组件库，ColorUI更注重于视觉交互！

其组件在美观性方面比较突出。

GitHub地址：[github.com/weilanwl/Co…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fweilanwl%2FColorUI)

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\25.png)

#### **3.6 Lin UI**

Lin UI 是基于 **微信小程序原生语法** 实现的组件库。遵循简洁，易用的设计规范。

简洁、易用、灵活的微信小程序UI组件库 [doc.mini.talelin.com/](https://link.juejin.cn/?target=http%3A%2F%2Fdoc.mini.talelin.com%2F)

npm下载安装：`npm install lin-ui --production`

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\26.png)

#### **3.7 NutUI**（主要结合Taro跨端开发）

京东风格的轻量级移动端 Vue 组件库

> https://nutui.jd.com/

#### 设计初衷

在跨端小程序的开发过程中，我们发现没有合适的组件库可以使用，尤其在做电商商城类场景的业务时，没有符合京东 App 规范的组件库为我们的小程序项目提供支持。为了填补这一空白，同时让 NutUI 组件库能够为更多的开发者带来便利，我们决定在 NutUI 3.0 中 增加小程序多端适配的能力。

Taro 在小程序跨端开发中有着出众的表现，Taro 3x 在 2020年11月也宣布支持了 Vue3，所以我们可以采用 Taro + Vue 的技术栈来达到在小程序中适配多端的目的。

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\29.png)

github地址：https://github.com/jdf2e/nutui

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\27.png)

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\28.png)

---

### **4. 跨平台统一开发解决方案**

 2019 年 9 月 12 日阿里巴巴主导发起并联合 W3C 中国及国内外厂商起草了 MiniApp 标准化白皮书（MiniApp Standardization White Paper），旨在制定共同标准减少平台差异，并成立了相关工作组（作为小程序元老的腾讯没有参加，毕竟如果标准化落地的话意味着现有的微信小程序可以以极低的成本迁移到其他平台，对于拥有全网近半数小程序的腾讯无疑是一种损失）。截至目前产出了 lifecycle，manifest，packaging 等几篇文档，**但从目前来看各平台对这些标准的实现度还很低，并未实现统一，所以这么来看标准化的路看着还很长**。

在当下要解决跨平台开发问题可以采用框架转换。

在纵观整个前端的历史，无论是 CSS 预处理器的大行其道、各种模版的流行，还是 CoffeeScript 乃至  TypeScript  的诞生，都印证了这个说法，微信小程序这里也不例外。因此，各种小程序开发框架如百花齐放，层出不穷。

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\30.png)

从框架的语法来说大致分为 React、Vue 两类：

#### **4.1 Taro：React 阵营代表**

> https://taro-docs.jd.com/taro/docs/

通过实现一套  `DOM/BOM`  的 API（实际会操作 Taro DOM Tree -- Taro 自定义的一颗虚拟 dom 树）通过 template Taro DOM Tree 在运行时动态生成页面结构。优势是框架兼容性更高支持：React、Vue、Preact 等，实现了一套自己的事件机制不依赖特定平台。同时缺点也比较明显：深层嵌套的 template 结构导致包体积略大，实际生成页面冗余节点较多

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\31.png)

#### **4.2 Uni-app：Vue 阵营代表**

> https://uniapp.dcloud.io/

提供 IDE 、开发配套流程完整上手容易，有最多的使用者。在编译阶段将 Vue SFC 写法的代码编译成 小程序代码文件（JS、WXML、WXSS、JSON），在运行阶段进行 vue 和 page 实例的绑定，将 patch 阶段的 dom 操作替换为 setData。优点是生成的页面结构与原生接近没有冗余节点。缺陷主要是可用框架比较局限，基本是 fork 特定版本的 vue 做定制化开发，无法支持其他版本和框架

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\32.png)

目前从一些社区反馈及测试结果来看，uniapp 在开发工具链、多端一致性体验上相对领先。Taro 背靠京东在自家多个小程序上使用（京东购物在阿拉丁小程序指数中排入 TOP10 ），近一年更新也很频繁，最新版本已经在适配鸿蒙应用。**框架间并没有明显拉开差距，实际在选型时可能主要还是看团队本身的技术栈偏好。**

除了使用框架开发外，在开发过程中也可以使用一些转换器来帮助将单平台小程序转换为其他平台的版本。目前主流的转换工具包括以下几个：

- antmove：目前较为流行的多端转换器，可将支付宝/微信小程序转换为多端小程序，支持增量编译。转换支持度概览
- wx2swan: 微信小程序到百度小程序
- @qihoo/wx2qh：微信小程序到 360 小程序
- miniprogram-to-uniapp：uniapp 提供的转换工具，可将原生微信小程序转成 uniapp 项目来适配多端
- @tarojs/cli：tarojs 提供的转换工具，也只能将原生微信小程序应用转换为 Taro 项目

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\33.png)

可以看出上述 npm 包的下载量都不多，说明该方法目前使用场景太小了，很多包已经很久没有更新，所以**预计今年也不会有什么大的改变**。

随着一些跨端框架（Uniapp、Taro）的出现部分跨端转换器基本已不再维护，这边仅作补充。我们接下来来了解一些跨端转换器相关的内容：

- wept: 微信小程序实时运行工具，目前支持 Web、iOS 两端的运行环境
- hera-cli: 用小程序方式来写跨平台应用的开发框架，可以打包成 Android 、 iOS 应用，以及 H5
- weweb-cli: 兼容小程序语法的前端框架，可以用小程序的写法，来写 web 应用

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\34.png)

**跨端这项技术并非为了完全替代原生开发，针对每个场景我们都可以用原生写出性能最佳的代码**。但是这样做工作量太大，实际项目开发中需要掌握效率与优化之间的平衡，跨端的使用场景并不一定是项目级别的，可以是业务级甚至是页面级的。

跨端的优势在于能让我们在书写更有效率的代码、拥有更丰富的生态的同时，还带来了不错的性能。

#### **4.3 kbone**

> https://developers.weixin.qq.com/miniprogram/dev/platform-capabilities/extended/kbone/
>
> https://wechat-miniprogram.github.io/kbone/docs/#%E4%BB%8B%E7%BB%8D
>
> https://wechat-miniprogram.github.io/kbone/docs/qa/#%E9%99%90%E5%88%B6

kbone 是一个致力于微信小程序和 Web 端同构的解决方案。

微信小程序的底层模型和 Web 端不同，我们想直接把 Web 端的代码挪到小程序环境内执行是不可能的。kbone 的诞生就是为了解决这个问题，它实现了一个适配器，在适配层里模拟出了浏览器环境，让 Web 端的代码可以不做什么改动便可运行在小程序里。

因为 kbone 是通过提供适配器的方式来实现同构，所以它的优势很明显：

- 大部分流行的前端框架都能够在 kbone 上运行，比如 Vue、React、Preact 等。
- 支持更为完整的前端框架特性，因为 kbone 不会对框架底层进行删改（比如 Vue 中的 v-html 指令、Vue-router 插件）。
- 提供了常用的 dom/bom 接口，让用户代码无需做太大改动便可从 Web 端迁移到小程序端。
- 在小程序端运行时，仍然可以使用小程序本身的特性（比如像 live-player 内置组件、分包功能）。
- 提供了一些 Dom 扩展接口，让一些无法完美兼容到小程序端的接口也有替代使用方案（比如 getComputedStyle 接口）

以上对比：

`taro` 和 `uni-app` 是比较典型的多端框架，发布到各个端均可。而 `kbone` 只支持微信小程序和H5。

taro 和 uni-app 均将常用接口及组件封装了成了跨端API和跨端组件，组件规范沿用微信小程序的规范，部分平台特有API，这两个框架亦有应对方案：

taro：支持与小程序代码混写，可通过混写的方式调用框架尚未封装的小程序新增API
uni-app：支持条件编译，可在条件编译代码块中，随意调用各个平台新增的API及组件
`taro` 和 `uni-app` 可以不受限的调用各家小程序平台所有的API及组件。

kbone 沿用web的开发习惯，使用html标签及js api；涉及微信特有api时，可通过process.env.isMiniprogram判断环境，然后编写微信原生代码。对于html中没有标签可替代的微信内置组件（如swiper），需要使用 wx-component 标签或者使用 wx- 前缀，这样的内置组件会被包裹一层自定义组件，带来相应的性能开销。

除了接口、组件之外，我们以微信小程序为例，找几个典型能力对比框架支持度：

| 框架           | taro | uni-app | kbone |
| -------------- | ---- | ------- | ----- |
| 微信自定义组件 | ⭕️    | ⭕️       | ⭕️     |
| 三方插件       | ⭕️    | ⭕️       | ❌     |
| 分包加载       | ⭕️    | ⭕️       | ⭕️     |
| sitemap        | ⭕️    | ⭕️       | ⭕️     |
| wxs            | ❌    | ⭕️       | ❌     |
| 云开发         | ⭕️    | ⭕️       | ⭕️     |



**插件市场**
一个开发框架能否成功，除了框架自身的高度产品化外，开发者生态的构建也至关重要。

uni-app 于2018年底率先推出插件市场，支持前端组件、js sdk、页面模板、项目模板、原生插件等多种类型，且提供了赞赏、付费购买等手段，刺激轮子作者的创作激情。目前市场上已发布插件接近1500个，众多插件下载量都在万次以上。

Taro 于 2019年5月上线物料市场，目前市场上已发布物料90个；从热门榜单来看，下载量并不大，下载最多的也就数百。

![](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\35.png)

**跨端灵活性**
跨端开发，离不开条件编译。因为不能用统一来抹杀各个平台的特色。

优良的条件编译能力对各端开发的灵活度至关重要，可以让开发者在共享和个性化之间游刃有余。

taro 、uni-app和 kbone 均支持在js代码通过process.env判断平台，然后编写平台特有代码。

taro额外支持统一接口的多端文件编码方式，以及在样式文件中使用ifdef条件编译。

uni-app是全面可条件编译的，目录、文件、配置、组件、js、css，所有一切均可通过ifdef条件编译。

跨端支持小结结论：uni-app > taro > kbone


**开发体验**
taro、uni-app、kbone均支持cli模式，可以在主流前端工具中开发，且基本都带有d.ts的语法提示库。三个框架均支持主流的vue或react语法，配套的ide工具链较丰富，着色、校验、格式化完善。

相比微信原生，这三个开发框架的开发体验都更为优秀。

但在开发工具维度上，明显高出一截的框架是uni-app，其出品公司同时也是HBuilderX的出品公司DCloud.io，HBuilderX为uni-app做了很多优化，代码提示、转到定义、easycom、运行调试…故uni-app的开发效率、易用性非其他框架可及。

开发体验维度，对比结果：uni-app > taro,kbone


---

### **5. 总结**

- 小程序拥有接近原生 App 的体验。
- 小程序并不是真正的 **“无需下载”**，只是小程序的体积很小，在当今高速的网络环境下能够快速下载，用户感知不到，更确切的来说是 **“无感下载”**。
- 基于移动端布局的局限性，可以高效且简单的开发，迭代快速。
- 小程序是双线程模型，逻辑层和渲染层分别运行在不同的线程中，通过 JSBridge 进行通信。
- 在小程序里有实现专门的 JSBridge 来实现 JS 和 Native 的双向调用。
- wxml 文件通过 wcc 编译：wxml => JS => VirtualDOM。
- wxss 文件通过 wcsc 编译：wxss => JS => style。
- 小程序其实也是一种 hybrid 技术，但是他围绕宿主应用，实现了更为强大的生态，提供更为便捷的服务。
- 目前小程序跨平台统一开发解决方案逐渐成熟。

---

### **6. 2021年小程序互联网发展白皮书**

> 阿拉丁地址：https://www.aldzs.com/viewpointarticle?id=16175



参考文献：

https://blog.csdn.net/hbcui1984/article/details/105430682

https://www.aldzs.com/viewpointarticle?id=16175

https://juejin.cn/post/6844903810788245511

https://mp.weixin.qq.com/s/Cbne5rm71ci0OW0-8Hx8dQ

https://developers.weixin.qq.com/ebook?action=get_post_info&docid=0000286f908988db00866b85f5640a


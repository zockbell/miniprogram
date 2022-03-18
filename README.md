## 小程序介绍

### **1. 小程序发展史**

#### **1.1 Native App**

在智能机刚兴起的时代，网络还不是很发达，网页浏览速度也很慢，以文字为主。市面上的应用以 Native App 为主。

Native App 是基于 iOS 或者安卓的原生应用，特点是开发成本高，迭代慢，但是性能和体验很好，消息推送及时，比如 qq，微信等。

![01](D:\pep\37--2022年项目\05-小程序介绍文档\imgs\01.webp)

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

### **3. 总结**

- 小程序拥有接近原生 App 的体验。
- 小程序并不是真正的 **“无需下载”**，只是小程序的体积很小，在当今高速的网络环境下能够快速下载，用户感知不到，更确切的来说是 **“无感下载”**。
- 基于移动端布局的局限性，可以高效且简单的开发，迭代快速。
- 小程序是双线程模型，逻辑层和渲染层分别运行在不同的线程中，通过 JSBridge 进行通信。
- 在小程序里有实现专门的 JSBridge 来实现 JS 和 Native 的双向调用。
- wxml 文件通过 wcc 编译：wxml => JS => VirtualDOM。
- wxss 文件通过 wcsc 编译：wxss => JS => style。
- 小程序其实也是一种 hybrid 技术，但是他围绕宿主应用，实现了更为强大的生态，提供更为便捷的服务。



小程序的实时数据展示

https://mp.weixin.qq.com/s/Cbne5rm71ci0OW0-8Hx8dQ

https://developers.weixin.qq.com/ebook?action=get_post_info&docid=00064a9b998288cb0086840c65940a

阿拉丁：

https://www.aldzs.com/viewpointarticle?id=16175
---
title: 基于React-native+Taro开发
category: 
  [React Native]
tags:
  [React Native]
category_bar: true
---

# 基于React-native+Taro开发

**开发平台：Windows **

**目标平台：Android**

本文基于对[**React Native 中文网**](https://reactnative.cn/)及[**Taro官网**](https://taro.jd.com/)总结环境搭建，并记录开发过程中遇到的问题



## 环境搭建

### 安装依赖

必须安装的依赖有：**Node**、**JDK** 和 **Android Studio**。

虽然你可以使用`任何编辑器`来开发应用（编写 js 代码），但你仍然**必须安装 Android Studio **来获得编译 Android 应用所需的**工具**和**环境**。

#### [Node](https://nodejs.org/en)

版本应大于等于 14，使用Node建议使用安装[**nvm**](https://github.com/coreybutler/nvm-windows/releases)控制node版本，使用nrm控制镜像源及查看当前镜像源延迟

> 注意：不要使用 cnpm！cnpm 安装的模块路径比较奇怪，packager 不能正常识别！

#### [JDK](https://www.oracle.com/cn/java/technologies/downloads/)

react-native新版本不再支持**jdk1.7**和**jdk1.8**,需要 Java Development Kit [JDK] 11。你可以在命令行中输入 `javac -version`（请注意是 javac，不是 java）来查看你当前安装的 JDK 版本。如果版本不合要求，则可以去[Temurin](https://adoptium.net/?variant=openjdk11&jvmVariant=hotspot)或[Oracle JDK](https://www.oracle.com/java/technologies/downloads/#java11)上下载(后者下载需注册登录)

> 低于 0.67 版本的 React Native 需要 JDK 1.8 版本（官方也称 8 版本）。

#### [Android Studio](https://developer.android.google.cn/studio/features?hl=zh-cn)

Android Studio安装可直接参考[**React Native 中文网 Android 开发环境**](https://reactnative.cn/docs/environment-setup#android-%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83)

可通过查看设备命令行**`adb devices`**判断环境是否安装成功

> 需要注意配置`环境变量`时`%ANDROID_HOME%`不生效时可替换成`%ANDROID_SDK_ROOT%`,根据文档规定配置`%ANDROID_HOME%`即可，但是笔者在配置过程中首次使用`%ANDROID_HOME%`，导致`platform-tools`中`adb devices`只有首次生效，后调整成`%ANDROID_SDK_ROOT%`排除bug，但是当第二次配置成`%ANDROID_HOME%`却没有bug出现，**不排除其他环境配置问题导致，示例仅供参考**

> 如果定义了 ANDROID_HOME 并且其中包含有效的 SDK 安装，系统会使用 ANDROID_HOME 的值而不是 ANDROID_SDK_ROOT 中的值。
>
> - 如果未定义 ANDROID_HOME，系统会使用 ANDROID_SDK_ROOT 中的值。
> - 如果定义了 ANDROID_HOME，但其中不存在或不包含有效的 SDK 安装，系统会使用 ANDROID_SDK_ROOT 中的值。



## 运行调试

> 在开发过程中，由于线材、设备分辨率等限制，优先推荐使用物理机局域网调试，当不需要过多的额外的挂载设备时推荐使用虚拟机调试

### 物理机局域网调试

> 调试的前提需要先将`debug`包(非realease包)安装到移动设备上，那么直接使用命令行`yarn taro build --type rn --reset-cache`(基于taro)即可

1. 首先确保你的电脑和手机设备在**同一个 Wi-Fi 环境**下。
2. 在设备上运行你的 React Native 应用。和打开其它 App 一样操作。
3. 你应该会看到一个“红屏”错误提示。这是正常的，下面的步骤会解决这个报错。
4. 摇晃设备，或者运行`adb shell input keyevent 82`，可以打开**开发者菜单**。
5. 点击`Dev Settings` -> `Debug server host for device`。
6. 输入你电脑的 IP 地址和端口号（譬如 10.0.1.1:8081）。**在 Windows 上**，打开命令提示符并输入`ipconfig`来查询你的 IP 地址。
7. 回到**开发者菜单**然后选择`Reload JS`。

> 当以上操作流程无误，依然无法触发Reload JS,请关闭React Native 应用，并将二者的网络断开重新连接，如果依然不生效，请重启设备



### 虚拟机调试

需要先[创建一个虚拟设备](https://developer.android.com/studio/run/managing-avds.html)。点击"Create Virtual Device..."，然后选择所需的设备类型并点击"Next"，然后选择**S** API Level 31 image

> 请不要轻易点击 Android Studio 中可能弹出的建议更新项目中某依赖项的建议，否则可能导致无法运行。

### 物理机USB调试

使用 Android 真机来代替模拟器进行开发，只需用 usb 数据线连接到电脑，然后遵照[在设备上运行](https://reactnative.cn/docs/running-on-device)这篇文档的说明操作即可

### 编译并运行 React Native 应用

> 使用虚拟机调试可完全跳过该步骤，只需要`yarn start`启动项目即可

确保你先运行了模拟器或者连接了真机，然后在你的项目目录中运行`yarn android`或者`yarn react-native run-android`：

```jsx
cd AwesomeProject
yarn android
# 或者
yarn react-native run-android
```



此命令会对项目的原生部分进行编译，同时在另外一个命令行中启动`Metro`服务对 js 代码进行实时打包处理（类似 webpack）。`Metro`服务也可以使用`yarn start`命令单独启动。

如果配置没有问题，你应该可以看到应用自动安装到设备上并开始运行。注意第一次运行时需要下载大量编译依赖，耗时可能数十分钟。此过程`严重依赖稳定的代理软件`，否则将频繁遭遇链接超时和断开，导致无法运行。

也可以尝试阿里云提供的[maven 镜像](https://help.aliyun.com/document_detail/102512.html?spm=a2c40.aliyun_maven_repo.0.0.361865e90r2x4b)，将`android/build.gradle`中的`jcenter()`和`google()`分别替换为`maven { url 'https://maven.aliyun.com/repository/jcenter' }`和`maven { url 'https://maven.aliyun.com/repository/google' }`（注意有多处需要替换）。

`npx react-native run-android`只是运行应用的方式之一。你也可以在 Android Studio 中直接运行应用。

> 译注：建议在`run-android`成功后再尝试使用 Android Studio 启动。请不要轻易点击 Android Studio 中可能弹出的建议更新项目中某依赖项的建议，否则可能导致无法运行。

> 如果你无法正常运行，遇到奇奇怪怪的红屏错误，先回头`仔细对照文档检查`，然后可以看看[问题讨论区](https://github.com/reactnativecn/react-native-website/issues)。不同时期不同版本可能会碰到不同的问题，我们会在论坛中及时解答更新。但请注意***千万不要\***执行 bundle 命令，那样会导致代码完全无法刷新。



## 实践

**技术栈：react-native taro taro-ui**

**设备：东集Seuic手持终端PDA**

> 东集Seuic手持终端PDA未知来源初始密码:4007770876



### React Native组件

#### ScrollView

在开发过程中，同一个页面中存在多层ScrollView嵌套问题，此时会存在只有最外层的ScrollView生效，其他的ScrollView并未生效，但是

ScrollView本身提供了属性nestedScrollEnabled支持嵌套

##### 最优解

```react
<ScrollView >
    ...
  <ScrollView nestedScrollEnabled={true}>
   ...
   </ScrollView>
    ..
</ScrollView>
```

##### 自定义实现

通过state状态值来控制外层的ScrollView是否可以滚动

```react
 const [showScroll, setShowScroll] = useState<boolean>(false)
 return (
     <ScrollView scrollEnabled={showScroll}>
                <ScrollView
                    onTouchStart={(ev) => {
                        console.log('开始滑动');
                        setShowScroll(false)
                    }}
                    onMomentumScrollEnd={(e) => {
                        console.log('滑动结束');
                        setShowScroll(true)
                    }}
                    //当用户开始拖动此视图时调用此函数
                    onScrollBeginDrag={() => {
                        console.log('拖动开始');
                        setShowScroll(false)
                    }}
                    //当用户停止拖动此视图时调用此函数
                    onScrollEndDrag={(e) => {
                        console.log('拖动结束');
                        setShowScroll(true)
                    }}
                    style={{ margin: 10, maxHeight: 300 }}
                >
                </ScrollView>
     </ScrollView>
 )
```

#### 按钮需要点击两次生效

在页面的开发过程中，发现页面上的按钮需要触发两次才能生效(先失焦，再聚焦)，点击按钮需要两次的原因是，焦点还在原来的TextInput上面，解决的办法就是，监听软键盘的消失方法，然后在消失方法里面，再一次调用软键盘消失方法：但是只推荐react-native官方提供的方法，即使用组件用ScrollView或FlatList组件包裹，给ScrollView或FlatList组件添加如下属性`keyboardShouldPersistTaps='handled'`

> 如果当前界面有软键盘，那么点击 scrollview 后是否收起键盘，取决于本属性的设置。
>
> （译注：很多人反应 TextInput 无法自动失去焦点/需要点击多次切换到其他组件等等问题，其关键都是需要将 TextInput 放到 ScrollView 中再设置本属性）
>
> - `'never'` （默认值），点击 TextInput 以外的子组件会使当前的软键盘收起。此时子元素不会收到点击事件。
> - `'always'`，键盘不会自动收起，ScrollView 也不会捕捉点击事件，但子组件可以捕获。
> - `'handled'`，当点击事件被子组件捕获时，键盘不会自动收起。这样切换 TextInput 时键盘可以保持状态。多数带有 TextInput 的情况下你应该选择此项。
> - `false`，已过时，请使用'never'代替。
> - `true`，已过时，请使用'always'代替。

```react
<ScrollView keyboardShouldPersistTaps='handled'>
```

若基于taro框架以上配置未生效，需要在每个页面入口index.tsx同级目录添加一个index.config.ts文件

```typescript
//index.config.ts
export default definePageConfig({
    disableScroll: true
  })
  
```

#### Text

一个用于显示文本的 React 组件，并且它也支持嵌套、样式，以及触摸处理。属性`selectable`决定用户是否可以长按选择文本，以便复制和粘贴。

```react
<Text selectable={true}>and red</Text>
```


# frp-Android
A frp client for Android  
一个Android的frpc客户端

简体中文 | [English](README_en.md)

<div style="display:inline-block">
<img src="./image/image1.png" alt="image1.png" width="200">
<img src="./image/image2.png" alt="image2.png" width="200">
</div>

## 编译方法

如果您想自定义frp内核，可以通过Github Actions或通过Android Studio编译

### 通过Github Actions编译

1. 将您的apk签名密钥文件转为base64，以下为Linux示例
```shell
base64 -w 0 keystore.jks > keystore.jks.base64
```
2. fork本项目
3. 转到Github项目的此页面：Settings > Secrets and variables > Actions > Repository secrets
4. 添加以下四个环境变量：
```KEY_ALIAS``` ```KEY_PASSWORD``` ```STORE_FILE``` ```STORE_PASSWORD```  
其中```STORE_FILE```的内容为步骤1的base64，其他环境变量内容请根据您的密钥文件自行填写
5. Push提交自动触发编译或在Actions页面手动触发

### 通过Android Studio编译

1. 在项目根目录创建apk签名密钥设置文件```keystore.properties```，内容参考同级的```keystore.example.properties```
2. 使用Android Studio进行编译打包

### 通过命令行打包

如果你只想在命令行环境下完成打包，可以按照下面的步骤进行：

1. 确保本机已经安装好 Android SDK，并在项目根目录创建 ```local.properties``` 指定 ```sdk.dir```（Android Studio 会自动生成该文件，如果已经存在可跳过）。
2. 在项目根目录创建签名配置文件 ```keystore.properties```，字段与示例文件保持一致。
3. 执行 ```./gradlew assembleRelease``` 生成签名的发布 APK，或者执行 ```./gradlew assembleDebug``` 生成调试包。
4. 打包成功后，可在 ```app/build/outputs/apk``` 目录下找到相应的 APK 文件。

> **提示**：首次执行 Gradle Wrapper 会从网络下载对应版本的 Gradle，如果处于内网或无法直接访问官方源，可提前在可联网环境下载好 `gradle-8.9-bin.zip` 并配置本地镜像，或设置 HTTP 代理后再运行上述命令。

## 常见问题
### 项目的frp内核(libfrpc.so)是怎么来的？
直接从[frp的release](https://github.com/fatedier/frp/releases)里把对应ABI的Linux版本压缩包解压之后重命名frpc为libfrpc.so  
项目不是在代码里调用so中的方法，而是把so作为一个可执行文件，然后通过shell去执行对应的命令  
因为Golang的零依赖特性，所以可以直接在Android里通过shell运行可执行文件

### 开机自启与后台保活
按照原生Android规范设计，如有问题请在系统设置内允许开机自启/后台运行相关选项
# 自动构建Lean's 大神的OpenWrt固件

每个月1号，自动拉取最新的Lean大神的代码，配置常用软件构建镜像

```bash
$ docker pull crazygit/lean-openwrt-x86-64:latest
```

如果你偏好纯净的官方原版本的OpenWrt固件，可以使用我编译的原版固件

```bash
$ docker pull crazygit/openwrt-x86-64
```

自己编译的固件打包成Docker镜像以及使用配置，请参考:

<https://github.com/crazygit/openwrt-x86-64>

Lean's大神项目地址:

<https://github.com/coolsnowwolf/lede>

本库使用的github action是基于`P3TERX`大神的模板做了修改

<https://github.com/P3TERX/Actions-OpenWrt>

github action相关的使用，请参考[原作者的博客](https://p3terx.com/archives/build-openwrt-with-github-actions.html)

## 补充

如下根据自身的使用场景，基于[原作者博客](https://p3terx.com/archives/build-openwrt-with-github-actions.html)内容的基础上，做的一点补充

### 如何生成`.config`

`.config`文件的作用就是定制编译过程中需要做的配置
如:
* 系统的配置
* 需要预装的软件包

它的生成方式有如下几种

### 方法一(拿来主义)

直接使用别人配置好的，如下是别的大神挑选的常用预装软件生成的配置，下载下来即可使用

<https://raw.githubusercontent.com/esirplayground/AutoBuild-OpenWrt/master/x86_64.config>

### 方法二(自己动手)

直接在`.github/workflows/build-openwrt.yml`开启ssh登录，然后ssh到服务端执行`make menuconfig`进行定制

### 方法三(自娱自乐)

这个方法其实是方法二的变形，只是所有的东西都要你自己来，自己搭建编译环境生成`.config`，

首先参考[链接](https://github.com/coolsnowwolf/lede)搭建本地编译环境,然后执行

```bash
$ make menuconfig
```

选择要安装的软件包和定制的配置项

```bash
$ ./scripts/diffconfig.sh > diffconfig # write the changes to diffconfig
$ cp diffconfig .config   # write changes to .config
$ make defconfig   # expand to full config
```

编译步骤定制可以参考:

<https://openwrt.org/docs/guide-developer/build-system/use-buildsystem>

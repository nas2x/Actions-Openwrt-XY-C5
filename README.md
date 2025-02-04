# Github Actions Lean Openwrt XY-C5

- Openwrt源码是[coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)。
- Github Actions来自于[P3TERX/Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt)，[中文教程](https://p3terx.com/archives/build-openwrt-with-github-actions.html)。
- 编译目标为小娱路由XY-C1/C5
- 默认登陆IP 192.168.1.1 密码 password

## 编译固件配置说明

### 固件包含以下插件
- IPv6支持
- Helloworld
- 网络共享（Samba）
- 硬盘休眠
- UPnP
- 解锁网易云灰色歌曲
- 网络唤醒
- Argon 主题

## Config文件生成参考

- `make menuconfig` 可参考[OpenWrt MenuConfig设置和LuCI插件选项说明](https://mtom.ml/827.html)，一般先选`Target System`，`Subtarget`，`Target Profile`，再选`LUCI`插件。
- .config文件生成可借助WSL或Ubuntu虚拟机，执行以下命令
```
sudo sed -i 's#http://archive.ubuntu.com#https://mirrors.huaweicloud.com#' /etc/apt/sources.list
sudo sed -i 's#http://security.ubuntu.com#https://mirrors.huaweicloud.com#' /etc/apt/sources.list
sudo apt update
sudo apt upgrade -y
sudo apt-get -y install subversion libncurses5-dev git git-core build-essential unzip bzip2 python2.7
git clone https://github.com/coolsnowwolf/lede
cd lede
sed -i 's/^#\(.*helloworld\)/\1/' feeds.conf.default
./scripts/feeds update -a && ./scripts/feeds install -a
make menuconfig
make defconfig
./scripts/diffconfig.sh > seed.config
```
进入目录`\\wsl$\Ubuntu*\home\*\lede`复制一下这个`seed.config`的文本内容到项目根目录的`.config`文件中，这样就不用每次都SSH连接到 Actions生成编译配置，真正一键编译。
- 差分文件seed.config[参考来源](https://github.com/coolsnowwolf/lede/issues/2288)

## Custom files（自定义文件）

- 自定义文件 “files 大法”是把你自定义的配置编译到固件里。这样升级或恢复出厂设置都不需要保留配置，缺省值就是自定义的配置。
- 如你现在的network设置编译进固件：首先提取路由固件下的`\etc\config\network` 然后在项目根目录下创建files目录并`push` 到 `\files\etc\config\network `，最后编译出来的固件就是现在设置的network。
- 另外使用“files 大法”仓库最好设为私有，否则你的配置信息，如宽带账号等会公开在网上。

## 相关参考

- [超简单云编译](https://github.com/281677160/build-openwrt)
- [借助 GitHub Actions 的 OpenWrt 在线集成自动编译](https://github.com/KFERMercer/OpenWrt-CI)
- [qughij/openwrt-xiaoyu_xy-c5](https://github.com/qughij/openwrt-xiaoyu_xy-c5)
- [SuLingGG/OpenWrt-Rpi](https://github.com/SuLingGG/OpenWrt-Rpi)
- [IvanSolis1989/OpenWrt-DIY](https://github.com/IvanSolis1989/OpenWrt-DIY)
- [zlxj2000/Openwrt-firmware](https://github.com/zlxj2000/Openwrt-firmware)
- [SuLingGG/Action-OpenWrt-Plus](https://github.com/SuLingGG/Action-OpenWrt-Plus)
- [Lancenas/Actions-Lean-OpenWrt](https://github.com/Lancenas/Actions-Lean-OpenWrt)
- [Lancenas/actions-openwrt-helloworld](https://github.com/Lancenas/actions-openwrt-helloworld)
- [xiaorouji/openwrt-passwall](https://github.com/xiaorouji/openwrt-passwall)
- [kenzok8/openwrt-packages](https://github.com/kenzok8/openwrt-packages)
- [kenzok8/small](https://github.com/kenzok8/small)
- [基本的Git技能](https://www.liaoxuefeng.com/wiki/896043488029600)
- [面向小白的Github_Action使用workflow自动编译lean_openwrt教程](https://zhuanlan.zhihu.com/p/94402324)
- [关于Github Action自动编译Lean_Openwrt的配置修改问题](https://zhuanlan.zhihu.com/p/94527343)
- [firker/diy-ziyong](https://github.com/firker/diy-ziyong)
- [xiaoqingfengATGH/feeds-xiaoqingfeng](https://github.com/xiaoqingfengATGH/feeds-xiaoqingfeng)
- [OpenWrt 编译 LuCI -> Applications 添加插件应用说明-Lean【2021.11.18更新】](https://www.right.com.cn/forum/thread-344825-1-1.html)
- [利用Github Actions云编译并自定义OpenWrt固件【2021.11.05更新】](https://jarviswwong.com/github-actions-compile-opwnwrt-firmware.html)
- [编译属于自己的OpenWrt固件【2021.08.05更新】](https://post.smzdm.com/p/ad27vkgn/)
- [编译Lean的Openwrt固件全攻略【2021.8.18更新】](https://imgki.com/archives/openwrt-lean.html)
- [OpenWRT 软件包中文对照说明](http://www.wbini.com/id-12.html)


## License

[MIT](https://github.com/P3TERX/Actions-OpenWrt/blob/main/LICENSE) © P3TERX

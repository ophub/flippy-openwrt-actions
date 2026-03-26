# 功能说明

[English Instructions](README.md) | [中文说明](README.cn.md)

[unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit) 是 `Flippy` 开发的 OpenWrt 打包脚本仓库。支持全志（微加云）、瑞芯微（贝壳云、我家云、电犀牛 R66S/R68S、瑞莎 5B/E25、西瓜皮等）以及晶晨 S9xxx 系列型号（如 S905x3、S905x2、S922x、S905x、S905d、S905、S912 等）设备。

本 Action 直接调用其打包脚本，未做任何修改，仅在此基础上进行了智能化的 GitHub Action 封装开发，使通过 GitHub Actions 进行固件打包更加便捷和灵活。

## OpenWrt 固件默认信息

| 系统名称        | 默认账号 | 默认密码  | SSH 端口 | IP 地址 |
| -------------- | ------- | ------- | ------- | ------- |
| 🛜 [OpenWrt.OS](https://github.com/ophub/flippy-openwrt-actions/releases) | root | password | 22 | 192.168.1.1 |
| 🐋 [OpenWrt.Docker](https://hub.docker.com/u/ophub) | root | password | 22 | 192.168.1.1 |

## 使用方法

在 `.github/workflows/*.yml` 工作流文件中引入此 Action 即可使用，例如 [package-openwrt-image.yml](.github/workflows/package-openwrt-image.yml)，示例代码如下：

```yaml
- name: Package OpenWrt Firmware
  uses: ophub/flippy-openwrt-actions@main
  env:
    OPENWRT_ARMSR: openwrt/bin/targets/*/*/*.tar.gz
    PACKAGE_SOC: all
    KERNEL_VERSION_NAME: 6.12.y_6.18.y
    KERNEL_AUTO_LATEST: true
    OPENWRT_IP: 192.168.1.1
```

## 可选参数说明

基于 `Flippy` 最新发布的内核打包脚本，针对 `打包文件`、`make.env`、`内核版本选择`、`目标设备 SoC` 等提供了可选参数配置。

| 参数                   | 默认值                  | 说明                                            |
|------------------------|------------------------|------------------------------------------------|
| OPENWRT_ARMSR          | 无                     | 必选项。设置 `openwrt-armsr-armv8-generic-rootfs.tar.gz` 的文件路径，可使用相对路径（如 `openwrt/bin/targets/*/*/*.tar.gz`）或网络下载地址（如 `https://github.com/*/releases/*/*.tar.gz`） |
| SCRIPT_REPO_URL        | unifreq/openwrt_packit | 设置打包脚本源码仓库的 `<owner>/<repo>` |
| SCRIPT_REPO_BRANCH     | master                 | 设置打包脚本源码仓库的分支                        |
| KERNEL_REPO_URL        | breakingbadboy/OpenWrt | 设置内核下载仓库的 `<owner>/<repo>`，默认从 breakingbadboy 维护的[内核 Releases](https://github.com/breakingbadboy/OpenWrt/releases/tag/kernel_stable) 下载。 |
| KERNEL_VERSION_NAME    | 6.12.y_6.18.y          | 设置[内核版本](https://github.com/breakingbadboy/OpenWrt/releases/tag/kernel_stable)，可查看并选择指定版本。支持指定单个内核（如 `6.12.y`），也可用 `_` 连接多个内核（如 `6.12.y_6.18.y`） |
| KERNEL_AUTO_LATEST     | true                   | 设置是否自动采用同系列最新版本内核。当设为 `true` 时，将自动在内核库中查找 `KERNEL_VERSION_NAME` 所指定内核（如 `6.12.y`）的同系列是否存在更新版本，若有则自动替换为最新版。设为 `false` 时将使用指定版本的内核进行编译。 |
| PACKAGE_SOC            | all                    | 设置打包目标设备的 `SOC`，默认 `all` 打包全部设备，可指定单个设备（如 `s905x3`），也可用 `_` 连接多个设备（如 `s905x3_s905d`）。各设备的 SoC 代码为：`100ask-dshanpi-a1`, `vplus`, `cm3`, `jp-tvbox`, `beikeyun`, `l1pro`, `rock5b`, `rock5c`, `e52c`, `e54c`, `r66s`, `r68s`, `e25`, `photonicat`, `watermelon-pi`, `yixun-rs6pro`, `zcube1-max`, `ak88`, `ht2`, `e20c`, `e24c`, `h28k`, `h66k`, `h68k`, `h69k`, `h69k-max`, `h88k`, `h88k-v3`, `rk3399`, `s905`, `s905d`, `s905x2`, `s905x3`, `s912`, `s922x`, `s922x-n2`, `qemu`, `diy`。说明：`s922x-n2` 是 `s922x-odroid-n2`，`diy` 是自定义设备。 |
| OPENWRT_IP             | 192.168.1.1            | 设置 OpenWrt 的默认 `IP` 地址                   |
| GZIP_IMGS              | auto                   | 设置打包完成后的固件压缩格式，可选值：`.gz`（默认）/ `.xz` / `.zip` / `.zst` / `.7z` |
| SELECT_PACKITPATH      | openwrt_packit         | 设置 `/opt` 下的打包目录名称                     |
| SELECT_OUTPUTPATH      | output                 | 设置 `${SELECT_PACKITPATH}` 目录中固件输出的目录名称 |
| SAVE_OPENWRT_ROOTFS    | true                   | 设置打包完成后是否保存 `*-rootfs.tar.gz` 文件       |
| SCRIPT_100ASKDSHANPIA1 | mk_rk3576_100ask-dshanpi-a1.sh | 设置打包 `rk3576 100ask-dshanpi-a1` 的脚本文件名 |
| SCRIPT_VPLUS           | mk_h6_vplus.sh         | 设置打包 `h6 vplus` 的脚本文件名                 |
| SCRIPT_CM3             | mk_rk3566_radxa-cm3-rpi-cm4-io.sh | 设置打包 `rk3566 radxa-cm3-rpi-cm4-io` 的脚本文件名 |
| SCRIPT_HT2             | mk_rk3528_ht2.sh       | 设置打包 `rk3528 ht2` 的脚本文件名               |
| SCRIPT_E20C            | mk_rk3528_e20c.sh      | 设置打包 `rk3528 e20c` 的脚本文件名              |
| SCRIPT_E24C            | mk_rk3528_e24c.sh      | 设置打包 `rk3528 e24c` 的脚本文件名              |
| SCRIPT_H28K            | mk_rk3528_h28k.sh      | 设置打包 `rk3528 h28k` 的脚本文件名              |
| SCRIPT_H66K            | mk_rk3568_h66k.sh      | 设置打包 `rk3568 h66k` 的脚本文件名              |
| SCRIPT_H68K            | mk_rk3568_h68k.sh      | 设置打包 `rk3568 h68k` 的脚本文件名              |
| SCRIPT_H69K            | mk_rk3568_h69k.sh      | 设置打包 `rk3568 h69k` 的脚本文件名              |
| SCRIPT_H88K            | mk_rk3588_h88k.sh      | 设置打包 `rk3588 h88k/ak88` 的脚本文件名         |
| SCRIPT_H88KV3          | mk_rk3588_h88k-v3.sh   | 设置打包 `rk3588 h88k-v3` 的脚本文件名           |
| SCRIPT_JPTVBOX         | mk_rk3566_jp-tvbox.sh  | 设置打包 `rk3566 jp-tvbox` 的脚本文件名          |
| SCRIPT_BEIKEYUN        | mk_rk3328_beikeyun.sh  | 设置打包 `rk3328 beikeyun` 的脚本文件名          |
| SCRIPT_L1PRO           | mk_rk3328_l1pro.sh     | 设置打包 `rk3328 l1pro` 的脚本文件名             |
| SCRIPT_ZCUBE1MAX       | mk_rk3399_zcube1-max.sh | 设置打包 `rk3399 zcube1-max` 的脚本文件名       |
| SCRIPT_ROCK5B          | mk_rk3588_rock5b.sh    | 设置打包 `rk3588 rock5b` 的脚本文件名            |
| SCRIPT_ROCK5C          | mk_rk3588s_rock5c.sh   | 设置打包 `rk3588s rock5c` 的脚本文件名           |
| SCRIPT_E52C            | mk_rk3588s_e52c.sh     | 设置打包 `rk3588s e52c` 的脚本文件名             |
| SCRIPT_E54C            | mk_rk3588s_e54c.sh     | 设置打包 `rk3588s e54c` 的脚本文件名             |
| SCRIPT_R66S            | mk_rk3568_r66s.sh      | 设置打包 `rk3568 r66s` 的脚本文件名              |
| SCRIPT_R68S            | mk_rk3568_r68s.sh      | 设置打包 `rk3568 r68s` 的脚本文件名              |
| SCRIPT_E25             | mk_rk3568_e25.sh       | 设置打包 `rk3568 e25` 的脚本文件名               |
| SCRIPT_PHOTONICAT      | mk_rk3568_photonicat.sh | 设置打包 `rk3568 photonicat` 的脚本文件名       |
| SCRIPT_RS6PRO          | mk_rk3528_rs6pro.sh    | 设置打包 `rk3528 yixun-rs6pro` 的脚本文件名      |
| SCRIPT_WATERMELONPI    | mk_rk3568_watermelon-pi.sh | 设置打包 `rk3568 watermelon-pi` 的脚本文件名 |
| SCRIPT_S905            | mk_s905_mxqpro+.sh     | 设置打包 `s905 mxqpro+` 的脚本文件名             |
| SCRIPT_S905D           | mk_s905d_n1.sh         | 设置打包 `s905d n1` 的脚本文件名                 |
| SCRIPT_S905X2          | mk_s905x2_x96max.sh    | 设置打包 `s905x2 x96max` 的脚本文件名            |
| SCRIPT_S905X3          | mk_s905x3_multi.sh     | 设置打包 `s905x3 multi` 的脚本文件名             |
| SCRIPT_S912            | mk_s912_zyxq.sh        | 设置打包 `s912 zyxq` 的脚本文件名                |
| SCRIPT_S922X           | mk_s922x_gtking.sh     | 设置打包 `s922x gtking` 的脚本文件名             |
| SCRIPT_S922X_N2        | mk_s922x_odroid-n2.sh  | 设置打包 `s922x odroid-n2` 的脚本文件名          |
| SCRIPT_QEMU            | mk_qemu-aarch64_img.sh | 设置打包 `qemu` 的脚本文件名                     |
| SCRIPT_DIY             | mk_diy.sh              | 设置打包 `diy` 自定义脚本文件名                   |
| SCRIPT_DIY_PATH        | 无                     | 设置 `SCRIPT_DIY` 文件的来源路径。可使用网络地址（如 `https://weburl/mydiyfile`）或仓库中的相对路径（如 `script/mk_s905w.sh`） |
| CUSTOMIZE_RK3399       | 无                     | 设置自定义 rk3399 设备列表，格式为 `board1:dtb1/board2:dtb2`。设为 `none` 则忽略该选项。 |
| WHOAMI                 | flippy                 | 设置 `make.env` 中 `WHOAMI` 参数的值            |
| OPENWRT_VER            | auto                   | 设置 `make.env` 中 `OPENWRT_VER` 参数的值。默认 `auto` 将自动继承文件中的赋值，设为其他值时将覆盖为自定义参数。 |
| SW_FLOWOFFLOAD         | 1                      | 设置 `make.env` 中 `SW_FLOWOFFLOAD` 参数的值    |
| HW_FLOWOFFLOAD         | 0                      | 设置 `make.env` 中 `HW_FLOWOFFLOAD` 参数的值    |
| SFE_FLOW               | 1                      | 设置 `make.env` 中 `SFE_FLOW` 参数的值          |
| ENABLE_WIFI_K504       | 1                      | 设置 `make.env` 中 `ENABLE_WIFI_K504` 参数的值  |
| ENABLE_WIFI_K510       | 1                      | 设置 `make.env` 中 `ENABLE_WIFI_K510` 参数的值  |
| DISTRIB_REVISION       | R$(date +%m.%d)        | 设置 `make.env` 中 `DISTRIB_REVISION` 参数的值  |
| DISTRIB_DESCRIPTION    | OpenWrt                | 设置 `make.env` 中 `DISTRIB_DESCRIPTION` 参数的值  |

💡 通常使用默认参数即可，也可根据实际需要进行自定义配置。例如，当 Flippy 重命名了打包脚本导致默认脚本文件无法匹配，或 make.env 中的固件版本号未同步更新时，可通过可选参数进行实时指定和个性化配置。

## 输出参数说明

按照 GitHub Actions 的标准规范，输出了以下 3 个环境变量，供后续编译步骤使用。由于 GitHub 近期调整了 Fork 仓库的默认设置，Workflow 的读写权限默认处于关闭状态，因此上传到 `Releases` 前需要手动开启 `Workflow 读写权限`，详见[使用说明](https://github.com/ophub/amlogic-s9xxx-openwrt/blob/main/documents/README.cn.md#3-fork-仓库并设置工作流权限)。

| 参数                            | 默认值                      | 说明                 |
|--------------------------------|----------------------------|----------------------|
| ${{ env.PACKAGED_OUTPUTPATH }} | /opt/openwrt_packit/output | 打包输出路径           |
| ${{ env.PACKAGED_OUTPUTDATE }} | 07.15.1058                 | 打包日期              |
| ${{ env.PACKAGED_STATUS }}     | success / failure          | 打包状态：成功 / 失败   |

## 链接

- [OpenWrt](https://github.com/openwrt/openwrt)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [immortalwrt](https://github.com/immortalwrt/immortalwrt)
- [unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit)
- [breakingbadboy/OpenWrt](https://github.com/breakingbadboy/OpenWrt)

## License

The flippy-openwrt-actions © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/flippy-openwrt-actions/blob/main/LICENSE)

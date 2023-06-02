# 功能说明

查看英文说明 | [View English description](README.md)

[unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit) 是 `Flippy` 开发的 OpenWrt 打包脚本仓库。支持全志（微加云）、瑞芯微（贝壳云，我家云，电犀牛R66S/R68S，恒领H88K/H66K/H68K/H69K，瑞莎5B/E25），以及晶晨 S9xxx 系列型号如 S905x3、S905x2、S922x、S905x、S905d，S905，S912 等设备。

此 Actions 使用他的打包脚本，未做任何修改，仅进行了智能化 Action 应用开发，让使用 github Actions 打包时变得更加简单化和个性化。

## 使用方法

在 `.github/workflows/*.yml` 云编译脚本中引入此 Actions 即可使用，例如 [packaging-openwrt.yml](.github/workflows/packaging-openwrt.yml)。代码如下：

```yaml
- name: Package OpenWrt Firmware
  uses: ophub/flippy-openwrt-actions@main
  env:
    OPENWRT_ARMVIRT: openwrt/bin/targets/*/*/*.tar.gz
    PACKAGE_SOC: all
    KERNEL_VERSION_NAME: 6.1.1_5.15.1
    KERNEL_AUTO_LATEST: true
    GH_TOKEN: ${{ secrets.GH_TOKEN }}
```

## 可选参数说明

根据 `Flippy` 最新发布的内核打包脚本，对 `打包文件`、`make.env`、`选择内核版本`、`选择盒子SoC` 等进行了可选参数配置。

| 参数                   | 默认值                  | 说明                                            |
|------------------------|------------------------|------------------------------------------------|
| OPENWRT_ARMVIRT        | 无                     | 必选项. 设置 `openwrt-armvirt-64-default-rootfs.tar.gz` 的文件路径，可以使用相对路径如 `openwrt/bin/targets/*/*/*.tar.gz` 或 网络文件下载地址如 `https://github.com/*/releases/*/*.tar.gz` |
| SCRIPT_REPO_URL        | unifreq/openwrt_packit | 设置打包脚本源码仓库的 `<owner>/<repo>` |
| SCRIPT_REPO_BRANCH     | master                 | 设置打包脚本源码仓库的分支                        |
| KERNEL_REPO_URL        | breakings/OpenWrt      | 设置内核下载仓库的 `<owner>/<repo>`，默认从 breakings 维护的[内核 Releases](https://github.com/breakings/OpenWrt/releases/tag/kernel_stable)里下载。 |
| KERNEL_VERSION_NAME    | 6.1.1_5.15.1           | 设置[内核版本](https://github.com/breakings/OpenWrt/releases/tag/kernel_stable)，可以查看并选择指定。可指定单个内核如 `6.1.1` ，可选择多个内核用`_`连接如 `6.1.1_5.15.1` |
| KERNEL_AUTO_LATEST     | true                   | 设置是否自动采用同系列最新版本内核。当为 `true` 时，将自动在内核库中查找在 `KERNEL_VERSION_NAME` 中指定的内核如 `6.1.1` 的同系列是否有更新的版本，如有更新版本时，将自动更换为最新版。设置为 `false` 时将编译指定版本内核。 |
| PACKAGE_SOC            | all                    | 设置打包盒子的 `SOC` ，默认 `all` 打包全部盒子，可指定单个盒子如 `s905x3` ，可选择多个盒子用`_`连接如 `s905x3_s905d` 。各盒子的SoC代码为：`vplus`, `cm3`, `beikeyun`, `l1pro`, `rock5b`, `h88k`, `ak88`, `r66s`, `r68s`, `h66k`, `h68k`, `h69k`, `e25`, `photonicat`, `s905`, `s905d`, `s905x2`, `s905x3`, `s912`, `s922x`, `s922x-n2`, `qemu`, `diy`。说明：`s922x-n2` 是 `s922x-odroid-n2`, `diy` 是自定义盒子。 |
| GZIP_IMGS              | auto                   | 设置打包完毕后文件压缩的格式，可选值 `.gz`（默认） / `.xz` / `.zip` / `.zst` / `.7z` |
| SELECT_PACKITPATH      | openwrt_packit         | 设置 `/opt` 下的打包目录名称                     |
| SELECT_OUTPUTPATH      | output                 | 设置 `${SELECT_PACKITPATH}` 目录中固件输出的目录名称 |
| SCRIPT_VPLUS           | mk_h6_vplus.sh         | 设置打包 `h6 vplus` 的脚本文件名                 |
| SCRIPT_CM3             | mk_rk3566_radxa-cm3-rpi-cm4-io.sh | 设置打包 `rk3566 radxa-cm3-rpi-cm4-io` 的脚本文件名 |
| SCRIPT_BEIKEYUN        | mk_rk3328_beikeyun.sh  | 设置打包 `rk3328 beikeyun` 的脚本文件名          |
| SCRIPT_L1PRO           | mk_rk3328_l1pro.sh     | 设置打包 `rk3328 l1pro` 的脚本文件名             |
| SCRIPT_ROCK5B          | mk_rk3588_rock5b.sh    | 设置打包 `rk3588 rock5b` 的脚本文件名            |
| SCRIPT_H88K            | mk_rk3588_h88k.sh      | 设置打包 `rk3588 h88k/ak88` 的脚本文件名         |
| SCRIPT_R66S            | mk_rk3568_r66s.sh      | 设置打包 `rk3568 r66s` 的脚本文件名              |
| SCRIPT_R68S            | mk_rk3568_r68s.sh      | 设置打包 `rk3568 r68s` 的脚本文件名              |
| SCRIPT_H66K            | mk_rk3568_h66k.sh      | 设置打包 `rk3568 h66k` 的脚本文件名              |
| SCRIPT_H68K            | mk_rk3568_h68k.sh      | 设置打包 `rk3568 h68k` 的脚本文件名              |
| SCRIPT_H69K            | mk_rk3568_h69k.sh      | 设置打包 `rk3568 h69k` 的脚本文件名              |
| SCRIPT_E25             | mk_rk3568_e25.sh       | 设置打包 `rk3568 e25` 的脚本文件名               |
| SCRIPT_PHOTONICAT      | mk_rk3568_photonicat.sh  | 设置打包 `rk3568 photonicat` 的脚本文件名      |
| SCRIPT_S905            | mk_s905_mxqpro+.sh     | 设置打包 `s905 mxqpro+` 的脚本文件名             |
| SCRIPT_S905D           | mk_s905d_n1.sh         | 设置打包 `s905d n1` 的脚本文件名                 |
| SCRIPT_S905X2          | mk_s905x2_x96max.sh    | 设置打包 `s905x2 x96max` 的脚本文件名            |
| SCRIPT_S905X3          | mk_s905x3_multi.sh     | 设置打包 `s905x3 multi` 的脚本文件名             |
| SCRIPT_S912            | mk_s912_zyxq.sh        | 设置打包 `s912 zyxq` 的脚本文件名                |
| SCRIPT_S922X           | mk_s922x_gtking.sh     | 设置打包 `s922x gtking` 的脚本文件名             |
| SCRIPT_S922X_N2        | mk_s922x_odroid-n2.sh  | 设置打包 `s922x odroid-n2` 的脚本文件名          |
| SCRIPT_QEMU            | mk_qemu-aarch64_img.sh | 设置打包 `qemu` 的脚本文件名                     |
| SCRIPT_DIY             | mk_diy.sh              | 设置打包 `diy` 自定义脚本文件名                   |
| SCRIPT_DIY_PATH        | 无                     | 设置 `SCRIPT_DIY` 文件的来源路径。可以使用网址如 `https://weburl/mydiyfile` 或你仓库中的相对路径如 `script/mk_s905w.sh` |
| WHOAMI                 | flippy                 | 设置 `make.env` 中 `WHOAMI` 参数的值            |
| OPENWRT_VER            | auto                   | 设置 `make.env` 中 `OPENWRT_VER` 参数的值。默认 `auto` 将自动继承文件中的赋值，设置为其他参数时将替换为自定义参数。 |
| SW_FLOWOFFLOAD         | 1                      | 设置 `make.env` 中 `SW_FLOWOFFLOAD` 参数的值    |
| HW_FLOWOFFLOAD         | 0                      | 设置 `make.env` 中 `HW_FLOWOFFLOAD` 参数的值    |
| SFE_FLOW               | 1                      | 设置 `make.env` 中 `SFE_FLOW` 参数的值          |
| ENABLE_WIFI_K504       | 1                      | 设置 `make.env` 中 `ENABLE_WIFI_K504` 参数的值  |
| ENABLE_WIFI_K510       | 1                      | 设置 `make.env` 中 `ENABLE_WIFI_K510` 参数的值  |
| DISTRIB_REVISION       | R$(date +%m.%d)        | 设置 `make.env` 中 `DISTRIB_REVISION` 参数的值  |
| DISTRIB_DESCRIPTION    | OpenWrt                | 设置 `make.env` 中 `DISTRIB_DESCRIPTION` 参数的值  |
| GH_TOKEN               | 无                     | 可选项。设置 ${{ secrets.GH_TOKEN }}，用于 [api.github.com](https://docs.github.com/en/rest/overview/resources-in-the-rest-api?apiVersion=2022-11-28#requests-from-personal-accounts) 查询。 |

💡 一般情况下使用默认参数即可，你也可以根据需要进行配置。例如在 Flippy 把打包脚本重命名后导致无法找到原默认脚本文件、make.env 中的固件版本号未更新等情况下，你可以使用可选参数进行实时指定及个性化配置。

## 输出参数说明

根据 github.com 的标准输出了 3 个环境变量，方便编译步骤后续使用。由于 github.com 最近修改了 fork 仓库的设置，默认关闭了 Workflow 的读写权限，所以上传到 `Releases` 需要给仓库添加 `${{ secrets.GITHUB_TOKEN }}` 和 `${{ secrets.GH_TOKEN }}` 并设置 `Workflow 读写权限`，详见[使用说明](https://github.com/ophub/amlogic-s9xxx-openwrt/blob/main/make-openwrt/documents/README.cn.md#2-设置隐私变量-github_token)。

| 参数                            | 默认值                      | 说明                       |
|--------------------------------|----------------------------|----------------------------|
| ${{ env.PACKAGED_OUTPUTPATH }} | /opt/openwrt_packit/output | 打包后的固件所在文件夹的路径    |
| ${{ env.PACKAGED_OUTPUTDATE }} | 07.15.1058                 | 打包日期                     |
| ${{ env.PACKAGED_STATUS }}     | success / failure          | 打包状态。成功 / 失败          |

## 链接

- [OpenWrt](https://github.com/openwrt/openwrt)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [immortalwrt](https://github.com/immortalwrt/immortalwrt)
- [unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit)
- [breakings/OpenWrt](https://github.com/breakings/OpenWrt)

## License

The flippy-openwrt-actions © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/flippy-openwrt-actions/blob/main/LICENSE)

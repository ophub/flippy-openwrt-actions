# Flippy 的 OpenWrt 打包脚本 Actions

查看英文说明 | [View English description](README.md)

完全使用 [Flippy 原站](https://github.com/unifreq/openwrt_packit) 的打包脚本，不做任何脚本修改，仅进行了智能化 Action 应用开发，让打包操作变得更加简单化和个性化。

支持 贝壳云、我家云、微加云，以及 Amlogic S9xxx 系列型号如 S905x3、S905x2、S922x、S905x、S905d，S905，S912 等。

## 使用方法

在 `.github/workflows/*.yml` 云编译脚本中引入此 Actions 即可使用，例如 [build-openwrt-armvirt.yml](https://github.com/ophub/op/blob/main/.github/workflows/build-openwrt-armvirt.yml#L144) ，代码如下：

```yaml

- name: Package Armvirt as OpenWrt
  uses: ophub/flippy-openwrt-actions@main
  env:
    OPENWRT_ARMVIRT: openwrt/bin/targets/*/*/*.tar.gz
    PACKAGE_SOC: s905d_s905x3_beikeyun
    KERNEL_VERSION_NAME: 5.15.25_5.4.180

```

## 可选参数说明

根据 `Flippy` 最新发布的内核打包脚本，对 `打包文件`、`make.env`、`选择内核版本`、`选择盒子SoC` 等进行了可选参数配置。

| 参数                   | 默认值                  | 说明                                            |
|------------------------|------------------------|------------------------------------------------|
| OPENWRT_ARMVIRT_PATH   | no                     | 必选项. 设置 `openwrt-armvirt-64-default-rootfs.tar.gz` 的文件路径，可以使用相对路径如 `openwrt/bin/targets/*/*/*.tar.gz` 或 网络文件下载地址如 `https://github.com/*/releases/*/openwrt-armvirt-64-default-rootfs.tar.gz` |
| SCRIPT_REPO_URL        | [unifreq/openwrt_packit](https://github.com/ophub/flippy-openwrt-actions/blob/main/openwrt_flippy.sh#L24) | 设置打包脚本源码仓库。可以填写 `github` 的完整网址如 `https://github.com/unifreq/openwrt_packit` 或 仓库/项目 简写如 `unifreq/openwrt_packit` |
| SCRIPT_REPO_BRANCH     | master                 | 设置打包脚本源码仓库的分支                        |
| KERNEL_REPO_URL        | [breakings/.../kernel](https://github.com/ophub/flippy-openwrt-actions/blob/main/openwrt_flippy.sh#L26) | 设置内核下载地址，默认从 breakings 维护的 [kernel](https://github.com/breakings/OpenWrt/tree/main/opt/kernel) 库里下载 Flippy 的内核。 |
| KERNEL_VERSION_NAME    | 5.15.25_5.4.180        | 设置内核版本，[kernel](https://github.com/breakings/OpenWrt/tree/main/opt/kernel) 库里收藏了众多 Flippy 的原版内核，可以查看并选择指定。可指定单个内核如 `5.4.180` ，可选择多个内核用`_`连接如 `5.15.25_5.4.180` ，内核名称以 kernel 目录中的文件夹名称为准。 |
| KERNEL_AUTO_LATEST     | true                   | 设置是否自动采用同系列最新版本内核。当为 `true` 时，将自动在内核库中查找在 `KERNEL_VERSION_NAME` 中指定的内核如 5.4.180 的 5.4 同系列是否有更新的版本，如有更新版本时，将自动更换为最新版。设置为 `false` 时将编译指定版本内核。 |
| PACKAGE_SOC            | s905d_s905x3_beikeyun  | 设置打包盒子的 `SOC` ，默认 `all` 打包全部盒子，可指定单个盒子如 `s905x3` ，可选择多个盒子用`_`连接如 `s905x3_s905d` 。各盒子的SoC代码为：`vplus` `beikeyun` `l1pro` `s905` `s905d` `s905x2` `s905x3` `s912` `s922x` `s922x-n2` `diy`。说明：`s922x-n2` 是 `s922x-odroid-n2`, `diy` 是自定义盒子。 |
| GZIP_IMGS              | auto                   | 设置打包完毕后文件压缩的格式，可选值 `.gz`（默认） / `.xz` / `.zip` / `.zst` / `.7z` |
| SELECT_PACKITPATH      | openwrt_packit         | 设置 `/opt` 下的打包目录名称                     |
| SELECT_OUTPUTPATH      | output                 | 设置 `${SELECT_PACKITPATH}` 目录中固件输出的目录名称 |
| SCRIPT_VPLUS           | mk_h6_vplus.sh         | 设置打包 `h6 vplus` 的脚本文件名                 |
| SCRIPT_BEIKEYUN        | mk_rk3328_beikeyun.sh  | 设置打包 `rk3328 beikeyun` 的脚本文件名          |
| SCRIPT_L1PRO           | mk_rk3328_l1pro.sh     | 设置打包 `rk3328 l1pro` 的脚本文件名             |
| SCRIPT_S905            | mk_s905_mxqpro+.sh     | 设置打包 `s905 mxqpro+` 的脚本文件名             |
| SCRIPT_S905D           | mk_s905d_n1.sh         | 设置打包 `s905d n1` 的脚本文件名                 |
| SCRIPT_S905X2          | mk_s905x2_x96max.sh    | 设置打包 `s905x2 x96max` 的脚本文件名            |
| SCRIPT_S905X3          | mk_s905x3_multi.sh     | 设置打包 `s905x3 multi` 的脚本文件名             |
| SCRIPT_S912            | mk_s912_zyxq.sh        | 设置打包 `s912 zyxq` 的脚本文件名                |
| SCRIPT_S922X           | mk_s922x_gtking.sh     | 设置打包 `s922x gtking` 的脚本文件名             |
| SCRIPT_S922X_N2        | mk_s922x_odroid-n2.sh  | 设置打包 `s922x odroid-n2` 的脚本文件名          |
| SCRIPT_DIY             | mk_diy.sh              | 设置打包 `diy` 自定义脚本文件名                  |
| WHOAMI                 | flippy                 | 设置 `make.env` 中 `WHOAMI` 参数的值            |
| OPENWRT_VER            | auto                   | 设置 `make.env` 中 `OPENWRT_VER` 参数的值。默认 `auto` 将自动继承文件中的赋值，设置为其他参数时将替换为自定义参数。 |
| SW_FLOWOFFLOAD         | 1                      | 设置 `make.env` 中 `SW_FLOWOFFLOAD` 参数的值    |
| HW_FLOWOFFLOAD         | 0                      | 设置 `make.env` 中 `HW_FLOWOFFLOAD` 参数的值    |
| SFE_FLOW               | 1                      | 设置 `make.env` 中 `SFE_FLOW` 参数的值    |
| ENABLE_WIFI_K504       | 1                      | 设置 `make.env` 中 `ENABLE_WIFI_K504` 参数的值  |
| ENABLE_WIFI_K510       | 1                      | 设置 `make.env` 中 `ENABLE_WIFI_K510` 参数的值  |

💡 一般情况下使用默认参数即可，你也可以根据需要进行配置。例如在 Flippy 把打包脚本重命名后导致无法找到原默认脚本文件、make.env 中的固件版本号未更新等情况下，你可以使用可选参数进行实时指定及个性化配置。

## 输出参数说明

根据 github.com 的标准输出了 3 个环境变量，方便编译步骤后续使用。

| 参数                            | 默认值                      | 说明                       |
|--------------------------------|----------------------------|----------------------------|
| ${{ env.PACKAGED_OUTPUTPATH }} | /opt/openwrt_packit/output | 打包后的固件所在文件夹的路径    |
| ${{ env.PACKAGED_OUTPUTDATE }} | 2021.07.15.1058            | 打包日期                     |
| ${{ env.PACKAGED_STATUS }}     | success / failure          | 打包状态。成功 / 失败          |

## OpenWrt 固件个性化定制说明

此 `Actions` 仅提供 OpenWrt 打包服务，你需要自己编译 `openwrt-armvirt-64-default-rootfs.tar.gz` ，编译方法可以查看 [armvirt_64](https://github.com/ophub/op/tree/main/router/armvirt_64)

## 鸣谢

- [OpenWrt](https://github.com/openwrt/openwrt)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [Lienol/openwrt](https://github.com/Lienol/openwrt)
- [unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit)
- [breakings/kernel](https://github.com/breakings/OpenWrt/tree/main/opt/kernel)

## License

The flippy-openwrt-actions © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/flippy-openwrt-actions/blob/main/LICENSE)

# Flippy 的 OpenWrt 打包脚本 Actions

查看英文说明 | [View English description](README.md)

完全使用 [Flippy 原站](https://github.com/unifreq/openwrt_packit) 的打包脚本，不做任何脚本修改，仅进行了智能化 Action 应用开发，让打包操作变得更加简单化和个性化。

支持 贝壳云、我家云、微加云，以及 Amlogic S9xxx 系列型号如 S905x3、S905x2、S922x、S905x、S905d，S905，S912 等。

## 使用方法

在 `.github/workflows/*.yml` 云编译脚本中引入此 Actions 即可使用，例如 [build-openwrt-armvirt.yml](https://github.com/ophub/op/blob/main/.github/workflows/build-openwrt-armvirt.yml) ，代码如下：

```yaml

- name: Package Armvirt as OpenWrt
  uses: ophub/flippy-openwrt-actions@main
  env:
    OPENWRT_ARMVIRT: openwrt/bin/targets/*/*/*.tar.gz
    PACKAGE_SOC: s905d_s905x3_beikeyun
    KERNEL_VERSION_NAME: 5.13.2_5.4.132
    OPENWRT_VER: R21.7.15

```

## 可选参数说明

根据 `Flippy` 最新发布的内核打包脚本，对 `打包文件`、`make.env`、`选择内核版本`、`选择盒子SoC` 等进行了可选参数配置。

| 参数                   | 默认值                  | 说明                                            |
|------------------------|------------------------|------------------------------------------------|
| OPENWRT_ARMVIRT_PATH   | no                     | 必选项. 设置 `openwrt-armvirt-64-default-rootfs.tar.gz` 的文件路径，可以使用相对路径如 `openwrt/bin/targets/*/*/*.tar.gz` 或 网络文件下载地址如 `https://github.com/*/releases/*/openwrt-armvirt-64-default-rootfs.tar.gz` |
| SCRIPT_REPO_URL        | [unifreq/openwrt_packit](https://github.com/ophub/flippy-openwrt-actions/blob/main/openwrt_flippy.sh#L21) | 设置打包脚本源码仓库。可以填写 `github` 的完整网址如 `https://github.com/unifreq/openwrt_packit` 或 仓库/项目 简写如 `unifreq/openwrt_packit` |
| SCRIPT_REPO_BRANCH     | master                 | 设置打包脚本源码仓库的分支                        |
| KERNEL_REPO_URL        | [ophub/*/amlogic-kernel](https://github.com/ophub/flippy-openwrt-actions/blob/main/openwrt_flippy.sh#L23) | 设置内核下载地址，默认从 ophub 的 [amlogic-kernel](https://github.com/ophub/amlogic-s9xxx-openwrt/tree/main/amlogic-s9xxx/amlogic-kernel) 库里下载 Flippy 的原版内核，你可以设置为其他网络下载地址。`svn checkout` 地址格式如 `https://github.com/ophub/amlogic-s9xxx-openwrt/trunk/amlogic-s9xxx/amlogic-kernel/kernel` |
| KERNEL_VERSION_NAME    | 5.13.2_5.4.132         | 设置内核版本，ophub 的 [amlogic-kernel](https://github.com/ophub/amlogic-s9xxx-openwrt/tree/main/amlogic-s9xxx/amlogic-kernel) 库里收藏了众多 Flippy 的原版内核，可以查看并选择指定。可指定单个内核如 `5.4.132` ，可选择多个内核用`_`连接如 `5.13.2_5.4.132` ，内核名称以 kernel 目录中的文件夹名称为准。 |
| PACKAGE_SOC            | s905d_s905x3_beikeyun  | 设置打包盒子的 `SOC` ，默认 `all` 打包全部盒子，可指定单个盒子如 `s905x3` ，可选择多个盒子用`_`连接如 `s905x3_s905d` 。各盒子的SoC代码为：`vplus` `beikeyun` `l1pro` `s905` `s905d` `s905x2` `s905x3` `s912` `s922x` |
| GZIP_IMGS              | true                   | 设置打包完毕是否自动压缩为 .img.gz 文件 (压缩包上传下载更快) |
| SCRIPT_VPLUS           | mk_h6_vplus.sh         | 设置打包 `h6 vplus` 的脚本文件名                 |
| SCRIPT_BEIKEYUN        | mk_rk3328_beikeyun.sh  | 设置打包 `rk3328 beikeyun` 的脚本文件名          |
| SCRIPT_L1PRO           | mk_rk3328_l1pro.sh     | 设置打包 `rk3328 l1pro` 的脚本文件名             |
| SCRIPT_S905            | mk_s905_mxqpro+.sh     | 设置打包 `s905 mxqpro+` 的脚本文件名             |
| SCRIPT_S905D           | mk_s905d_n1.sh         | 设置打包 `s905d n1` 的脚本文件名                 |
| SCRIPT_S905X2          | mk_s905x2_x96max.sh    | 设置打包 `s905x2 x96max` 的脚本文件名            |
| SCRIPT_S905X3          | mk_s905x3_multi.sh     | 设置打包 `s905x3 multi` 的脚本文件名             |
| SCRIPT_S912            | mk_s912_zyxq.sh        | 设置打包 `s912 zyxq` 的脚本文件名                |
| SCRIPT_S022X           | mk_s922x_gtking.sh     | 设置打包 `s922x gtking` 的脚本文件名             |
| WHOAMI                 | flippy                 | 设置 `make.env` 中 `WHOAMI` 参数的值            |
| OPENWRT_VER            | R21.6.22               | 设置 `make.env` 中 `OPENWRT_VER` 参数的值       |
| SFE_FLAG               | 0                      | 设置 `make.env` 中 `SFE_FLAG` 参数的值          |
| FLOWOFFLOAD_FLAG       | 1                      | 设置 `make.env` 中 `FLOWOFFLOAD_FLAG` 参数的值  |
| ENABLE_WIFI_K504       | 1                      | 设置 `make.env` 中 `ENABLE_WIFI_K504` 参数的值  |
| ENABLE_WIFI_K510       | 1                      | 设置 `make.env` 中 `ENABLE_WIFI_K510` 参数的值  |

💡 一般情况下使用默认参数即可，你也可以根据需要进行配置。例如在 Flippy 把打包脚本重命名后导致无法找到原默认脚本文件、make.env 中的固件版本号未更新等情况下，你可以使用可选参数进行实时指定及个性化配置。

## 输出参数说明

根据 github.com 的标准输出了 3 个环境变量，方便编译步骤后续使用。

| 参数                            | 默认值                  | 说明                       |
|--------------------------------|-------------------------|---------------------------|
| ${{ env.PACKAGED_OUTPUTPATH }} | /opt/openwrt_packit/tmp | 打包后的固件所在文件夹的路径  |
| ${{ env.PACKAGED_OUTPUTDATE }} | 2021.07.15.1058         | 打包日期                    |
| ${{ env.PACKAGED_STATUS }}     | success / failure       | 打包状态。成功 / 失败        |

## OpenWrt 固件个性化定制说明

此 `Actions` 仅提供 OpenWrt 打包服务，你需要自己编译 `openwrt-armvirt-64-default-rootfs.tar.gz` ，编译方法可以查看 [armvirt_64](https://github.com/ophub/op/tree/main/router/armvirt_64)

## 鸣谢

- [OpenWrt](https://github.com/openwrt/openwrt)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [Lienol/openwrt](https://github.com/Lienol/openwrt)
- [unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit)

## License

[LICENSE](https://github.com/ophub/flippy-openwrt-actions/blob/main/LICENSE) © OPHUB


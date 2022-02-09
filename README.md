# Flippy's OpenWrt packaging script Actions

View Chinese description  |  [查看中文说明](README.cn.md)

The packaging scripts of the [Flippy repository](https://github.com/unifreq/openwrt_packit) are completely used, without any script modification, and only intelligent Action application development is carried out, making the packaging operation easier and more personalized.

Support vplus, beikeyun, l1pro, and Amlogic S9xxx STB are S905x3, S905x2, S922x, S905x, S905d, S905, S912.

## Instructions

Introduce this Actions in the `.github/workflows/*.yml` cloud compilation script to use, for example [build-openwrt-armvirt.yml](https://github.com/ophub/op/blob/main/.github/workflows/build-openwrt-armvirt.yml#L144) . code show as below:

```yaml

- name: Package Armvirt as OpenWrt
  uses: ophub/flippy-openwrt-actions@main
  env:
    OPENWRT_ARMVIRT: openwrt/bin/targets/*/*/*.tar.gz
    PACKAGE_SOC: s905d_s905x3_beikeyun
    KERNEL_VERSION_NAME: 5.10.80_5.4.160

```

## Optional parameter description

According to the latest kernel packaging script released by `Flippy`, optional parameter configuration is carried out on `package file`, `make.env`, `select kernel version`, `select box SoC`, etc.

| parameter              | Defaults               | Description                                                   |
|------------------------|------------------------|---------------------------------------------------------------|
| OPENWRT_ARMVIRT_PATH   | no                     | required. Set the file path of `openwrt-armvirt-64-default-rootfs.tar.gz` , you can use a relative path such as `openwrt/bin/targets/*/*/*.tar.gz` or the network file download address. E.g `https://github.com/*/releases/*/openwrt-armvirt-64-default-rootfs.tar.gz` . |
| SCRIPT_REPO_URL        | [unifreq/openwrt_packit](https://github.com/ophub/flippy-openwrt-actions/blob/main/openwrt_flippy.sh#L24) | Set up the packaging script source code repository. You can fill in the full URL of `github` such as `https://github.com/unifreq/openwrt_packit` or repository/project abbreviation such as `unifreq/openwrt_packit` |
| SCRIPT_REPO_BRANCH     | master                 | Set the branch of the packaged script source code repository. |
| KERNEL_REPO_URL        | [breakings/.../kernel](https://github.com/ophub/flippy-openwrt-actions/blob/main/openwrt_flippy.sh#L26) | Set the kernel download address, Used by default from [kernel](https://github.com/breakings/OpenWrt/tree/main/opt/kernel) maintained by breakings. |
| KERNEL_VERSION_NAME    | 5.10.80_5.4.160        | Set the kernel version，[kernel](https://github.com/breakings/OpenWrt/tree/main/opt/kernel) library contains many original kernels of `Flippy`, you can view and choose to specify. you can specify a single kernel such as `5.4.160`, you can choose multiple kernel to use `_` connection such as `5.10.80_5.4.160` . The name of the kernel is subject to the folder name in the kernel directory. |
| KERNEL_AUTO_LATEST     | true                   | Set whether to automatically adopt the latest version of the kernel of the same series. When it is `true`, it will automatically find in the kernel library whether there is an updated version of the kernel specified in `KERNEL_VERSION_NAME` such as 5.4.160. If there is the latest version of 5.4 same series, it will automatically Replace with the latest version. When set to `false`, the specified version of the kernel will be compiled. |
| PACKAGE_SOC            | s905d_s905x3_beikeyun  | Set the `SoC` of the packaging box, the default `all` packs all boxes, you can specify a single box such as `s905x3`, you can choose multiple boxes to use `_` connection such as `s905x3_s905d` . SOC code of each box is: `vplus` `beikeyun` `l1pro` `s905` `s905d` `s905x2` `s905x3` `s912` `s922x` `s922x-n2` `diy`, Note: `s922x-n2` is `s922x-odroid-n2`, `diy` is a custom box.  |
| GZIP_IMGS              | auto                   | Set the file compression format after packaging, optional values are `.gz` (default) / `.xz` / `.zip` / `.zst` / `.7z` |
| SELECT_PACKITPATH      | openwrt_packit         | Set the packit directory under `/opt` |
| SELECT_OUTPUTPATH      | output                 | Set the directory name of the firmware output in the `${SELECT_PACKITPATH}` directory. |
| SCRIPT_VPLUS           | mk_h6_vplus.sh         | Set the script file name for packaging `h6 vplus` |
| SCRIPT_BEIKEYUN        | mk_rk3328_beikeyun.sh  | Set the script file name for packaging `rk3328 beikeyun` |
| SCRIPT_L1PRO           | mk_rk3328_l1pro.sh     | Set the script file name for packaging `rk3328 l1pro` |
| SCRIPT_S905            | mk_s905_mxqpro+.sh     | Set the script file name for packaging `s905 mxqpro+` |
| SCRIPT_S905D           | mk_s905d_n1.sh         | Set the script file name for packaging `s905d n1` |
| SCRIPT_S905X2          | mk_s905x2_x96max.sh    | Set the script file name for packaging `s905x2 x96max` |
| SCRIPT_S905X3          | mk_s905x3_multi.sh     | Set the script file name for packaging `s905x3 multi` |
| SCRIPT_S912            | mk_s912_zyxq.sh        | Set the script file name for packaging `s912 zyxq` |
| SCRIPT_S922X           | mk_s922x_gtking.sh     | Set the script file name for packaging `s922x gtking` |
| SCRIPT_S922X_N2        | mk_s922x_odroid-n2.sh  | Set the script file name for packaging `s922x odroid-n2` |
| SCRIPT_DIY             | mk_diy.sh              | Set the script file name for packaging `diy` |
| WHOAMI                 | flippy                 | Set the value of the `WHOAMI` parameter in `make.env` |
| OPENWRT_VER            | auto                   | Set the value of the `OPENWRT_VER` parameter in `make.env`. The default `auto` will automatically inherit the assignment in the file, and when set to other parameters, it will be replaced with custom parameters. |
| SW_FLOWOFFLOAD         | 1                      | Set the value of the `SW_FLOWOFFLOAD` parameter in `make.env` |
| HW_FLOWOFFLOAD         | 0                      | Set the value of the `HW_FLOWOFFLOAD` parameter in `make.env` |
| SFE_FLOW               | 1                      | Set the value of the `SFE_FLOW` parameter in `make.env` |
| ENABLE_WIFI_K504       | 1                      | Set the value of the `ENABLE_WIFI_K504` parameter in `make.env` |
| ENABLE_WIFI_K510       | 0                      | Set the value of the `ENABLE_WIFI_K510` parameter in `make.env` |

💡 Normally, you can use the default parameters, but you can also configure them according to your needs. For example, after Flippy renamed the packaging script, the original default script file cannot be found, and the firmware version number in make.env has not been updated. You can use optional parameters for real-time designation and personalized configuration.

## Output parameter description

According to the standard of github.com, 3 environment variables are output to facilitate the subsequent use of the compilation step.

| parameter                                | For example                | Description                   |
|------------------------------------------|----------------------------|-------------------------------|
| ${{ env.PACKAGED_OUTPUTPATH }}           | /opt/openwrt_packit/output | OpenWrt firmware storage path |
| ${{ env.PACKAGED_OUTPUTDATE }}           | 2021.07.15.1058            | Packing date                  |
| ${{ env.PACKAGED_STATUS }}               | success / failure          | Package status                |

## OpenWRT firmware personalized custom description

This `Actions` only provides OpenWrt packaging services, you need to compile the `openwrt-armvirt-64-default-rootfs.tar.gz` . The compilation method can be viewed [armvirt_64](https://github.com/ophub/op/tree/main/router/armvirt_64)

## Acknowledgments

- [OpenWrt](https://github.com/openwrt/openwrt)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [Lienol/openwrt](https://github.com/Lienol/openwrt)
- [unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit)
- [breakings/kernel](https://github.com/breakings/OpenWrt/tree/main/opt/kernel)

## License

The flippy-openwrt-actions © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/flippy-openwrt-actions/blob/main/LICENSE)


# Flippy's OpenWrt packaging script Actions

View Chinese description  |  [查看中文说明](README.cn.md)

The packaging scripts of the [Flippy repository](https://github.com/unifreq/openwrt_packit) are completely used, without any script modification, and only intelligent Action application development is carried out, making the packaging operation easier and more personalized.

Support vplus, beikeyun, l1pro, and Amlogic S9xxx STB are S905x3, S905x2, S922x, S905x, S905d, S905, S912.

## Instructions

Introduce this Actions in the `.github/workflows/*.yml` cloud compilation script to use, for example [build-openwrt-armvirt.yml](https://github.com/ophub/op/blob/main/.github/workflows/build-openwrt-armvirt.yml) . code show as below:

```yaml

- name: Package Armvirt as OpenWrt
  uses: ophub/flippy-openwrt-actions@main
  env:
    OPENWRT_ARMVIRT: openwrt/bin/targets/*/*/*.tar.gz
    PACKAGE_SOC: s905d_s905x3_beikeyun
    KERNEL_VERSION_NAME: 5.10.31.TF_5.4.113

```

## Optional parameter description

According to the latest kernel packaging script released by `Flippy`, optional parameter configuration is carried out on `package file`, `make.env`, `select kernel version`, `select box SoC`, etc.

| parameter              | Defaults               | Description                                                   |
|------------------------|------------------------|---------------------------------------------------------------|
| OPENWRT_ARMVIRT_PATH   | no                     | required. Set the file path of `openwrt-armvirt-64-default-rootfs.tar.gz` , you can use a relative path such as `openwrt/bin/targets/*/*/*.tar.gz` or the network file download address. E.g `https://github.com/*/releases/*/openwrt-armvirt-64-default-rootfs.tar.gz` . |
| SCRIPT_REPO_URL        | [unifreq/openwrt_packit](https://github.com/ophub/flippy-openwrt-actions/blob/main/openwrt_flippy.sh#L21) | Set up the packaging script source code repository. You can fill in the full URL of `github` such as `https://github.com/unifreq/openwrt_packit` or repository/project abbreviation such as `unifreq/openwrt_packit` |
| SCRIPT_REPO_BRANCH     | master                 | Set the branch of the packaged script source code repository. |
| KERNEL_REPO_URL        | [ophub/*/kernel](https://github.com/ophub/flippy-openwrt-actions/blob/main/openwrt_flippy.sh#L23) | Set the kernel download address, Used by default from [kernel](https://github.com/ophub/amlogic-s9xxx-openwrt/tree/main/amlogic-s9xxx/amlogic-kernel/kernel) , You can set it as other network download address. `svn checkout` The address format is like `https://github.com/ophub/amlogic-s9xxx-openwrt/trunk/amlogic-s9xxx/amlogic-kernel/kernel` |
| KERNEL_VERSION_NAME    | 5.4.108_5.10.26.TF     | Set the kernel version，Ophub's [kernel](https://github.com/ophub/amlogic-s9xxx-openwrt/tree/main/amlogic-s9xxx/amlogic-kernel/kernel) library contains many original kernels of `Flippy`, you can view and choose to specify. |
| PACKAGE_SOC            | s905d_s905x3_beikeyun  | Set the `SoC` of the packaging box, the default `all` packs all boxes, you can specify a single box such as `s905x3`, you can choose multiple boxes to use _ connection such as `s905x3_s905d` . SOC code of each box is: `vplus` `beikeyun` `l1pro` `s905` `s905d` `s905x2` `s905x3` `s912` `s922x` |
| GZIP_IMGS_VALUE        | true                   | Set whether to automatically compress to .img.gz file after packaging (compression package upload and download faster) |
| SCRIPT_VPLUS_FILE      | mk_h6_vplus.sh         | Set the script file name for packaging `h6 vplus` |
| SCRIPT_BEIKEYUN_FILE   | mk_rk3328_beikeyun.sh  | Set the script file name for packaging `rk3328 beikeyun` |
| SCRIPT_L1PRO_FILE      | mk_rk3328_l1pro.sh     | Set the script file name for packaging `rk3328 l1pro` |
| SCRIPT_S905_FILE       | mk_s905_mxqpro+.sh     | Set the script file name for packaging `s905 mxqpro+` |
| SCRIPT_S905D_FILE      | mk_s905d_n1.sh         | Set the script file name for packaging `s905d n1` |
| SCRIPT_S905X2_FILE     | mk_s905x2_x96max.sh    | Set the script file name for packaging `s905x2 x96max` |
| SCRIPT_S905X3_FILE     | mk_s905x3_multi.sh     | Set the script file name for packaging `s905x3 multi` |
| SCRIPT_S912_FILE       | mk_s912_zyxq.sh        | Set the script file name for packaging `s912 zyxq` |
| SCRIPT_S022X_FILE      | mk_s922x_gtking.sh     | Set the script file name for packaging `s922x gtking` |
| WHOAMI_VALUE           | flippy                 | Set the value of the `WHOAMI` parameter in `make.env` |
| OPENWRT_VER_VALUE      | R21.4.18               | Set the value of the `OPENWRT_VER_VALUE` parameter in `make.env` |
| SFE_FLAG_VALUE         | 0                      | Set the value of the `SFE_FLAG_VALUE` parameter in `make.env` |
| FLOWOFFLOAD_FLAG_VALUE | 1                      | Set the value of the `FLOWOFFLOAD_FLAG_VALUE` parameter in `make.env` |

💡 Normally, you can use the default parameters, but you can also configure them according to your needs. For example, after Flippy renamed the packaging script, the original default script file cannot be found, and the firmware version number in make.env has not been updated. You can use optional parameters for real-time designation and personalized configuration.

## Output parameter description

According to the standard of github.com, 3 environment variables are output to facilitate the subsequent use of the compilation step.

| parameter                                | For example             | Description                   |
|------------------------------------------|-------------------------|-------------------------------|
| ${{ env.PACKAGED_OUTPUTPATH }}           | /opt/openwrt_packit/tmp | OpenWrt firmware storage path |
| ${{ env.PACKAGED_OUTPUTDATE }}           | 2021.04.21.1058         | Packing date                  |
| ${{ env.PACKAGED_STATUS }}               | success / failure       | Package status                |

## OpenWRT firmware personalized custom description

This `Actions` only provides OpenWrt packaging services, you need to compile the `openwrt-armvirt-64-default-rootfs.tar.gz` . The compilation method can be viewed [armvirt_64](https://github.com/ophub/op/tree/main/router/armvirt_64)

## Acknowledgments

- [OpenWrt](https://github.com/openwrt/openwrt)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [Lienol/openwrt](https://github.com/Lienol/openwrt)
- [unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit)
- [tuanqing/mknop](https://github.com/tuanqing/mknop)
- [P3TERX/Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt)

## License

[LICENSE](https://github.com/ophub/flippy-openwrt-actions/blob/main/LICENSE) © OPHUB


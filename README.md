# Function description

View Chinese description  |  [查看中文说明](README.cn.md)

[unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit) is a repository of OpenWrt packaging scripts developed by `Flippy`. It supports Allwinner (VPlus), Rockchip (BeikeYun, Chainedbox-L1-Pro, FastRhino-R66S/R68S, Hinlink-H88K/H66K/H68K/H69K, Radxa-5B/E25), as well as the Amlogic S9xxx series models such as S905x3, S905x2, S922x, S905x, S905d, S905, S912, and other devices.

This Actions uses his packaging script without modification, only intelligentizes Action application development, making it easier and more personalized to use github Actions for packaging.

## Usage

Simply include this Actions in the `.github/workflows/*.yml` cloud compilation script to use it, such as [packaging-openwrt.yml](.github/workflows/packaging-openwrt.yml). The code is as follows:

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

## Optional Parameter Instructions

According to `Flippy`'s latest kernel packaging script, optional parameter configurations have been made for `packaging files`, `make.env`, `selecting the kernel version`, `selecting the box SoC`, and so on.

| parameter              | Defaults               | Description                                                   |
|------------------------|------------------------|---------------------------------------------------------------|
| OPENWRT_ARMVIRT        | None                   | This is a required option. Set the file path of `openwrt-armvirt-64-default-rootfs.tar.gz`, which can use relative paths such as `openwrt/bin/targets/*/*/*.tar.gz` or network file download addresses such as `https://github.com/*/releases/*/*.tar.gz`. |
| SCRIPT_REPO_URL        | unifreq/openwrt_packit | Set `<owner>/<repo>` of the packaging script source code repository. |
| SCRIPT_REPO_BRANCH     | master                 | Set the branch of the packaging script source code repository. |
| KERNEL_REPO_URL        | breakings/OpenWrt      | Set `<owner>/<repo>` of the kernel download repository. By default, it downloads from [kernel Releases](https://github.com/breakings/OpenWrt/releases/tag/kernel_stable) maintained by breakings. |
| KERNEL_VERSION_NAME    | 6.1.1_5.15.1           | Set the [kernel version](https://github.com/breakings/OpenWrt/releases/tag/kernel_stable), which can be viewed and selected. A single kernel can be specified, such as `6.1.1`, or multiple kernels can be selected and connected using `_`, such as `6.1.1_5.15.1`. |
| KERNEL_AUTO_LATEST     | true                   | Set whether to automatically adopt the latest version of the same series kernel. When set to `true`, it will automatically search for an updated version of the same series kernel specified in `KERNEL_VERSION_NAME`, such as `6.1.1`, in the kernel repository. If there is an updated version, it will be automatically replaced with the latest version. When set to `false`, the specified version of the kernel will be compiled. |
| PACKAGE_SOC            | all                    | Set the `SOC` of the box to be packaged. By default, `all` packages all boxes. A single box can be specified, such as `s905x3`, or multiple boxes can be selected and connected using `_`, such as `s905x3_s905d`. The SoC codes for each box are: `vplus`, `cm3`, `beikeyun`, `l1pro`, `rock5b`, `h88k`, `ak88`, `r66s`, `r68s`, `h66k`, `h68k`, `h69k`, `e25`, `photonicat`, `s905`, `s905d`, `s905x2`, `s905x3`, `s912`, `s922x`, `s922x-n2`, `qemu`, `diy`. Note: `s922x-n2` is `s922x-odroid-n2`, and `diy` is a custom box. |
| GZIP_IMGS              | auto                   | Set the compression format of the packaged file after packaging is complete. Optional values are `.gz` (default) / `.xz` / `.zip` / `.zst` / `.7z`. |
| SELECT_PACKITPATH      | openwrt_packit         | Set the packaging directory name under `/opt`. |
| SELECT_OUTPUTPATH      | output                 | Set the directory name for the firmware output in the `${SELECT_PACKITPATH}` directory. |
| SCRIPT_VPLUS           | mk_h6_vplus.sh         | Set the script file name for packaging `h6 vplus`. |
| SCRIPT_CM3             | mk_rk3566_radxa-cm3-rpi-cm4-io.sh | Set the script file name for packaging `rk3566 radxa-cm3-rpi-cm4-io`. |
| SCRIPT_BEIKEYUN        | mk_rk3328_beikeyun.sh  | Set the script file name for packaging `rk3328 beikeyun`. |
| SCRIPT_L1PRO           | mk_rk3328_l1pro.sh     | Set the script file name for packaging `rk3328 l1pro`. |
| SCRIPT_ROCK5B          | mk_rk3588_rock5b.sh    | Set the script file name for packaging `rk3588 rock5b`. |
| SCRIPT_H88K            | mk_rk3588_h88k.sh      | Set the script file name for packaging `rk3588 h88k/ak88`. |
| SCRIPT_R66S            | mk_rk3568_r66s.sh      | Set the script file name for packaging `rk3568 r66s`. |
| SCRIPT_R68S            | mk_rk3568_r68s.sh      | Set the script file name for packaging `rk3568 r68s`. |
| SCRIPT_H66K            | mk_rk3568_h66k.sh      | Set the script file name for packaging `rk3568 h66k`. |
| SCRIPT_H68K            | mk_rk3568_h68k.sh      | Set the script file name for packaging `rk3568 h68k`. |
| SCRIPT_H69K            | mk_rk3568_h69k.sh      | Set the script file name for packaging `rk3568 h69k`. |
| SCRIPT_E25             | mk_rk3568_e25.sh       | Set the script file name for packaging `rk3568 e25`.  |
| SCRIPT_PHOTONICAT      | mk_rk3568_photonicat.sh  | Set the script file name for packaging `rk3568 photonicat`. |
| SCRIPT_S905            | mk_s905_mxqpro+.sh     | Set the script file name for packaging `s905 mxqpro+`. |
| SCRIPT_S905D           | mk_s905d_n1.sh         | Set the script file name for packaging `s905d n1`. |
| SCRIPT_S905X2          | mk_s905x2_x96max.sh    | Set the script file name for packaging `s905x2 x96max`. |
| SCRIPT_S905X3          | mk_s905x3_multi.sh     | Set the script file name for packaging `s905x3 multi`. |
| SCRIPT_S912            | mk_s912_zyxq.sh        | Set the script file name for packaging `s912 zyxq`. |
| SCRIPT_S922X           | mk_s922x_gtking.sh     | Set the script file name for packaging `s922x gtking`. |
| SCRIPT_S922X_N2        | mk_s922x_odroid-n2.sh  | Set the script file name for packaging `s922x odroid-n2`. |
| SCRIPT_QEMU            | mk_qemu-aarch64_img.sh | Set the script file name for packaging `qemu`. |
| SCRIPT_DIY             | mk_diy.sh              | Set the script file name for packaging `diy`. |
| SCRIPT_DIY_PATH        | None                   | Set the load path for `SCRIPT_DIY` files. You can use a URL such as `https://weburl/mydiyfile` or a relative path in your repository such as `script/mk_s905w.sh`. |
| WHOAMI                 | flippy                 | Set the value of the `WHOAMI` parameter in `make.env`. |
| OPENWRT_VER            | auto                   | Set the value of the `OPENWRT_VER` parameter in `make.env`. The default `auto` will automatically inherit the assignment in the file, and when set to other parameters, it will be replaced with custom parameters. |
| SW_FLOWOFFLOAD         | 1                      | Set the value of the `SW_FLOWOFFLOAD` parameter in `make.env`. |
| HW_FLOWOFFLOAD         | 0                      | Set the value of the `HW_FLOWOFFLOAD` parameter in `make.env`. |
| SFE_FLOW               | 1                      | Set the value of the `SFE_FLOW` parameter in `make.env`.       |
| ENABLE_WIFI_K504       | 1                      | Set the value of the `ENABLE_WIFI_K504` parameter in `make.env`. |
| ENABLE_WIFI_K510       | 1                      | Set the value of the `ENABLE_WIFI_K510` parameter in `make.env`. |
| DISTRIB_REVISION       | R$(date +%m.%d)        | Set the value of the `DISTRIB_REVISION` parameter in `make.env`. |
| DISTRIB_DESCRIPTION    | OpenWrt                | Set the value of the `DISTRIB_DESCRIPTION` parameter in `make.env`. |
| GH_TOKEN               | None                   | Optional. Set ${{ secrets.GH_TOKEN }} for [api.github.com](https://docs.github.com/en/rest/overview/resources-in-the-rest-api?apiVersion=2022-11-28#requests-from-personal-accounts) query. |

💡 In general, default parameters can be used. You can also configure them as needed. For example, if Flippy renames the packaging script and the original default script file cannot be found, or the firmware version number in make.env is not updated, you can use optional parameters to specify and personalize the configuration in real time.

## Output Parameter Instructions

According to the standard output of github.com, three environment variables are output for easy use in subsequent compilation steps. Recently, github.com has modified the settings for fork repositories and has disabled Workflow's read/write permissions by default. Therefore, uploading to `Releases` requires adding `${{ secrets.GITHUB_TOKEN }}` and `${{ secrets.GH_TOKEN }}` to the repository and setting `Workflow Read and Write Permissions`. For details, please refer to the [instructions](https://github.com/ophub/amlogic-s9xxx-openwrt/blob/main/make-openwrt/documents/README.md#2-set-the-privacy-variable-github_token).

| parameter                                | For example                | Description                   |
|------------------------------------------|----------------------------|-------------------------------|
| ${{ env.PACKAGED_OUTPUTPATH }}           | /opt/openwrt_packit/output | OpenWrt firmware storage path |
| ${{ env.PACKAGED_OUTPUTDATE }}           | 07.15.1058                 | Packing date                  |
| ${{ env.PACKAGED_STATUS }}               | success / failure          | Package status                |

## Links

- [OpenWrt](https://github.com/openwrt/openwrt)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [immortalwrt](https://github.com/immortalwrt/immortalwrt)
- [unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit)
- [breakings/OpenWrt](https://github.com/breakings/OpenWrt)

## License

The flippy-openwrt-actions © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/flippy-openwrt-actions/blob/main/LICENSE)


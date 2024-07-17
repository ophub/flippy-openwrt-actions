# Function description

View Chinese description  |  [查看中文说明](README.cn.md)

[unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit) is an OpenWrt packaging script repository developed by `Flippy`. It supports Allwinner (VPlus), Rockchip (BeikeYun, Chainedbox-L1-Pro, FastRhino-R66S/R68S, Radxa-5B/E25, Watermelon-pi, etc.), and Amlogic S9xxx series models such as S905x3, S905x2, S922x, S905x, S905d, S905, S912, etc.

This Actions uses his packaging scripts without any modification, only developed into a smart Action application, making the use of github Actions for packaging simpler and more personalized.

## Usage

This Actions can be used by referencing it in the `.github/workflows/*.yml` cloud compilation script, for example [packaging-openwrt.yml](.github/workflows/packaging-openwrt.yml). The code is as follows:

```yaml
- name: Package OpenWrt Firmware
  uses: ophub/flippy-openwrt-actions@main
  env:
    OPENWRT_ARMVIRT: openwrt/bin/targets/*/*/*.tar.gz
    PACKAGE_SOC: all
    KERNEL_VERSION_NAME: 6.1.y_6.6.y
    KERNEL_AUTO_LATEST: true
```

## Optional Parameters

Based on the latest kernel packaging scripts released by `Flippy`, optional parameter configurations have been made for `Packaging Files`, `make.env`, `Select Kernel Version`, `Select Box SoC`, and so on.

| Parameter              | Default                | Description                                                    |
|------------------------|------------------------|---------------------------------------------------------------|
| OPENWRT_ARMVIRT        | None                   | Required. Set the file path for `openwrt-armvirt-64-default-rootfs.tar.gz`. You can use a relative path such as `openwrt/bin/targets/*/*/*.tar.gz` or a web file download link such as `https://github.com/*/releases/*/*.tar.gz` |
| SCRIPT_REPO_URL        | unifreq/openwrt_packit | Set `<owner>/<repo>` of the packaging script source repository |
| SCRIPT_REPO_BRANCH     | master                 | Set the branch of the packaging script source repository      |
| KERNEL_REPO_URL        | breakings/OpenWrt      | Set `<owner>/<repo>` of the kernel download repository, it downloads from the [kernel Releases](https://github.com/breakings/OpenWrt/releases/tag/kernel_stable) maintained by breakings by default. |
| KERNEL_VERSION_NAME    | 6.1.y_6.6.y            | Set the [Kernel version](https://github.com/breakings/OpenWrt/releases/tag/kernel_stable), you can check and select a specific one. You can specify a single kernel such as `6.1.y`, or select multiple kernels connected with `_` like `6.1.y_6.6.y` |
| KERNEL_AUTO_LATEST     | true                   | Set whether to automatically adopt the latest version kernel of the same series. When set to `true`, it will automatically look for whether there is an updated version of the kernel specified in `KERNEL_VERSION_NAME`, such as `6.1.y`, in the kernel library, and if there is an updated version, it will automatically replace it with the latest version. When set to `false`, it will compile the specified version kernel. |
| PACKAGE_SOC            | all                    | Set the `SOC` of the package box, the default is `all` to package all boxes, you can specify a single box like `s905x3`, or select multiple boxes connected with `_` like `s905x3_s905d`. The SoC codes for each box are: `vplus`, `cm3`, `jp-tvbox`, `beikeyun`, `l1pro`, `rock5b`, `rock5c`, `e52c`, `r66s`, `r68s`, `e25`, `photonicat`, `watermelon-pi`, `zcube1-max`, `ht2`, `e20c`, `h28k`, `h66k`, `h68k`, `h69k`, `h69k-max`, `h88k`, `h88k-v3`, `rk3399`, `s905`, `s905d`, `s905x2`, `s905x3`, `s912`, `s922x`, `s922x-n2`, `qemu`, `diy`. Note: `s922x-n2` is `s922x-odroid-n2`, `diy` is a custom box. |
| GZIP_IMGS              | auto                   | Set the format of the file compression after packaging, optional values are `.gz` (default) / `.xz` / `.zip` / `.zst` / `.7z` |
| SELECT_PACKITPATH      | openwrt_packit         | Set the name of the packaging directory under `/opt`          |
| SELECT_OUTPUTPATH      | output                 | Set the name of the firmware output directory in the `${SELECT_PACKITPATH}` directory |
| SCRIPT_VPLUS           | mk_h6_vplus.sh         | Set the script filename for packaging `h6 vplus`              |
| SCRIPT_CM3             | mk_rk3566_radxa-cm3-rpi-cm4-io.sh | Set the script filename for packaging `rk3566 radxa-cm3-rpi-cm4-io` |
| SCRIPT_HT2             | mk_rk3528_ht2.sh       | Set the script filename for packaging `rk3528 ht2`           |
| SCRIPT_E20C            | mk_rk3528_e20c.sh      | Set the script filename for packaging `rk3528 e20c`           |
| SCRIPT_H28K            | mk_rk3528_h28k.sh      | Set the script filename for packaging `rk3528 h28k`           |
| SCRIPT_H66K            | mk_rk3568_h66k.sh      | Set the script filename for packaging `rk3568 h66k`           |
| SCRIPT_H68K            | mk_rk3568_h68k.sh      | Set the script filename for packaging `rk3568 h68k`           |
| SCRIPT_H69K            | mk_rk3568_h69k.sh      | Set the script filename for packaging `rk3568 h69k`           |
| SCRIPT_H88K            | mk_rk3588_h88k.sh      | Set the script filename for packaging `rk3588 h88k/ak88`      |
| SCRIPT_H88KV3          | mk_rk3588_h88k-v3.sh   | Set the script filename for packaging `rk3588 h88k-v3`        |
| SCRIPT_JPTVBOX         | mk_rk3566_jp-tvbox.sh  | Set the script filename for packaging `rk3566 jp-tvbox`       |
| SCRIPT_BEIKEYUN        | mk_rk3328_beikeyun.sh  | Set the script filename for packaging `rk3328 beikeyun`       |
| SCRIPT_L1PRO           | mk_rk3328_l1pro.sh     | Set the script filename for packaging `rk3328 l1pro`          |
| SCRIPT_ZCUBE1MAX       | mk_rk3399_zcube1-max.sh | Set the script filename for packaging `rk3399 zcube1-max`    |
| SCRIPT_ROCK5B          | mk_rk3588_rock5b.sh    | Set the script filename for packaging `rk3588 rock5b`         |
| SCRIPT_ROCK5C          | mk_rk3588s_rock5c.sh   | Set the script filename for packaging `rk3588s rock5c`        |
| SCRIPT_E52C            | mk_rk3588s_e52c.sh     | Set the script filename for packaging `rk3588s e52c`          |
| SCRIPT_R66S            | mk_rk3568_r66s.sh      | Set the script filename for packaging `rk3568 r66s`           |
| SCRIPT_R68S            | mk_rk3568_r68s.sh      | Set the script filename for packaging `rk3568 r68s`           |
| SCRIPT_E25             | mk_rk3568_e25.sh       | Set the script filename for packaging `rk3568 e25`            |
| SCRIPT_PHOTONICAT      | mk_rk3568_photonicat.sh       | Set the script filename for packaging `rk3568 photonicat`       |
| SCRIPT_WATERMELONPI    | mk_rk3568_watermelon-pi.sh    | Set the script filename for packaging `rk3568 watermelon-pi`    |
| SCRIPT_S905            | mk_s905_mxqpro+.sh     | Set the script filename for packaging `s905 mxqpro+`          |
| SCRIPT_S905D           | mk_s905d_n1.sh         | Set the script filename for packaging `s905d n1`              |
| SCRIPT_S905X2          | mk_s905x2_x96max.sh    | Set the script filename for packaging `s905x2 x96max`         |
| SCRIPT_S905X3          | mk_s905x3_multi.sh     | Set the script filename for packaging `s905x3 multi`          |
| SCRIPT_S912            | mk_s912_zyxq.sh        | Set the script filename for packaging `s912 zyxq`             |
| SCRIPT_S922X           | mk_s922x_gtking.sh     | Set the script filename for packaging `s922x gtking`          |
| SCRIPT_S922X_N2        | mk_s922x_odroid-n2.sh  | Set the script filename for packaging `s922x odroid-n2`       |
| SCRIPT_QEMU            | mk_qemu-aarch64_img.sh | Set the script filename for packaging `qemu`                  |
| SCRIPT_DIY             | mk_diy.sh              | Set the script filename for packaging `diy` custom            |
| SCRIPT_DIY_PATH        | None                   | Set the source path for `SCRIPT_DIY`. It can be a URL like `https://weburl/mydiyfile` or a relative path in your repository like `script/mk_s905w.sh` |
| CUSTOMIZE_RK3399       | None                   | Set custom rk3399 device list, format: `board1:dtb1/board2:dtb2` |
| WHOAMI                 | flippy                 | Set the value for `WHOAMI` in `make.env`                      |
| OPENWRT_VER            | auto                   | Set the value for `OPENWRT_VER` in `make.env`. By default, `auto` will inherit the value from the file. When set to other parameters, it will replace the original parameter with the custom parameter |
| SW_FLOWOFFLOAD         | 1                      | Set the value for `SW_FLOWOFFLOAD` in `make.env`              |
| HW_FLOWOFFLOAD         | 0                      | Set the value for `HW_FLOWOFFLOAD` in `make.env`              |
| SFE_FLOW               | 1                      | Set the value for `SFE_FLOW` in `make.env`                    |
| ENABLE_WIFI_K504       | 1                      | Set the value for `ENABLE_WIFI_K504` in `make.env`            |
| ENABLE_WIFI_K510       | 1                      | Set the value for `ENABLE_WIFI_K510` in `make.env`            |
| DISTRIB_REVISION       | R$(date +%m.%d)        | Set the value for `DISTRIB_REVISION` in `make.env`            |
| DISTRIB_DESCRIPTION    | OpenWrt                | Set the value for `DISTRIB_DESCRIPTION` in `make.env`         |

💡 In general, using the default parameters is sufficient, but you can configure them according to your needs. For instance, if Flippy renames the packaging script making the original default script file unfindable, or if the firmware version number in make.env is not updated, you can use optional parameters for real-time specification and personalized configuration.

## Output Parameters Explanation

According to the standard of github.com, 3 environment variables have been output for use in subsequent compilation steps. Since github.com recently changed the settings of fork repositories, read-write permissions for Workflow are turned off by default. Therefore, Therefore, to upload to Releases, it is necessary to `set Workflow read and write permissions`. For details, see [User Manual](https://github.com/ophub/amlogic-s9xxx-openwrt/tree/main/documents#3-fork-the-repository-and-set-workflow-permissions).

| Parameter                      | Default Value              | Description                             |
|--------------------------------|----------------------------|-----------------------------------------|
| ${{ env.PACKAGED_OUTPUTPATH }} | /opt/openwrt_packit/output | Path of the folder containing the packaged firmware |
| ${{ env.PACKAGED_OUTPUTDATE }} | 07.15.1058                 | Packaging date                          |
| ${{ env.PACKAGED_STATUS }}     | success / failure          | Packaging status. Success / Failure     |

## Links

- [OpenWrt](https://github.com/openwrt/openwrt)
- [coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
- [immortalwrt](https://github.com/immortalwrt/immortalwrt)
- [unifreq/openwrt_packit](https://github.com/unifreq/openwrt_packit)
- [breakings/OpenWrt](https://github.com/breakings/OpenWrt)

## License

The flippy-openwrt-actions © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/flippy-openwrt-actions/blob/main/LICENSE)


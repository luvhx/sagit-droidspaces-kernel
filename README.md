# 小米 6（sagit）DroidSpaces 内核

适用：**小米 6（sagit）** · **LineageOS 22.2**（Android 15）· Linux **4.4.302**

## 下载

从 [Releases](https://github.com/luvhx/sagit-droidspaces-kernel/releases/latest) 下载：

| 文件 | 用途 |
| --- | --- |
| `boot-sagit-droidspaces.img` | fastboot 直刷（推荐） |
| `DroidSpaces-sagit-4.4.302-kernel.zip` | TWRP / Horizon Kernel Flasher 卡刷（AnyKernel3） |
| `Image.gz-dtb` | 内核镜像 + DTB，自行打包用 |

SHA-256：

| 文件 | SHA-256 |
| --- | --- |
| `boot-sagit-droidspaces.img` | `c132aea514b351b612836eae6125077cde1d0464e058a947933706d56604aac1` |
| `DroidSpaces-sagit-4.4.302-kernel.zip` | `77a894381f6f96d4568881469aaebbfd0a5c8388bed939bfbc1d7a362e187904` |
| `Image.gz-dtb` | `47e521532bb8b4351368868eeef0f485883ffa89cd352858e28df59cf015d50e` |

## 使用方法

### 前置

1. 解锁 bootloader；
2. 系统为 **LineageOS 22.2**（`lineage-22.2-20261106-nightly-sagit` 或相近 nightly）；
3. 备份 `boot`、`persist` 分区，电量 > 50%。

### 方式 A：fastboot（推荐）

```bash
fastboot flash boot boot-sagit-droidspaces.img
fastboot reboot
```

> 若当前 boot 已被 Magisk 修补过，直刷会覆盖 root，刷完需重装一次 Magisk。

### 方式 B：TWRP 卡刷

1. TWRP 中刷入 `DroidSpaces-sagit-4.4.302-kernel.zip`（只替换 kernel，保留原 ramdisk）；
2. 重启，首次开机约 3–5 分钟。

### 用 Magisk 获取 root（推荐）

1. 下载安装 [Magisk](https://github.com/topjohnwu/Magisk/releases/latest)（`Magisk-v*.apk`）；
2. 打开 Magisk → **安装 → 选择并修补一个文件**，选择 `boot-sagit-droidspaces.img`，生成 `magisk_patched-*.img`；
3. 把修补后的镜像拷到电脑：

```bash
fastboot flash boot magisk_patched-*.img
fastboot reboot
```

4. 打开 Magisk 确认版本号与超级用户授权正常。

> 之后每次刷新内核（无论 fastboot 还是 AnyKernel3），都要用 Magisk 重新修补一次 boot 才能恢复 root；先刷 Magisk 再刷 AnyKernel3 zip 可保留原 ramdisk。

### 安装 DroidSpaces

1. 安装 [DroidSpaces APK](https://github.com/ravindu644/Droidspaces-OSS/releases/latest) → 授予 root；
2. 按提示安装 backend，重启；
3. 打开 **Settings → Requirements → Check Requirements**，全部通过后即可创建容器。

## 资源链接

- LineageOS 设备页：<https://download.lineageos.org/devices/sagit>
- LOS 22.2 ROM（20261106）：<https://mirrorbits.lineageos.org/full/sagit/20261106/lineage-22.2-20261106-nightly-sagit-signed.zip>
- 官方 boot.img：<https://mirrorbits.lineageos.org/full/sagit/20261106/boot.img>
- 官方 recovery.img：<https://mirrorbits.lineageos.org/full/sagit/20261106/recovery.img>
- LOS 安装教程：<https://wiki.lineageos.org/devices/sagit/install>
- TWRP：<https://dl.twrp.me/sagit/>
- Magisk：<https://github.com/topjohnwu/Magisk/releases/latest>
- DroidSpaces：<https://github.com/ravindu644/Droidspaces-OSS>
- 内核源码：<https://github.com/LineageOS/android_kernel_xiaomi_msm8998>（`lineage-22.2`）

## 说明

- 内核版本 `4.4.302-perf`；已开启命名空间（PID/UTS/IPC/NET/USER）、SYSVIPC、seccomp filter、cgroup v1（memcg/cfs/pids）、devtmpfs、overlayfs、veth/bridge/nf_tables/NAT/macvlan；关闭 `ANDROID_PARANOID_NETWORK`。
- 所有驱动内建（0 个 `=m` 模块），不存在模块 vermagic 不匹配问题。
- root 方式为 **Magisk**：SukiSU / KernelSU 要求 Linux 4.14+，sagit 的 4.4.302 不满足，故本内核不集成。
- 刷机有风险，自行承担。

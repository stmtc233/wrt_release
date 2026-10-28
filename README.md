# WRT Release - S20P 专属精简编译仓库

本项目专为 **CLX S20P (MediaTek MT7986 / Filogic 830, AX6000)** 定制，基于 [chasey-dev/immortalwrt-mt798x-rebase](https://github.com/chasey-dev/immortalwrt-mt798x-rebase) 25.12 分支，融合现代轻量化组件与核心依赖保护。

---

## 固件特性

- **无线与硬件加速**：
  - 联发科 MT7986 原厂闭源无线驱动（AX6000 高性能 Wi-Fi 6，支持 160MHz）。
  - MTK 硬件加速（HNAT / Warp / TurboACC-MTK / eQoS-MTK）。
- **现代美观 UI**：
  - 内置 **Aurora** 与 **Argon** 现代化主题，支持暗色模式与深度自定义配置。
- **流量与设备监控**：
  - 集成现代化 **Bandix** 实时设备带宽监控与动态平滑折线图。
  - 剔除传统老旧且冗余的 `wrtbwmon`，系统更轻快。
- **全量核心组件与 Kmod 保留**：
  - **USB 移动共享网络**：`kmod-usb-net-cdc-ether`、`kmod-usb-net-rndis`、`kmod-usb-net-ipheth`（手机热点与随身 WiFi）。
  - **磁盘与文件系统**：`kmod-mtd-rw`（解锁 MTD 读写）、EXT4、FAT32、NTFS3、Btrfs、exFAT。
  - **网络与加密驱动**：`kmod-tun`、`kmod-wireguard`、`kmod-tcp-bbr`、`kmod-nft-tproxy`、全量 Crypto 与 iptables/nftables 核心模块。
  - **Release 全量 Kmod 打包**：编译发布时自动将所有 kmod 归档为 `kmods_S20P_*.tar.gz` 上传至 Release，彻底避免后续安装驱动出现 vermagic 内核版本哈希不匹配的问题。
- **依赖保护与扩展性**：
  - 集成 `package/luci-compat-keep` 依赖守护包，确保 `luci-compat` 不被清理。
  - 预装完整 Ruby 环境（`ruby`, `ruby-yaml` 等）及 `chinadns-ng`、`ipt2socks`、`coreutils`，固件保持轻简的同时，方便后续直接手动安装 OpenClash 或其它拓展包。

---

## 默认配置信息

- **默认后台 IP**：`192.168.1.1`
- **默认用户**：`root` / 无密码
- **默认 Wi-Fi 密码**：`12345678`

---

## 本地编译

### 1. 准备环境 (Ubuntu LTS)

```bash
sudo apt -y update
sudo apt -y full-upgrade
sudo apt install -y dos2unix libfuse-dev build-essential
sudo bash -c 'bash <(curl -sL https://build-scripts.immortalwrt.org/init_build_environment.sh)'
```

### 2. 编译固件

```bash
# 克隆仓库
git clone https://github.com/stmtc233/wrt_release.git
cd wrt_release

# 执行完整编译
./build.sh S20P

# 调试模式（只生成 .config 供检查）
./build.sh S20P debug

# 预览配置片段组合
./build.sh S20P config_preview
```

编译产物将保存在根目录的 `firmware/` 文件夹中。

---

## GitHub Actions 自动云编译

- **手动触发编译**：进入 Actions -> 选择 **Build WRT** 或 **Release WRT** -> 点击 **Run workflow**，机型选择 `S20P` 即可。
- **自动上游跟踪**：工作流 **Upstream Watch** 会每 6 小时自动检测上游源码分支变动，并在检测到更新后自动触发 Release 构建。

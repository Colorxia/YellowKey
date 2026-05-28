<div align="center">

# YellowKey - BitLocker 绕过漏洞

[![English](https://img.shields.io/badge/English-README-blue)](README_EN.md)
[![中文](https://img.shields.io/badge/中文-README-green)](README.md)

> ⚠️ **免责声明：** 本项目仅供安全研究和授权渗透测试使用。使用者需遵守当地法律法规，作者不对任何滥用行为负责。

</div>

## 📋 漏洞概述

**YellowKey** 是一个影响 Windows BitLocker 加密的严重安全漏洞，攻击者可以通过该漏洞在无需密码的情况下，获得对 BitLocker 保护卷的完全访问权限。

该漏洞位于 Windows 恢复环境（WinRE）中的一个特殊组件，原作者发现其行为极其可疑，几乎像是一个**后门（Backdoor）**。

---

## 🔍 漏洞详情

### 影响范围

| 系统 | 影响状态 |
|------|----------|
| Windows 11 | ✅ 受影响 |
| Windows Server 2022 | ✅ 受影响 |
| Windows Server 2025 | ✅ 受影响 |
| Windows 10 | ❌ 不受影响 |

### 可疑特征

1. **组件隐蔽性**：触发漏洞的组件仅存在于 WinRE 镜像中，在互联网上找不到任何公开资料
2. **同名不同功能**：该组件在普通 Windows 安装中也存在（同名），但不包含触发 BitLocker 绕过的功能
3. **选择性影响**：仅影响 Windows 11 及 Server 2022/2025，Windows 10 完全免疫

> 💡 原作者评论："除了这是故意的之外，我找不到任何其他解释。"

---

## 🛠️ 复现步骤

### 准备工作

- 一个 USB 存储设备（建议 NTFS 格式，FAT32/exFAT 也可）
- 一台启用了 BitLocker 的 Windows 11 / Server 2022/2025 设备
- 本仓库中的 `FsTx` 文件夹

### 操作流程

#### 第一步：准备 USB 设备

将 `FsTx` 文件夹**原样复制**到 USB 设备的以下路径：

```
你的U盘:\System Volume Information\FsTx
```

> 📌 **注意：** `System Volume Information` 是系统隐藏文件夹，需要开启"显示隐藏文件"才能看到。

> 💡 **替代方案：** 你甚至不需要 USB 设备！可以直接将文件复制到磁盘的 EFI 分区中，效果相同。

#### 第二步：插入目标设备

将准备好的 USB 设备插入启用了 BitLocker 保护的目标计算机。

#### 第三步：进入 Windows 恢复环境

1. 点击 **开始菜单** → **电源**
2. **按住 `Shift` 键**的同时，点击 **"重启"**
3. 计算机将进入 Windows 恢复环境（WinRE）

#### 第四步：触发漏洞

1. 点击重启按钮后，**松开 `Shift` 键**
2. **立即按住 `Ctrl` 键**，并**保持不要松开**
3. 等待系统响应

#### 第五步：获取 Shell

如果操作正确，系统将弹出一个具有**完全访问权限**的命令行 Shell，可以访问 BitLocker 保护的卷中的所有数据。

![Shell 截图](https://github.com/user-attachments/assets/eda6c823-4a6b-4aec-bad2-b9afad640dd6)

---

## 📁 项目结构

```
YellowKey/
├── FsTx/
│   └── 95F62703B343F111A92A005056975458/
│       ├── FsTxTemp/
│       │   └── 98F62703B343F111A92A005056975458
│       └── FsTxLogs/
│           ├── FsTxLog.blf
│           ├── FsTxLogContainer00000000000000000001
│           ├── FsTxLogContainer00000000000000000002
│           ├── FsTxKtmLog.blf
│           ├── FsTxKtmLogContainer00000000000000000001
│           └── FsTxKtmLogContainer00000000000000000002
├── LICENSE
├── README.md
└── shell.png
```

---

## 🔐 安全建议

### 对于系统管理员

1. **监控 WinRE 访问**：密切关注非授权的 WinRE 启动行为
2. **检查 EFI 分区**：定期检查 EFI 分区是否存在异常文件
3. **限制物理访问**：确保服务器和关键设备的物理安全
4. **启用 Secure Boot**：确保 Secure Boot 功能已启用并正确配置

### 对于普通用户

1. **保持系统更新**：及时安装 Windows 安全更新
2. **物理安全**：确保计算机不被未授权人员物理接触
3. **启用 TPM + PIN**：在 BitLocker 设置中启用 TPM + PIN 保护

---

## 🙏 致谢

感谢以下团队/组织协助完成漏洞的公开披露：

- **MORSE** - 安全研究团队
- **MSTIC** - 微软安全威胁情报中心
- **Microsoft GHOST** - 微软安全响应团队

---

## 📜 许可证

本项目采用 [MIT License](LICENSE) 开源协议。

---

## ⚖️ 法律声明

本项目仅供以下用途：

- ✅ 安全研究人员进行漏洞分析
- ✅ 企业授权的渗透测试
- ✅ 安全教育培训

以下用途**严格禁止**：

- ❌ 未经授权的系统访问
- ❌ 任何非法入侵行为
- ❌ 恶意数据窃取

使用者需自行承担法律责任，作者概不负责。

---

**如果这个项目对你有帮助，请给个 ⭐ Star 支持一下！**

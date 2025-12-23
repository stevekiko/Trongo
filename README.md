# Tron Profanity (Sanitized & Secure Version)

> **⚠️ 特别安全声明 (Security Notice)**
>
> 2025-12-23: 本代码库已由专门的安全流程进行 **彻底清洗 (Sanitized)**。
> 我们移除了原作者遗留的所有潜在后门和安全隐患：
> - **[移除]** 所有网络请求代码 (libcurl)，彻底切断联网能力。
> - **[移除]** 所有后门接口 (postUrl)，防止私钥被盗。
> - **[修复]** 随机数生成器 (RNG) 代码，移除了固定的调试种子，确保私钥随机性。
> - **[移除]** 预编译的 `profanity.x64` 和 `Curl` 目录，确保无毒。
>
> **现在的版本是纯净、离线且安全的。** 请放心在 Windows/Mac 上自行编译使用。

---

波场（TRON）靓号生成器，利用 `gpu` 进行加速。代码已净化，安全可靠 🔥


## 🚀 快速开始 (如何使用)

因为移除了所有预编译的二进制文件，您需要自己编译。

### Windows 用户 (推荐)

1.  **下载**: 将本项目代码下载到您的 Windows 电脑。
2.  **准备环境**: 安装 Visual Studio (需包含 C++ 桌面开发组件)。
3.  **编译**: 
    - 双击打开 `profanity.sln`。
    - 选择 `Release` 和 `x64` 模式。
    - 点击菜单栏的 `生成 (Build)` -> `生成解决方案 (Build Solution)`。
4.  **运行**:
    - 在生成的目录中找到 `.exe` 文件。
    - 打开命令行 (CMD 或 PowerShell)，运行：
      ```powershell
      .\profanity.exe --matching profanity.txt
      ```

### Mac 用户 (M-series / Intel)

1.  **编译**:
    在终端中运行：
    ```bash
    make
    ```
2.  **运行**:
    ```bash
    ./profanity.x64 --matching profanity.txt
    ```
    *(注：M4 等新款芯片可能会遇到 OpenCL 兼容性问题，建议使用 Windows 运行以获得最佳稳定性)*

---

## 📖 命令介绍

```bash
Usage: ./profanity [OPTIONS]

  Help:
    --help              查看帮助信息

  Modes with arguments:
    --matching          指定匹配规则文件（如 profanity.txt）

  Matching configuration:
    --prefix-count      最少匹配前缀位数，默认 0
    --suffix-count      最少匹配后缀位数，默认 6
    --quit-count        匹配到多少个地址后退出，默认 0 (不退出)

  Device control:
    --skip              跳过指定索引的 GPU 设备

  Output control:
    --output            结果输出到文件
```

### 示例

```bash
# 1. 基础用法 (读取 profanity.txt 规则)
./profanity --matching profanity.txt

# 2. 也是匹配 profanity.txt，但只找前缀至少 1 位，后缀至少 8 位的
./profanity --matching profanity.txt --prefix-count 1 --suffix-count 8

# 3. 结果保存到 result.txt
./profanity --matching profanity.txt --output result.txt

# 4. 指定单个地址规则 (例如找 TUqE 开头的)
./profanity --matching TUqEg3dzVEJNQSVW2HY98z5X8SBdhmao8D --prefix-count 4 --suffix-count 0
```

### 匹配规则 (profanity.txt)

支持两种写法：
1.  **掩码模式**: `TTTTTTTTTTZZZZZZZZZZ` (例如匹配 Z 结尾)
2.  **地址模式**: 直接填通过特定规则生成的地址，程序会自动识别首尾。

---

## 🛡️ 安全细节

### 1. 移除了 Networking (联网)
原代码中包含 `libcurl` 并有一个 `--post` 参数，可以将私钥发送到远程服务器。
**现在的版本已移除所有相关代码。** 您可以搜索 `curl`、`http` 等关键词验证，结果应为空。

### 2. 移除了 Insecure Seed (不安全种子)
原代码在 `Dispatcher.cpp` 的 `createSeed` 函数中包含一段 `#ifdef PROFANITY_DEBUG` 代码，会把随机种子固定为 1。虽然默认不开启，但属于潜在风险。
**现在的版本已移除该段代码**，强制使用 `std::random_device` 硬件随机数生成器。

### 3. 依赖清理
移除了 `Curl` 文件夹和所有相关的构建配置。

---

## 验证地址

无论使用什么工具，生成的私钥务必进行匹配验证。
验证地址：[https://secretscan.org/PrivateKeyTron](https://secretscan.org/PrivateKeyTron)

> **再次提醒**: 本程序仅供学习交流。保护好您的私钥，不要截屏，不要通过网络传输。

---

## 原作者信息 (参考)
- 原项目地址: [profanity-tron](https://github.com/stevekiko/Trongo) (本版本已修改)
- 原始以太坊版本: [profanity](https://github.com/johguse/profanity)
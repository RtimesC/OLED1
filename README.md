# OLED1 - STM32 OLED 显示驱动库

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Language: C](https://img.shields.io/badge/Language-C-blue.svg)](https://en.wikipedia.org/wiki/C_(programming_language))
[![Platform: STM32](https://img.shields.io/badge/Platform-STM32-green.svg)](https://www.st.com/en/microcontrollers-microprocessors/stm32-32-bit-arm-cortex-mcus.html)

一个专业的基于 STM32 的 OLED (SSD1306) 显示屏驱动库，支持 I2C 通信协议。

## 📋 目录

- [项目简介](#项目简介)
- [功能特性](#功能特性)
- [快速开始](#快速开始)
- [硬件要求](#硬件要求)
- [软件要求](#软件要求)
- [编译和烧录](#编译和烧录)
- [项目结构](#项目结构)
- [API 使用示例](#api-使用示例)
- [贡献指南](#贡献指南)
- [许可证](#许可证)
- [联系方式](#联系方式)

## 项目简介

本项目实现了一个完整的 OLED 显示屏驱动程序，基于 STM32 微控制器和 SSD1306 控制器。该驱动库提供了丰富的显示功能，包括字符、字符串、数字显示等，适用于各种嵌入式显示应用场景。

## 功能特性

✨ **核心功能**

- 🖥️ **完整的 OLED 驱动** - 支持 SSD1306 控制器（128x64 分辨率）
- 🔌 **I2C 通信** - 高效的 I2C 总线通信实现
- 📝 **字符显示** - 支持单个字符和字符串显示
- 🔢 **数字显示** - 支持十进制、十六进制、二进制格式显示
- 💡 **LED 控制** - 集成 LED 控制功能
- ⌨️ **按键输入** - 支持按键扫描和检测
- 📚 **内置字体** - 包含多种字体大小支持

## 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/RtimesC/OLED1.git
cd OLED1
```

### 2. 打开项目

使用 Keil µVision 打开 `Project.uvprojx` 文件

### 3. 编译并烧录

点击编译按钮，然后将程序下载到 STM32 开发板

### 4. 运行示例

项目会自动运行示例代码，在 OLED 屏幕上显示测试信息

## 硬件要求

### 必需硬件

- **微控制器**: STM32F10x 系列（推荐 STM32F103C8T6）
- **显示屏**: OLED 模块（SSD1306 控制器，I2C 接口）
- **调试器**: ST-Link V2 或兼容调试器

### 可选硬件

- LED 指示灯
- 按键模块
- 面包板和杜邦线

## 软件要求

### 开发环境

- **IDE**: Keil µVision 5 或更高版本
- **编译器**: ARM Compiler (推荐 V5 或 V6)
- **调试工具**: ST-Link Utility 或 OpenOCD

### 依赖库

- STM32 标准外设库 (STM32F10x StdPeriph Driver)
- CMSIS 核心支持文件

## 编译和烧录

### 编译步骤

1. 打开 Keil µVision
2. 加载项目文件 `Project.uvprojx`
3. 选择目标板型号
4. 点击 `Project` -> `Build Target` (F7)
5. 等待编译完成

### 烧录步骤

1. 连接 ST-Link 调试器到 STM32 开发板
2. 在 Keil 中点击 `Flash` -> `Download` (F8)
3. 等待烧录完成
4. 复位开发板运行程序

## 项目结构

```
OLED1/
├── .github/              # GitHub 配置文件
│   ├── ISSUE_TEMPLATE/   # Issue 模板
│   └── PULL_REQUEST_TEMPLATE.md
├── docs/                 # 文档目录
│   ├── API.md           # API 参考文档
│   ├── HARDWARE.md      # 硬件连接说明
│   └── DEVELOPMENT.md   # 开发指南
├── Hardware/             # 硬件驱动代码
│   ├── LED.c            # LED 驱动实现
│   ├── LED.h            # LED 驱动头文件
│   ├── Key.c            # 按键驱动实现
│   ├── Key.h            # 按键驱动头文件
│   ├── OLED.c           # OLED 驱动实现
│   ├── OLED.h           # OLED 驱动头文件
│   └── OLED_Font.h      # OLED 字体数据
├── Library/              # STM32 标准库文件
├── System/               # 系统配置文件
├── User/                 # 用户应用代码
│   ├── main.c           # 主程序
│   ├── stm32f10x_conf.h # STM32 配置
│   ├── stm32f10x_it.c   # 中断处理
│   └── stm32f10x_it.h   # 中断处理头文件
├── Start/                # 启动文件
├── .editorconfig         # 编辑器配置
├── .gitignore           # Git 忽略文件配置
├── CHANGELOG.md         # 变更日志
├── CONTRIBUTING.md      # 贡献指南
├── LICENSE              # MIT 许可证
├── Project.uvprojx      # Keil 项目文件
└── README.md            # 项目说明文档
```

## API 使用示例

### OLED 初始化和显示

```c
#include "OLED.h"

int main(void)
{
    // 初始化 OLED
    OLED_Init();
    
    // 显示字符串
    OLED_ShowString(1, 1, "Hello World!");
    
    // 显示数字
    OLED_ShowNum(2, 1, 12345, 5);
    
    // 显示十六进制数
    OLED_ShowHexNum(3, 1, 0xABCD, 4);
    
    // 显示二进制数
    OLED_ShowBinNum(4, 1, 0b10101010, 8);
    
    while(1)
    {
        // 主循环
    }
}
```

### LED 控制

```c
#include "LED.h"

int main(void)
{
    // 初始化 LED
    LED_Init();
    
    // 打开 LED1
    LED1_ON();
    
    // 关闭 LED1
    LED1_OFF();
    
    // 切换 LED 状态
    LED1_TURN();
}
```

### 按键检测

```c
#include "Key.h"

int main(void)
{
    uint8_t KeyNum;
    
    // 初始化按键
    Key_Init();
    
    while(1)
    {
        // 获取按键值
        KeyNum = Key_GetNum();
        
        if(KeyNum == 1)
        {
            // 按键 1 被按下
        }
    }
}
```

详细的 API 文档请参考 [docs/API.md](docs/API.md)

## 贡献指南

我们欢迎所有形式的贡献！在提交贡献之前，请阅读我们的 [贡献指南](CONTRIBUTING.md)。

### 如何贡献

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启一个 Pull Request

## 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 联系方式

- 项目地址: [https://github.com/RtimesC/OLED1](https://github.com/RtimesC/OLED1)
- 作者: RtimesC
- 问题反馈: [GitHub Issues](https://github.com/RtimesC/OLED1/issues)

---

⭐ 如果这个项目对你有帮助，请给它一个 Star！
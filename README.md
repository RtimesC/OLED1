# OLED1 - STM32F10x OLED Display Driver

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-STM32F10x-blue.svg)](https://www.st.com/en/microcontrollers-microprocessors/stm32f103.html)
[![Language](https://img.shields.io/badge/language-C-orange.svg)](https://en.wikipedia.org/wiki/C_(programming_language))
[![Keil](https://img.shields.io/badge/IDE-Keil%20µVision-green.svg)](http://www.keil.com/)

基于 STM32F10x 的 OLED 显示屏驱动库，支持 I2C 通信协议。

---

## 📋 目录

- [项目简介](#项目简介)
- [功能特性](#功能特性)
- [硬件要求](#硬件要求)
- [快速开始](#快速开始)
- [项目结构](#项目结构)
- [API 文档](#api-文档)
- [示例代码](#示例代码)
- [配置说明](#配置说明)
- [常见问题](#常见问题)
- [贡献指南](#贡献指南)
- [许可证](#许可证)
- [作者](#作者)
- [致谢](#致谢)
- [联系方式](#联系方式)

---

## 📖 项目简介

OLED1 是一个针对 STM32F10x 系列微控制器开发的 OLED 显示屏驱动库。该项目提供了完整的 OLED 显示功能，包括字符、字符串、数字（十进制/十六进制/二进制）的显示。

### 主要特点

- ✅ **简单易用**：提供简洁的 API 接口，快速上手
- ✅ **功能完整**：支持多种数据格式显示
- ✅ **软件 I2C**：无需硬件 I2C 外设，任意 GPIO 可用
- ✅ **文档完善**：详细的 API 文档和硬件连接指南
- ✅ **开源免费**：MIT 许可证，可自由使用和修改
- ✅ **标准化代码**：遵循编码规范，易于维护和扩展

### 适用场景

- 🎓 **学习项目**：适合初学者学习 STM32 和 OLED 使用
- 🔧 **嵌入式开发**：可集成到各种嵌入式项目中
- 📊 **数据显示**：传感器数据、系统状态显示
- 🎮 **人机交互**：简单的用户界面显示
- 🛠️ **调试工具**：快速显示调试信息

---

## ✨ 功能特性

### 显示功能

| 功能 | 函数 | 说明 |
|------|------|------|
| **字符显示** | `OLED_ShowChar()` | 显示单个 ASCII 字符 |
| **字符串显示** | `OLED_ShowString()` | 显示字符串 |
| **数字显示** | `OLED_ShowNum()` | 显示无符号十进制数 |
| **有符号数显示** | `OLED_ShowSignedNum()` | 显示有符号十进制数（支持负数） |
| **十六进制显示** | `OLED_ShowHexNum()` | 显示十六进制数 |
| **二进制显示** | `OLED_ShowBinNum()` | 显示二进制数 |
| **清屏** | `OLED_Clear()` | 清空显示内容 |

### 通信特性

- **协议**：I2C（软件模拟）
- **速度**：可调节（通过延时参数配置）
- **引脚**：任意 GPIO 引脚可用
- **默认引脚**：PB8（SCL）、PB9（SDA）

### 显示参数

- **分辨率**：128 × 64 像素
- **显示区域**：4 行 × 16 列（8x16 字体）
- **字体**：8x16 ASCII 字体
- **字符集**：支持 ASCII 可打印字符（32-126）

---

## 🔧 硬件要求

### 开发板

- **推荐型号**：STM32F103C8T6 最小系统板（蓝色核心板）
- **MCU 系列**：STM32F10x（Cortex-M3）
- **最小 Flash**：32KB（程序约 10KB）
- **最小 RAM**：10KB

### OLED 模块

- **型号**：0.96 寸 I2C OLED 显示屏
- **分辨率**：128×64
- **驱动芯片**：SSD1306
- **工作电压**：3.3V（推荐）或 5V
- **通信接口**：I2C（4 线）

### 引脚连接表

| OLED 引脚 | STM32 引脚 | 功能说明 |
|-----------|-----------|----------|
| **VCC** | 3.3V | 电源正极（3.3V 推荐） |
| **GND** | GND | 电源地 |
| **SCL** | **PB8** | I2C 时钟线（可自定义） |
| **SDA** | **PB9** | I2C 数据线（可自定义） |

### 接线图

```
    STM32F103C8T6            0.96" OLED
  +----------------+        +------------+
  |                |        |            |
  |            3.3V|--------|VCC         |
  |            GND |--------|GND         |
  |            PB8 |--------|SCL         |
  |            PB9 |--------|SDA         |
  |                |        |            |
  +----------------+        +------------+
```

**详细硬件连接指南请参考：[docs/HARDWARE.md](docs/HARDWARE.md)**

---

## 🚀 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/RtimesC/OLED1.git
cd OLED1
```

### 2. 硬件连接

按照上面的引脚连接表连接 OLED 模块到 STM32 开发板：

1. 连接电源：VCC → 3.3V，GND → GND
2. 连接 I2C：SCL → PB8，SDA → PB9
3. 确保连接牢固，避免接触不良

### 3. 打开工程

1. 安装 **Keil µVision**（推荐 V5.28 或更高版本）
2. 使用 Keil µVision 打开 `Project.uvprojx` 文件
3. 选择正确的目标设备（Target）

### 4. 编译下载

```
1. 点击工具栏的"编译"按钮（或按 F7）
2. 连接 ST-Link 调试器到开发板
3. 点击"下载"按钮（或按 F8）
```

### 5. 运行测试

下载完成后，OLED 应该显示示例内容：

```
第 1 行：A  HelloWorld!
第 2 行：（可自定义）
第 3 行：（可自定义）
第 4 行：（可自定义）
```

### 6. 开始开发

修改 `User/main.c` 文件，添加您的显示代码：

```c
#include "stm32f10x.h"
#include "OLED.h"
#include "Delay.h"

int main(void)
{
    // 初始化 OLED
    OLED_Init();
    
    // 显示欢迎信息
    OLED_ShowString(1, 1, "Hello, OLED!");
    
    // 显示数字
    OLED_ShowNum(2, 1, 12345, 5);
    
    while (1)
    {
        // 您的代码
    }
}
```

---

## 📁 项目结构

```
OLED1/
│
├── Hardware/              # 硬件驱动代码
│   ├── OLED.c            # OLED 驱动实现
│   ├── OLED.h            # OLED 驱动头文件
│   ├── OLED_Font.h       # OLED 字体库（8x16 ASCII）
│   ├── LED.c             # LED 驱动（可选）
│   ├── LED.h             # LED 头文件
│   ├── Key.c             # 按键驱动（可选）
│   └── Key.h             # 按键头文件
│
├── System/                # 系统相关代码
│   ├── Delay.c           # 延时函数实现
│   └── Delay.h           # 延时函数头文件
│
├── User/                  # 用户应用代码
│   ├── main.c            # 主程序入口
│   ├── stm32f10x_conf.h  # STM32 库配置文件
│   ├── stm32f10x_it.c    # 中断服务函数
│   └── stm32f10x_it.h    # 中断头文件
│
├── Library/               # STM32 标准外设库
│   ├── inc/              # 头文件
│   └── src/              # 源文件
│
├── Start/                 # 启动文件
│   ├── startup_stm32f10x_md.s  # 启动汇编文件
│   ├── core_cm3.c        # Cortex-M3 核心文件
│   ├── core_cm3.h        # 核心头文件
│   ├── system_stm32f10x.c # 系统初始化
│   └── system_stm32f10x.h # 系统头文件
│
├── docs/                  # 项目文档
│   ├── API.md            # API 参考文档
│   └── HARDWARE.md       # 硬件连接指南
│
├── Project.uvprojx        # Keil 工程文件
├── Project.uvoptx         # Keil 选项文件
├── README.md              # 项目说明文档（本文件）
├── LICENSE                # MIT 许可证
├── CHANGELOG.md           # 更新日志
├── CONTRIBUTING.md        # 贡献指南
└── .gitignore             # Git 忽略配置
```

### 目录说明

- **Hardware/**：包含所有硬件外设的驱动代码，OLED 驱动是核心部分
- **System/**：系统级代码，如延时函数等
- **User/**：用户应用代码，`main.c` 是程序入口
- **Library/**：STM32 标准外设库（SPL）
- **Start/**：启动文件和系统初始化代码
- **docs/**：详细的文档，包括 API 和硬件指南

---

## 📖 API 文档

### OLED 初始化和控制

#### OLED_Init()

初始化 OLED 显示屏和 I2C 通信。

```c
void OLED_Init(void);
```

**使用示例：**
```c
OLED_Init();  // 在使用任何显示功能前必须先调用
```

---

#### OLED_Clear()

清空 OLED 显示内容。

```c
void OLED_Clear(void);
```

**使用示例：**
```c
OLED_Clear();  // 清空显示
```

---

### 字符和字符串显示

#### OLED_ShowChar()

在指定位置显示单个字符。

```c
void OLED_ShowChar(uint8_t Line, uint8_t Column, char Char);
```

**参数：**
- `Line`：行号（1-4）
- `Column`：列号（1-16）
- `Char`：要显示的字符（ASCII 32-126）

**使用示例：**
```c
OLED_ShowChar(1, 1, 'A');      // 第 1 行第 1 列显示 'A'
OLED_ShowChar(2, 5, '@');      // 第 2 行第 5 列显示 '@'
```

---

#### OLED_ShowString()

在指定位置显示字符串。

```c
void OLED_ShowString(uint8_t Line, uint8_t Column, char *String);
```

**参数：**
- `Line`：起始行号（1-4）
- `Column`：起始列号（1-16）
- `String`：要显示的字符串（以 '\0' 结尾）

**使用示例：**
```c
OLED_ShowString(1, 1, "Hello");           // 显示 "Hello"
OLED_ShowString(2, 1, "STM32 OLED");     // 显示 "STM32 OLED"

// 显示变量内容
char buffer[20];
sprintf(buffer, "Temp: %d C", temperature);
OLED_ShowString(3, 1, buffer);
```

---

### 数字显示

#### OLED_ShowNum()

显示无符号十进制数。

```c
void OLED_ShowNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
```

**参数：**
- `Line`：行号（1-4）
- `Column`：列号（1-16）
- `Number`：要显示的数字（0 - 4294967295）
- `Length`：显示位数（不足补 0）

**使用示例：**
```c
OLED_ShowNum(1, 1, 123, 5);    // 显示 "00123"
OLED_ShowNum(2, 1, 9999, 4);   // 显示 "9999"

// 显示传感器数值
uint32_t adc_value = 3250;
OLED_ShowNum(3, 1, adc_value, 4);  // 显示 "3250"
```

---

#### OLED_ShowSignedNum()

显示有符号十进制数（支持负数）。

```c
void OLED_ShowSignedNum(uint8_t Line, uint8_t Column, int32_t Number, uint8_t Length);
```

**参数：**
- `Line`：行号（1-4）
- `Column`：列号（1-16）
- `Number`：要显示的数字（可以是负数）
- `Length`：数值部分的显示位数

**使用示例：**
```c
OLED_ShowSignedNum(1, 1, -25, 3);   // 显示 "-025"
OLED_ShowSignedNum(2, 1, 100, 4);   // 显示 " 0100"

// 显示温度（可能为负）
int32_t temperature = -15;
OLED_ShowString(3, 1, "Temp:");
OLED_ShowSignedNum(3, 6, temperature, 3);  // 显示 "-015"
```

---

#### OLED_ShowHexNum()

显示十六进制数。

```c
void OLED_ShowHexNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
```

**参数：**
- `Line`：行号（1-4）
- `Column`：列号（1-16）
- `Number`：要显示的数字
- `Length`：显示位数（十六进制位）

**使用示例：**
```c
OLED_ShowHexNum(1, 1, 0xFF, 2);      // 显示 "FF"
OLED_ShowHexNum(2, 1, 0x1A2B, 4);    // 显示 "1A2B"

// 显示寄存器值
uint32_t reg = 0xABCD1234;
OLED_ShowString(3, 1, "0x");
OLED_ShowHexNum(3, 3, reg, 8);       // 显示 "ABCD1234"
```

---

#### OLED_ShowBinNum()

显示二进制数。

```c
void OLED_ShowBinNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
```

**参数：**
- `Line`：行号（1-4）
- `Column`：列号（1-16）
- `Number`：要显示的数字
- `Length`：显示位数（二进制位）

**使用示例：**
```c
OLED_ShowBinNum(1, 1, 5, 4);         // 显示 "0101"
OLED_ShowBinNum(2, 1, 0xFF, 8);      // 显示 "11111111"

// 显示端口状态
uint8_t port_status = GPIOA->IDR & 0xFF;
OLED_ShowString(3, 1, "PA:");
OLED_ShowBinNum(3, 4, port_status, 8);  // 显示端口状态
```

---

### 延时函数

#### Delay_us()

微秒级延时。

```c
void Delay_us(uint32_t us);
```

**参数：**
- `us`：延时时间（微秒）

**使用示例：**
```c
Delay_us(100);  // 延时 100 微秒
```

---

#### Delay_ms()

毫秒级延时。

```c
void Delay_ms(uint32_t ms);
```

**参数：**
- `ms`：延时时间（毫秒）

**使用示例：**
```c
Delay_ms(500);  // 延时 500 毫秒
```

---

#### Delay_s()

秒级延时。

```c
void Delay_s(uint32_t s);
```

**参数：**
- `s`：延时时间（秒）

**使用示例：**
```c
Delay_s(2);  // 延时 2 秒
```

---

**完整 API 文档请参考：[docs/API.md](docs/API.md)**

---

## 💡 示例代码

### 示例 1：基础显示

最简单的 OLED 显示示例：

```c
#include "stm32f10x.h"
#include "OLED.h"

int main(void)
{
    // 初始化 OLED
    OLED_Init();
    
    // 显示文本
    OLED_ShowString(1, 1, "OLED Demo");
    OLED_ShowString(2, 1, "STM32F103");
    
    // 显示数字
    OLED_ShowString(3, 1, "Num:");
    OLED_ShowNum(3, 6, 12345, 5);
    
    // 显示十六进制
    OLED_ShowString(4, 1, "Hex:");
    OLED_ShowHexNum(4, 6, 0xABCD, 4);
    
    while (1)
    {
        // 主循环
    }
}
```

**效果：**
```
行1: OLED Demo
行2: STM32F103
行3: Num: 12345
行4: Hex: ABCD
```

---

### 示例 2：动态数字显示

显示递增的计数器：

```c
#include "stm32f10x.h"
#include "OLED.h"
#include "Delay.h"

int main(void)
{
    uint32_t counter = 0;
    
    // 初始化
    OLED_Init();
    
    // 显示标题
    OLED_ShowString(1, 1, "Counter:");
    
    while (1)
    {
        // 更新计数器显示
        OLED_ShowNum(2, 1, counter, 6);
        counter++;
        
        // 延时
        Delay_ms(100);
        
        // 防止溢出
        if (counter > 999999) {
            counter = 0;
        }
    }
}
```

**效果：**  
计数器从 000000 开始，每 100ms 递增 1。

---

### 示例 3：传感器数据显示

显示温度和湿度数据：

```c
#include "stm32f10x.h"
#include "OLED.h"
#include "Delay.h"

// 模拟读取传感器数据
int32_t read_temperature(void) {
    return 25;  // 实际应从传感器读取
}

uint32_t read_humidity(void) {
    return 60;  // 实际应从传感器读取
}

int main(void)
{
    int32_t temperature;
    uint32_t humidity;
    
    // 初始化
    OLED_Init();
    
    // 显示标题
    OLED_ShowString(1, 1, "Sensor Data");
    OLED_ShowString(2, 1, "Temp:");
    OLED_ShowString(3, 1, "Humi:");
    
    while (1)
    {
        // 读取传感器数据
        temperature = read_temperature();
        humidity = read_humidity();
        
        // 显示温度（支持负数）
        OLED_ShowSignedNum(2, 6, temperature, 3);
        OLED_ShowString(2, 10, "C");
        
        // 显示湿度
        OLED_ShowNum(3, 6, humidity, 3);
        OLED_ShowString(3, 10, "%");
        
        // 每秒更新一次
        Delay_ms(1000);
    }
}
```

**效果：**
```
行1: Sensor Data
行2: Temp: 025 C
行3: Humi: 060 %
行4: (空)
```

---

### 示例 4：系统状态显示

显示系统运行时间和状态：

```c
#include "stm32f10x.h"
#include "OLED.h"
#include "Delay.h"

int main(void)
{
    uint32_t seconds = 0;
    uint32_t minutes = 0;
    uint32_t hours = 0;
    
    // 初始化
    OLED_Init();
    
    // 显示标题
    OLED_ShowString(1, 1, "System Status");
    OLED_ShowString(2, 1, "Uptime:");
    
    while (1)
    {
        // 显示运行时间（HH:MM:SS）
        OLED_ShowNum(3, 1, hours, 2);
        OLED_ShowChar(3, 3, ':');
        OLED_ShowNum(3, 4, minutes, 2);
        OLED_ShowChar(3, 6, ':');
        OLED_ShowNum(3, 7, seconds, 2);
        
        // 延时 1 秒
        Delay_ms(1000);
        
        // 更新时间
        seconds++;
        if (seconds >= 60) {
            seconds = 0;
            minutes++;
            if (minutes >= 60) {
                minutes = 0;
                hours++;
                if (hours >= 24) {
                    hours = 0;
                }
            }
        }
    }
}
```

**效果：**
```
行1: System Status
行2: Uptime:
行3: 00:05:23
行4: (空)
```

---

### 示例 5：多格式数据显示

同时显示十进制、十六进制和二进制：

```c
#include "stm32f10x.h"
#include "OLED.h"
#include "Delay.h"

int main(void)
{
    uint8_t value = 0;
    
    // 初始化
    OLED_Init();
    
    // 显示标题
    OLED_ShowString(1, 1, "Multi-Format");
    
    while (1)
    {
        // 十进制
        OLED_ShowString(2, 1, "Dec:");
        OLED_ShowNum(2, 6, value, 3);
        
        // 十六进制
        OLED_ShowString(3, 1, "Hex:");
        OLED_ShowHexNum(3, 6, value, 2);
        
        // 二进制
        OLED_ShowString(4, 1, "Bin:");
        OLED_ShowBinNum(4, 6, value, 8);
        
        // 延时并递增
        Delay_ms(200);
        value++;
    }
}
```

**效果（value=5 时）：**
```
行1: Multi-Format
行2: Dec: 005
行3: Hex: 05
行4: Bin: 00000101
```

---

### 示例 6：滚动显示

实现简单的文字滚动效果：

```c
#include "stm32f10x.h"
#include "OLED.h"
#include "Delay.h"

int main(void)
{
    uint8_t offset = 0;
    char *message = "  STM32 OLED Demo - Hello World!  ";
    
    // 初始化
    OLED_Init();
    OLED_ShowString(1, 1, "Scrolling Text:");
    
    while (1)
    {
        // 清除第 2 行
        OLED_ShowString(2, 1, "                ");
        
        // 显示从 offset 开始的 16 个字符
        for (uint8_t i = 0; i < 16 && message[offset + i] != '\0'; i++) {
            OLED_ShowChar(2, i + 1, message[offset + i]);
        }
        
        // 移动偏移量
        offset++;
        
        // 循环
        if (message[offset] == '\0' || offset > 20) {
            offset = 0;
        }
        
        Delay_ms(300);
    }
}
```

---

## ⚙️ 配置说明

### I2C 引脚配置

如果需要使用不同的 GPIO 引脚，请修改 `Hardware/OLED.c` 文件。

**步骤 1：修改引脚宏定义**

找到文件顶部的引脚定义：

```c
/*引脚配置*/
#define OLED_W_SCL(x)   GPIO_WriteBit(GPIOB, GPIO_Pin_8, (BitAction)(x))
#define OLED_W_SDA(x)   GPIO_WriteBit(GPIOB, GPIO_Pin_9, (BitAction)(x))
#define OLED_R_SDA()    GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_9)
```

**修改示例（改为 PA6 和 PA7）：**

```c
/*引脚配置*/
#define OLED_W_SCL(x)   GPIO_WriteBit(GPIOA, GPIO_Pin_6, (BitAction)(x))
#define OLED_W_SDA(x)   GPIO_WriteBit(GPIOA, GPIO_Pin_7, (BitAction)(x))
#define OLED_R_SDA()    GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_7)
```

**步骤 2：修改初始化函数**

在同一文件中找到 `OLED_I2C_Init()` 函数，修改 GPIO 初始化代码：

```c
void OLED_I2C_Init(void)
{
    // 使能 GPIOA 时钟（根据实际使用的端口修改）
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_OD;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    
    // 初始化 SCL 引脚
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6;  // 修改为新引脚
    GPIO_Init(GPIOA, &GPIO_InitStructure);     // 修改端口
    
    // 初始化 SDA 引脚
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_7;  // 修改为新引脚
    GPIO_Init(GPIOA, &GPIO_InitStructure);     // 修改端口
    
    OLED_W_SCL(1);
    OLED_W_SDA(1);
}
```

---

### I2C 速度调整

修改 I2C 通信速度以适应不同的应用场景。

在 `Hardware/OLED.c` 文件中找到延时配置：

```c
/*I2C时序延时配置 - 根据实际OLED响应速度调整*/
#define I2C_DELAY_ITERATIONS    10  // 默认值
```

**调整建议：**

| 场景 | 建议值 | 说明 |
|------|--------|------|
| **短距离接线（<10cm）** | 5-10 | 更快的通信速度 |
| **标准应用** | 10-20 | 默认，兼容性好 |
| **长距离接线（>20cm）** | 20-50 | 更稳定，速度较慢 |
| **通信不稳定** | 30-100 | 增大延时以提高稳定性 |

**示例：**

```c
// 提高速度（短距离，高质量接线）
#define I2C_DELAY_ITERATIONS    5

// 提高稳定性（长距离，干扰环境）
#define I2C_DELAY_ITERATIONS    50
```

---

### 显示参数配置

#### 字体大小

当前使用 8x16 像素的 ASCII 字体，字体数据在 `Hardware/OLED_Font.h` 中定义。

如需更换字体：
1. 准备新的字体数组
2. 替换 `OLED_Font.h` 中的字体数据
3. 如果字体大小改变，需修改显示函数

#### 显示区域

- **分辨率**：128×64 像素
- **可显示行数**：4 行（8x16 字体）
- **可显示列数**：16 列（每个字符 8 像素宽）

#### OLED 地址

如果您的 OLED 模块使用不同的 I2C 地址，请修改 `Hardware/OLED.c`：

```c
/*写地址*/
#define OLED_ADDRESS    0x78  // 默认地址

// 如果您的模块地址是 0x7A
#define OLED_ADDRESS    0x7A
```

---

## ❓ 常见问题

### Q1: OLED 不显示任何内容？

**A:** 请检查以下几点：

1. **电源连接**
   - 确认 VCC 连接到 3.3V（不是 5V）
   - 确认 GND 正确连接
   - 用万用表测量 OLED VCC 引脚电压

2. **引脚连接**
   - 确认 SCL 连接到 PB8
   - 确认 SDA 连接到 PB9
   - 检查杜邦线是否插紧

3. **代码检查**
   - 确认调用了 `OLED_Init()`
   - 确认显示代码在初始化之后

4. **硬件检查**
   - 更换一个新的 OLED 模块测试
   - 检查 OLED 模块是否有明显损坏

---

### Q2: 显示内容乱码或花屏？

**A:** 可能的原因和解决方法：

1. **I2C 通信速度过快**
   ```c
   // 增大延时参数
   #define I2C_DELAY_ITERATIONS    20  // 或更大
   ```

2. **接线过长**
   - 缩短杜邦线长度（建议 <20cm）
   - 使用屏蔽双绞线
   - 降低 I2C 速度

3. **上拉电阻问题**
   - 检查 OLED 模块是否有上拉电阻
   - 如果外部重复添加了上拉，可能导致问题
   - 确认上拉电阻值为 4.7kΩ

4. **电源不稳定**
   - 在 VCC 和 GND 之间加 0.1uF 电容
   - 使用稳压电源

---

### Q3: 编译时出现 "undefined reference" 错误？

**A:** 解决步骤：

1. **检查文件是否添加到工程**
   - 在 Keil 工程树中确认所有 `.c` 文件都已添加
   - 右键工程 → Add Files → 添加缺失文件

2. **检查头文件路径**
   - Options → C/C++ → Include Paths
   - 添加 `Hardware`、`System`、`User` 目录

3. **检查函数声明**
   - 确认 `.h` 文件中有函数声明
   - 确认 `#include` 了正确的头文件

---

### Q4: 如何提高显示更新速度？

**A:** 优化方法：

1. **避免频繁清屏**
   ```c
   // ❌ 不好的做法
   while (1) {
       OLED_Clear();  // 每次都清屏，很慢
       OLED_ShowNum(1, 1, value, 3);
   }
   
   // ✅ 好的做法
   while (1) {
       OLED_ShowNum(1, 1, value, 3);  // 直接覆盖写入
   }
   ```

2. **只更新变化的部分**
   ```c
   // 只更新需要改变的数字，不要重新显示整个屏幕
   ```

3. **减小 I2C 延时**
   ```c
   #define I2C_DELAY_ITERATIONS    5  // 更快的速度
   ```

4. **使用硬件 I2C**
   - 升级为使用 STM32 的硬件 I2C 外设
   - 速度可提升 5-10 倍

---

### Q5: 可以同时使用多个 OLED 吗？

**A:** 可以，但需要修改：

**方法 1：使用不同的 GPIO 引脚**
- 为每个 OLED 使用独立的 GPIO 引脚
- 创建多个 OLED 驱动实例

**方法 2：使用不同的 I2C 地址**
- 某些 OLED 模块支持通过跳线修改地址
- 修改代码中的 `OLED_ADDRESS` 宏
- 切换地址后调用不同的显示函数

---

### Q6: 如何显示中文字符？

**A:** 步骤：

1. **准备中文字库**
   - 制作或下载中文字体数组
   - 常用 16×16 点阵（GB2312 编码）

2. **添加中文显示函数**
   ```c
   void OLED_ShowChinese(uint8_t Line, uint8_t Column, uint8_t Index);
   ```

3. **注意事项**
   - 中文字库较大（几百 KB）
   - 需要较大的 Flash 空间
   - 显示速度较慢

**推荐**：对于简单应用，使用英文和数字即可，无需中文。

---

### Q7: 程序下载后没有任何反应？

**A:** 排查步骤：

1. **检查下载是否成功**
   - Keil 是否提示 "Programming Done"
   - 尝试重新下载

2. **检查复位**
   - 按下开发板的复位按钮
   - 重新上电

3. **检查启动模式**
   - 确认 BOOT0 引脚为低电平（从 Flash 启动）
   - 检查跳线帽位置

4. **调试输出**
   - 使用串口打印调试信息
   - 使用 LED 指示程序运行状态

---

### Q8: 如何在不同行显示不同大小的字符？

**A:** 当前库只支持 8×16 字体。如需不同大小：

1. **添加其他字体库**（如 6×8、16×16）
2. **实现新的显示函数**
3. **手动绘制**使用 `OLED_WriteData()` 函数

---

### Q9: 延时函数不准确怎么办？

**A:** 原因和解决：

1. **系统时钟未正确配置**
   - 检查 `system_stm32f10x.c` 中的时钟配置
   - 确认系统时钟为 72MHz

2. **中断影响**
   - 中断会影响延时精度
   - 关键延时可临时关闭中断

3. **使用硬件定时器**
   - 对于精确延时，使用 TIM 定时器
   - 使用 SysTick 中断方式

---

### Q10: 可以显示图片吗？

**A:** 可以，但需要额外开发：

1. **图片转换**
   - 使用工具将图片转为字节数组（如取模软件）
   - 图片格式：单色位图，128×64 或更小

2. **实现显示函数**
   ```c
   void OLED_ShowImage(uint8_t x, uint8_t y, 
                       uint8_t width, uint8_t height, 
                       const uint8_t *image);
   ```

3. **写入数据**
   - 使用 `OLED_WriteData()` 或 `OLED_WriteData_Multi()`
   - 按页（Page）方式写入位图数据

---

## 🤝 贡献指南

我们欢迎所有形式的贡献！无论是报告 Bug、提出新功能建议，还是提交代码，都请参考我们的贡献指南。

### 如何贡献

1. **Fork 本仓库**
2. **创建您的特性分支** (`git checkout -b feature/AmazingFeature`)
3. **提交您的更改** (`git commit -m 'Add some AmazingFeature'`)
4. **推送到分支** (`git push origin feature/AmazingFeature`)
5. **创建 Pull Request**

### 贡献内容

- 🐛 **Bug 修复**：发现并修复问题
- ✨ **新功能**：添加有用的新特性
- 📝 **文档改进**：改进文档质量
- 🎨 **代码优化**：提升代码质量和性能
- 🧪 **测试**：添加测试用例

### 代码规范

请遵循以下编码规范：

```c
/**
 * @brief  函数简短描述
 * @param  参数1: 参数说明
 * @param  参数2: 参数说明
 * @retval 返回值说明
 */
void FunctionName(uint8_t param1, uint32_t param2)
{
    uint32_t localVariable;  // 局部变量说明
    
    if (condition) {
        // 代码块
    }
}
```

**详细贡献指南请参考：[CONTRIBUTING.md](CONTRIBUTING.md)**

---

## 📄 许可证

本项目采用 MIT 许可证。详见 [LICENSE](LICENSE) 文件。

```
MIT License

Copyright (c) 2026 RtimesC

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

---

## 👨‍💻 作者

**RtimesC**

- GitHub: [@RtimesC](https://github.com/RtimesC)
- 项目链接: [https://github.com/RtimesC/OLED1](https://github.com/RtimesC/OLED1)

---

## 🙏 致谢

感谢以下资源和项目：

- **STMicroelectronics**：提供 STM32 标准外设库
- **Solomon Systech**：SSD1306 OLED 控制器
- **开源社区**：提供了宝贵的参考资料和灵感
- **所有贡献者**：感谢为本项目做出贡献的每一个人

### 参考资源

- [STM32 官方文档](https://www.st.com/en/microcontrollers-microprocessors/stm32f103.html)
- [SSD1306 数据手册](https://www.solomon-systech.com/en/product/advanced-display/oled-display-driver-ic/ssd1306/)
- [Keil µVision 用户手册](http://www.keil.com/support/man/docs/uv4/)

---

## 📞 联系方式

如果您有任何问题、建议或需要帮助，请通过以下方式联系：

- **提交 Issue**: [GitHub Issues](https://github.com/RtimesC/OLED1/issues)
- **Pull Request**: [GitHub Pull Requests](https://github.com/RtimesC/OLED1/pulls)
- **Email**: [您的邮箱]（如果有）

---

## 📚 相关文档

- [API 参考文档](docs/API.md) - 详细的 API 说明
- [硬件连接指南](docs/HARDWARE.md) - 硬件接线和配置
- [更新日志](CHANGELOG.md) - 版本更新记录
- [贡献指南](CONTRIBUTING.md) - 如何为项目做贡献

---

## 🔗 相关项目

- [STM32 HAL OLED 库](https://github.com/4ilo/ssd1306) - 基于 HAL 库的 OLED 驱动
- [u8g2](https://github.com/olikraus/u8g2) - 通用的图形库，支持多种显示器

---

## 📊 项目统计

- **语言**: C (89.8%), Assembly (5.7%)
- **代码行数**: ~2000 行
- **文件数量**: 40+ 文件
- **支持的芯片**: STM32F10x 系列
- **开源许可**: MIT License

---

## 🎯 路线图

### 已完成 ✅

- [x] 基础 OLED 驱动实现
- [x] 字符和字符串显示
- [x] 多种数字格式显示
- [x] 软件 I2C 实现
- [x] 完整的 API 文档
- [x] 硬件连接指南
- [x] 示例代码

### 计划中 📅

- [ ] 添加图形绘制功能（线、矩形、圆形）
- [ ] 添加中文字库支持
- [ ] 添加位图图片显示
- [ ] 支持硬件 I2C（提升速度）
- [ ] 添加动画效果
- [ ] 支持其他 OLED 控制器（SSD1315、SH1106）
- [ ] 添加多个 OLED 支持
- [ ] 优化内存使用

### 欢迎提议 💡

如果您有好的想法或功能建议，欢迎在 [Issues](https://github.com/RtimesC/OLED1/issues) 中提出！

---

## ⭐ Star 历史

如果这个项目对您有帮助，请给它一个 Star ⭐，这是对我们最大的鼓励！

[![Star History Chart](https://api.star-history.com/svg?repos=RtimesC/OLED1&type=Date)](https://star-history.com/#RtimesC/OLED1&Date)

---

## 📝 更新日志

### v1.0.0 (2026-02-02)

**新增：**
- ✨ OLED I2C 驱动基本实现
- ✨ 字符和字符串显示功能
- ✨ 数字显示功能（十进制/十六进制/二进制）
- ✨ 完整的 8x16 ASCII 字体库
- ✨ SysTick 延时函数
- 📝 完整的项目文档
- 📝 API 参考文档
- 📝 硬件连接指南
- 📝 贡献指南

**详细更新历史请参考：[CHANGELOG.md](CHANGELOG.md)**

---

## 🏆 成就徽章

[![GitHub stars](https://img.shields.io/github/stars/RtimesC/OLED1?style=social)](https://github.com/RtimesC/OLED1/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/RtimesC/OLED1?style=social)](https://github.com/RtimesC/OLED1/network/members)
[![GitHub issues](https://img.shields.io/github/issues/RtimesC/OLED1)](https://github.com/RtimesC/OLED1/issues)
[![GitHub pull requests](https://img.shields.io/github/issues-pr/RtimesC/OLED1)](https://github.com/RtimesC/OLED1/pulls)
[![GitHub last commit](https://img.shields.io/github/last-commit/RtimesC/OLED1)](https://github.com/RtimesC/OLED1/commits/main)

---

## 🌟 支持项目

如果您觉得这个项目有用，可以通过以下方式支持：

- ⭐ **Star** 本项目
- 🍴 **Fork** 并改进
- 🐛 报告 **Bug**
- 💡 提出新 **Feature** 建议
- 📝 改进 **文档**
- 🔗 **分享** 给其他人

---

## 💬 社区

加入我们的社区，与其他开发者交流：

- **GitHub Discussions**: [参与讨论](https://github.com/RtimesC/OLED1/discussions)
- **Issues**: [问题反馈](https://github.com/RtimesC/OLED1/issues)

---

<div align="center">

**⭐ 如果这个项目对您有帮助，请给它一个 Star！⭐**

Made with ❤️ by [RtimesC](https://github.com/RtimesC)

---

**[⬆ 回到顶部](#oled1---stm32f10x-oled-display-driver)**

</div>

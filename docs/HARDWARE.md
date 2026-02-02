# 硬件连接指南

本文档详细说明 OLED1 项目的硬件要求、接线方法和配置说明。

## 📋 目录

- [开发板要求](#开发板要求)
- [OLED 模块规格](#oled-模块规格)
- [引脚连接](#引脚连接)
- [硬件接线图](#硬件接线图)
- [自定义引脚配置](#自定义引脚配置)
- [硬件注意事项](#硬件注意事项)
- [调试工具](#调试工具)
- [常见问题排查](#常见问题排查)
- [扩展功能](#扩展功能)
- [电路原理图](#电路原理图)

---

## 🔌 开发板要求

### 推荐开发板

| 型号 | 说明 | 推荐度 |
|------|------|--------|
| **STM32F103C8T6 最小系统板** | 蓝色核心板，64KB Flash | ⭐⭐⭐⭐⭐ |
| STM32F103RCT6 开发板 | 256KB Flash，引脚更多 | ⭐⭐⭐⭐ |
| STM32F103ZET6 开发板 | 512KB Flash，适合大型项目 | ⭐⭐⭐ |

### 最低硬件要求

- **MCU**: STM32F10x 系列（Cortex-M3 内核）
- **Flash**: ≥ 32KB（程序大小约 10KB）
- **RAM**: ≥ 10KB
- **系统时钟**: 建议 72MHz（可调整）
- **可用 GPIO**: 至少 2 个（I2C 引脚）

### 兼容芯片列表

✅ **完全兼容**：
- STM32F103C6T6 (32KB Flash)
- STM32F103C8T6 (64KB Flash) - **推荐**
- STM32F103CBT6 (128KB Flash)
- STM32F103RBT6 (128KB Flash)
- STM32F103RCT6 (256KB Flash)
- STM32F103VCT6 (256KB Flash)
- STM32F103ZET6 (512KB Flash)

⚠️ **需要修改**：
- 其他 STM32 系列（F0/F2/F4/F7）需修改外设库

---

## 📺 OLED 模块规格

### 推荐 OLED 模块

**0.96 寸 I2C OLED 显示屏**

| 参数 | 规格 |
|------|------|
| **尺寸** | 0.96 英寸 |
| **分辨率** | 128 × 64 像素 |
| **显示颜色** | 单色（白光/蓝光/黄蓝双色） |
| **驱动芯片** | SSD1306 |
| **通信接口** | I2C（4 线接口） |
| **工作电压** | 3.3V - 5V（建议 3.3V） |
| **工作电流** | 约 20mA（全亮） |
| **视角** | >160° |
| **工作温度** | -30°C ~ +70°C |

### 模块引脚说明

| 引脚 | 名称 | 说明 |
|------|------|------|
| **GND** | 电源地 | 连接到 STM32 的 GND |
| **VCC** | 电源正 | 接 3.3V 或 5V（推荐 3.3V） |
| **SCL** | I2C 时钟 | I2C 时钟信号线（连接 STM32 GPIO） |
| **SDA** | I2C 数据 | I2C 数据信号线（连接 STM32 GPIO） |

### I2C 地址

- **7 位地址**: `0x3C`
- **8 位地址**: `0x78`（写地址，程序中使用）

---

## 🔗 引脚连接

### 默认连接表

| OLED 引脚 | STM32 引脚 | 说明 |
|-----------|-----------|------|
| **VCC** | 3.3V | OLED 电源（推荐 3.3V） |
| **GND** | GND | 接地 |
| **SCL** | **PB8** | I2C 时钟线（可自定义） |
| **SDA** | **PB9** | I2C 数据线（可自定义） |

### 引脚功能说明

1. **VCC 和 GND**
   - 提供 OLED 工作电源
   - **推荐使用 3.3V**（部分模块支持 5V）
   - 确保电源稳定，必要时加 0.1uF 去耦电容

2. **SCL（时钟线）**
   - I2C 时钟信号
   - 配置为开漏输出（Open-Drain）
   - 需要上拉电阻（通常模块自带）

3. **SDA（数据线）**
   - I2C 数据信号
   - 配置为开漏输出（Open-Drain）
   - 需要上拉电阻（通常模块自带）

### 上拉电阻

- **推荐值**: 4.7kΩ（常用）或 10kΩ
- **位置**: SCL 和 SDA 各需一个上拉到 VCC
- **注意**: 大多数 OLED 模块已自带上拉电阻，无需额外添加

---

## 🖼️ 硬件接线图

### 基础接线图（ASCII 艺术）

```
    STM32F103C8T6            0.96" OLED (I2C)
  +----------------+        +------------------+
  |                |        |                  |
  |            3.3V|--------|VCC               |
  |                |        |                  |
  |             GND|--------|GND               |
  |                |        |                  |
  |     PB8 (GPIO) |--------|SCL (时钟)        |
  |                |        |                  |
  |     PB9 (GPIO) |--------|SDA (数据)        |
  |                |        |                  |
  +----------------+        +------------------+
```

### 实物接线示意图

```
STM32 开发板 (蓝色核心板)           OLED 模块
    [USB口]                       [显示屏]
      |||                            ||||
   +--------+                      +------+
   | 3V3 ●  |                      | GND  |
   | GND ●  |                      | VCC  |
   | PB8 ●  |                      | SCL  |
   | PB9 ●  |                      | SDA  |
   +--------+                      +------+
      |||                            ||||
      |||                            ||||
      ||+----------------------------+|||  (杜邦线连接)
      |+------------------------------+||
      +--------------------------------+|
       +---------------------------------+

连接说明：
  STM32 3V3  →  OLED VCC  (红线)
  STM32 GND  →  OLED GND  (黑线)
  STM32 PB8  →  OLED SCL  (黄线)
  STM32 PB9  →  OLED SDA  (绿线)
```

### 面包板接线示例

```
                面包板
    +---------------------------+
    |  +-----+      +-------+   |
    |  |OLED |      |STM32  |   |
    |  |     |      |       |   |
    |  | VCC |------|  3.3V |   |
    |  | GND |------|  GND  |   |
    |  | SCL |------|  PB8  |   |
    |  | SDA |------|  PB9  |   |
    |  +-----+      +-------+   |
    |                           |
    +---------------------------+
```

---

## ⚙️ 自定义引脚配置

如果您的硬件使用不同的 I2C 引脚，需要修改代码。

### 步骤 1: 打开 OLED.c 文件

找到文件 `Hardware/OLED.c`，在文件顶部找到引脚定义：

```c
/*引脚配置*/
#define OLED_W_SCL(x)   GPIO_WriteBit(GPIOB, GPIO_Pin_8, (BitAction)(x))
#define OLED_W_SDA(x)   GPIO_WriteBit(GPIOB, GPIO_Pin_9, (BitAction)(x))
#define OLED_R_SDA()    GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_9)
```

### 步骤 2: 修改 GPIO 端口和引脚

**示例 1: 改为 PA6 和 PA7**

```c
/*引脚配置*/
#define OLED_W_SCL(x)   GPIO_WriteBit(GPIOA, GPIO_Pin_6, (BitAction)(x))
#define OLED_W_SDA(x)   GPIO_WriteBit(GPIOA, GPIO_Pin_7, (BitAction)(x))
#define OLED_R_SDA()    GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_7)
```

**示例 2: 改为 PC10 和 PC11**

```c
/*引脚配置*/
#define OLED_W_SCL(x)   GPIO_WriteBit(GPIOC, GPIO_Pin_10, (BitAction)(x))
#define OLED_W_SDA(x)   GPIO_WriteBit(GPIOC, GPIO_Pin_11, (BitAction)(x))
#define OLED_R_SDA()    GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_11)
```

### 步骤 3: 修改初始化函数

在同一文件中找到 `OLED_I2C_Init()` 函数，修改 GPIO 初始化：

**原代码（PB8/PB9）：**
```c
void OLED_I2C_Init(void)
{
    // 使能 GPIOB 时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);
    
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_OD;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    
    // 初始化 PB8 (SCL)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8;
    GPIO_Init(GPIOB, &GPIO_InitStructure);
    
    // 初始化 PB9 (SDA)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_9;
    GPIO_Init(GPIOB, &GPIO_InitStructure);
    
    OLED_W_SCL(1);
    OLED_W_SDA(1);
}
```

**修改为 PA6/PA7：**
```c
void OLED_I2C_Init(void)
{
    // 使能 GPIOA 时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_OD;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    
    // 初始化 PA6 (SCL)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6;
    GPIO_Init(GPIOA, &GPIO_InitStructure);
    
    // 初始化 PA7 (SDA)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_7;
    GPIO_Init(GPIOA, &GPIO_InitStructure);
    
    OLED_W_SCL(1);
    OLED_W_SDA(1);
}
```

### 引脚选择建议

**推荐引脚**：
- ✅ 5V 容忍引脚（如果使用 5V OLED）
- ✅ 不与其他外设冲突的引脚
- ✅ 支持重映射的 I2C 硬件引脚（方便日后升级为硬件 I2C）

**避免使用的引脚**：
- ❌ PA13/PA14（SWD 调试引脚）
- ❌ PA9/PA10（如需使用串口）
- ❌ 已被其他外设占用的引脚

### I2C 速度调整

修改 I2C 通信速度，可调整延时参数：

```c
/*I2C时序延时配置 - 根据实际OLED响应速度调整*/
#define I2C_DELAY_ITERATIONS    10  // 默认值

// 提高速度（减小延时）- 可能导致通信不稳定
#define I2C_DELAY_ITERATIONS    5   // 更快

// 降低速度（增大延时）- 提高稳定性
#define I2C_DELAY_ITERATIONS    20  // 更慢更稳定
```

**调整建议**：
- 短距离接线（<10cm）: 可用较小值（5-10）
- 长距离接线（>20cm）: 增大值（20-50）
- 遇到通信问题: 先尝试增大延时

---

## ⚠️ 硬件注意事项

### 1. 电源要求

✅ **正确做法**：
- 使用开发板的 **3.3V 输出**供电（推荐）
- 确保电源稳定，电流充足（至少 50mA）
- 在 VCC 引脚附近加 0.1uF 陶瓷电容（可选）

❌ **错误做法**：
- 不要直接连接 5V（可能损坏某些 OLED 模块）
- 避免电源波动过大
- 不要与大电流负载共用电源

### 2. 上拉电阻

✅ **正确配置**：
- 大多数 OLED 模块自带上拉电阻（4.7kΩ）
- 检查模块背面是否有标注
- 如模块无上拉，需外接 4.7kΩ 上拉电阻

❌ **常见错误**：
- 重复添加上拉电阻（导致上拉过强，速度下降）
- 上拉电阻过大（>10kΩ，信号不稳定）
- 上拉电阻过小（<1kΩ，功耗增大）

### 3. 接线长度

| 接线长度 | 说明 | 建议 |
|---------|------|------|
| **<10cm** | 短距离 | ✅ 最佳，无需特殊处理 |
| **10-30cm** | 中等距离 | ⚠️ 降低 I2C 速度，使用屏蔽线 |
| **>30cm** | 长距离 | ❌ 不推荐，需要降低速度并加强驱动 |

**长距离接线建议**：
- 使用屏蔽双绞线
- 增大 `I2C_DELAY_ITERATIONS` 参数
- 减小上拉电阻（改为 2.2kΩ）
- 考虑使用电平转换芯片

### 4. 静电防护

- OLED 显示屏对静电敏感
- 操作前先触摸接地物体释放静电
- 存储时使用防静电袋

### 5. 机械保护

- OLED 玻璃屏幕易碎，小心操作
- 避免按压显示区域
- 使用外壳或保护罩

---

## 🛠️ 调试工具

### 推荐工具

1. **ST-Link V2 调试器**
   - 用途：程序下载和调试
   - 价格：约 ￥20-50
   - 推荐度：⭐⭐⭐⭐⭐

2. **USB 转串口模块**
   - 用途：串口打印调试信息
   - 芯片：CH340G 或 CP2102
   - 推荐度：⭐⭐⭐⭐

3. **逻辑分析仪**
   - 用途：分析 I2C 通信波形
   - 型号：8 通道逻辑分析仪
   - 推荐度：⭐⭐⭐

4. **万用表**
   - 用途：检测电压、通断
   - 推荐度：⭐⭐⭐⭐⭐

### 调试步骤

**步骤 1: 检查硬件连接**
```bash
1. 使用万用表检查：
   - VCC 是否为 3.3V
   - GND 是否正确连接
   - SCL/SDA 是否与 GND 短路

2. 目视检查：
   - 杜邦线是否插紧
   - 引脚是否对应正确
```

**步骤 2: 测试基本功能**
```c
// 最小测试代码
int main(void)
{
    OLED_Init();             // 初始化
    OLED_ShowString(1, 1, "TEST");  // 显示测试文本
    while(1);
}
```

**步骤 3: 使用逻辑分析仪**
- 连接 SCL 和 SDA 到分析仪
- 观察 I2C 起始信号、地址、数据
- 检查是否有 ACK 应答

---

## ❓ 常见问题排查

### 问题 1: OLED 完全不显示（黑屏）

**可能原因**：
1. ❌ 电源未连接或电压不足
2. ❌ OLED 模块损坏
3. ❌ 引脚连接错误
4. ❌ 程序未正确初始化

**排查步骤**：
```
✓ 用万用表测量 OLED 的 VCC 引脚，确认有 3.3V 电压
✓ 检查 GND 是否连接
✓ 确认 SCL 和 SDA 连接正确（对照引脚表）
✓ 更换一个新的 OLED 模块测试
✓ 确认代码中调用了 OLED_Init()
```

### 问题 2: 显示内容乱码或花屏

**可能原因**：
1. ❌ I2C 通信速度过快
2. ❌ 接线过长或干扰严重
3. ❌ 上拉电阻配置不当

**解决方法**：
```c
// 方法 1: 降低 I2C 速度
#define I2C_DELAY_ITERATIONS    20  // 增大延时

// 方法 2: 缩短接线，使用屏蔽线

// 方法 3: 检查上拉电阻（应为 4.7kΩ）
```

### 问题 3: 显示不稳定，时有时无

**可能原因**：
1. ❌ 接线接触不良
2. ❌ 电源不稳定
3. ❌ 温度过高或过低

**解决方法**：
```
✓ 重新插拔所有连接线，确保接触良好
✓ 使用示波器检查电源波纹
✓ 加装去耦电容（0.1uF + 10uF）
✓ 焊接连接而不是使用杜邦线
```

### 问题 4: 部分内容不显示

**可能原因**：
1. ❌ 坐标超出范围
2. ❌ 字符串过长
3. ❌ 显示缓冲区问题

**检查代码**：
```c
// 错误示例
OLED_ShowString(1, 16, "AB");  // ❌ 只能显示 1 个字符

// 正确示例
OLED_ShowString(1, 15, "AB");  // ✅ 可以显示 2 个字符
```

### 问题 5: 编译报错

**常见错误及解决**：

```c
// 错误 1: 找不到头文件
#include "OLED.h"  // ❌ 路径错误

// 解决：检查文件路径配置
Options -> C/C++ -> Include Paths -> 添加 Hardware 目录

// 错误 2: 未定义的引用
undefined reference to 'OLED_Init'

// 解决：确保 OLED.c 已添加到工程
```

---

## 🔧 扩展功能

### 1. 连接多个 OLED

如果需要同时使用多个 OLED（需要支持不同 I2C 地址的模块）：

```c
// 方法 1: 使用不同的 GPIO 引脚
// 为每个 OLED 创建独立的驱动实例

// 方法 2: 使用 I2C 地址切换（需硬件支持）
// 某些 OLED 模块支持通过跳线修改地址
```

### 2. 使用硬件 I2C

当前使用软件模拟 I2C，可升级为硬件 I2C：

**优点**：
- ✅ 速度更快
- ✅ CPU 占用更低
- ✅ 支持 DMA

**缺点**：
- ❌ 引脚固定（需使用特定引脚）
- ❌ 代码复杂度增加

**硬件 I2C 引脚**：
- I2C1: PB6(SCL), PB7(SDA)
- I2C2: PB10(SCL), PB11(SDA)

### 3. 添加触摸功能

可搭配电容触摸芯片实现触摸交互：
- 推荐芯片：TTP223（单键）、TTP224（4 键）
- 连接方式：GPIO 输入

### 4. 显示图片

需要额外添加位图数据转换和显示函数：

```c
// 将 BMP 图片转换为数组
const uint8_t image[] = {
    0x00, 0x00, 0xFF, 0xFF, ...
};

// 显示位图（需自行实现）
void OLED_ShowImage(uint8_t x, uint8_t y, 
                    uint8_t width, uint8_t height, 
                    const uint8_t *image);
```

---

## 📐 电路原理图

### 最小系统原理图

```
                   +3.3V
                     |
                     |
                    +-+
                    | | 4.7kΩ (可选，模块通常自带)
                    +-+
                     |
          +----------+-----------+
          |                      |
          |                      |
        [SCL]                  [SDA]
          |                      |
          |                      |
     +----+----+            +----+----+
     |         |            |         |
     |  OLED   |            |  OLED   |
     | (SCL)   |            | (SDA)   |
     |         |            |         |
     |   [VCC] +---+   +----+  [GND]  |
     |         |   |   |    |         |
     +---------+   |   |    +---------+
                 +3.3V GND

STM32 侧：
  PB8 -----> SCL
  PB9 -----> SDA
  3.3V ----> VCC
  GND -----> GND
```

### 完整系统参考电路

```
                       STM32F103C8T6
                    +------------------+
                    |                  |
    +3.3V --------> | 3V3              |
                    |                  |
    GND ----------> | GND              |
                    |                  |
    (I2C)           |                  |
    SCL <---------- | PB8              |
    SDA <---------- | PB9              |
                    |                  |
    (UART - 可选)   |                  |
    TX  ----------> | PA9  (调试串口)  |
    RX  <---------- | PA10             |
                    |                  |
    (SWD 下载)      |                  |
    SWDIO <-------> | PA13             |
    SWCLK <-------- | PA14             |
                    |                  |
    (LED - 可选)    |                  |
    LED <---------- | PC13 (板载 LED)  |
                    |                  |
                    +------------------+
```

---

## 📚 参考资料

### 数据手册

1. **SSD1306 控制器数据手册**
   - 下载地址：[Solomon SSD1306 官方文档]
   - 内容：寄存器说明、命令集、时序图

2. **STM32F103 参考手册**
   - 下载地址：ST 官网
   - 内容：GPIO 配置、I2C 外设说明

### 推荐阅读

- I2C 总线协议规范
- OLED 显示原理
- STM32 标准外设库使用手册

---

## 💡 小贴士

1. **首次使用建议先用最简代码测试**
   ```c
   OLED_Init();
   OLED_ShowString(1, 1, "OK");
   ```

2. **使用示波器或逻辑分析仪观察 I2C 波形可快速定位问题**

3. **长距离传输或环境干扰严重时，适当降低 I2C 速度**

4. **定期清洁 OLED 屏幕，避免灰尘影响显示效果**

5. **保存好工作电路，便于日后快速搭建**

---

## 🔗 相关文档

- [README.md](../README.md) - 项目总览
- [API.md](API.md) - API 参考文档
- [CONTRIBUTING.md](../CONTRIBUTING.md) - 贡献指南

---

**最后更新：** 2026-02-02  
**版本：** 1.0.0  
**作者：** RtimesC

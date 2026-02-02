# API 参考文档

本文档详细描述了 OLED1 项目中所有公开的 API 函数。

## 📋 目录

- [OLED 驱动 API](#oled-驱动-api)
  - [OLED_Init](#oled_init)
  - [OLED_Clear](#oled_clear)
  - [OLED_ShowChar](#oled_showchar)
  - [OLED_ShowString](#oled_showstring)
  - [OLED_ShowNum](#oled_shownum)
  - [OLED_ShowSignedNum](#oled_showsignednum)
  - [OLED_ShowHexNum](#oled_showhexnum)
  - [OLED_ShowBinNum](#oled_showbinnum)
  - [OLED_SetCursor](#oled_setcursor)
  - [OLED_WriteData](#oled_writedata)
  - [OLED_WriteData_Multi](#oled_writedata_multi)
- [延时函数 API](#延时函数-api)
  - [Delay_us](#delay_us)
  - [Delay_ms](#delay_ms)
  - [Delay_s](#delay_s)
- [内部函数](#内部函数)
  - [OLED_I2C_Init](#oled_i2c_init)
  - [OLED_WriteCommand](#oled_writecommand)

---

## OLED 驱动 API

### OLED_Init

**函数原型：**
```c
void OLED_Init(void);
```

**功能说明：**

初始化 OLED 显示屏，包括 I2C 通信接口和 OLED 控制器（SSD1306）的配置。

**参数说明：**
- 无参数

**返回值：**
- 无返回值

**使用说明：**

1. 在使用任何 OLED 显示功能前，必须先调用此函数
2. 初始化过程包括：
   - 配置 I2C 引脚（PB8-SCL, PB9-SDA）
   - 发送 SSD1306 初始化命令序列
   - 清空显示缓存
   - 开启显示

**注意事项：**
- 确保 OLED 模块已正确连接
- 建议在 main 函数开始处调用
- 系统时钟需要已配置（SysTick 用于延时）

**使用示例：**
```c
#include "OLED.h"

int main(void)
{
    // 初始化 OLED
    OLED_Init();
    
    // 显示内容
    OLED_ShowString(1, 1, "Hello World!");
    
    while(1)
    {
        // 主循环
    }
}
```

---

### OLED_Clear

**函数原型：**
```c
void OLED_Clear(void);
```

**功能说明：**

清空 OLED 显示屏的所有内容，将显示区域全部填充为黑色（熄灭）。

**参数说明：**
- 无参数

**返回值：**
- 无返回值

**使用说明：**

1. 清空整个 128x64 像素的显示区域
2. 清屏后光标位置重置到 (0, 0)
3. 执行时间约 10-20ms（取决于 I2C 速度）

**注意事项：**
- 调用前需要先初始化 OLED
- 频繁调用可能导致闪烁
- 如果只需要更新部分内容，建议直接覆盖写入

**使用示例：**
```c
// 清空显示
OLED_Clear();

// 显示新内容
OLED_ShowString(1, 1, "New Content");
```

---

### OLED_ShowChar

**函数原型：**
```c
void OLED_ShowChar(uint8_t Line, uint8_t Column, char Char);
```

**功能说明：**

在 OLED 显示屏的指定位置显示单个 ASCII 字符。

**参数说明：**

| 参数 | 类型 | 说明 | 范围 |
|------|------|------|------|
| Line | uint8_t | 行号（从上到下） | 1-4 |
| Column | uint8_t | 列号（从左到右） | 1-16 |
| Char | char | 要显示的字符 | ASCII 可打印字符（32-126） |

**返回值：**
- 无返回值

**使用说明：**

1. 字符使用 8x16 点阵字体，每个字符占用 2 行显示空间
2. Line 参数决定字符显示在第几行（1-4 行）
3. Column 参数决定字符在该行的位置（1-16 列）
4. 超出屏幕范围的字符不会显示

**坐标系统：**
```
    Column: 1  2  3  4  5  ... 16
Line 1:     A  B  C  D  E  ... P
Line 2:     
Line 3:     
Line 4:     
```

**注意事项：**
- 字符必须是 ASCII 可打印字符
- 坐标从 1 开始，不是从 0 开始
- 中文字符不支持（需要额外的字库）

**使用示例：**
```c
// 在第 1 行第 1 列显示字符 'A'
OLED_ShowChar(1, 1, 'A');

// 在第 2 行第 5 列显示字符 '@'
OLED_ShowChar(2, 5, '@');

// 在第 4 行第 16 列显示字符 'Z'
OLED_ShowChar(4, 16, 'Z');
```

---

### OLED_ShowString

**函数原型：**
```c
void OLED_ShowString(uint8_t Line, uint8_t Column, char *String);
```

**功能说明：**

在 OLED 显示屏的指定位置显示字符串。

**参数说明：**

| 参数 | 类型 | 说明 | 范围 |
|------|------|------|------|
| Line | uint8_t | 起始行号 | 1-4 |
| Column | uint8_t | 起始列号 | 1-16 |
| String | char* | 要显示的字符串（以 '\0' 结尾） | ASCII 字符串 |

**返回值：**
- 无返回值

**使用说明：**

1. 从指定位置开始，向右依次显示字符串中的每个字符
2. 字符串必须以 '\0' 结尾
3. 超出屏幕宽度的字符不会自动换行
4. 建议字符串长度不超过 (17 - Column) 个字符

**最大字符数：**
```
Column 1:  最多 16 个字符
Column 8:  最多 9 个字符
Column 16: 最多 1 个字符
```

**注意事项：**
- 不支持中文字符
- 不会自动换行，超出部分不显示
- 确保字符串有正确的 '\0' 结尾

**使用示例：**
```c
// 在第 1 行第 1 列显示 "Hello"
OLED_ShowString(1, 1, "Hello");

// 在第 2 行第 5 列显示 "STM32"
OLED_ShowString(2, 5, "STM32");

// 在第 3 行第 1 列显示 "Temperature:25C"
OLED_ShowString(3, 1, "Temperature:25C");

// 显示变量内容
char buffer[20];
sprintf(buffer, "Voltage: %.2fV", voltage);
OLED_ShowString(4, 1, buffer);
```

---

### OLED_ShowNum

**函数原型：**
```c
void OLED_ShowNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
```

**功能说明：**

在 OLED 显示屏的指定位置显示无符号十进制数字。

**参数说明：**

| 参数 | 类型 | 说明 | 范围 |
|------|------|------|------|
| Line | uint8_t | 行号 | 1-4 |
| Column | uint8_t | 列号 | 1-16 |
| Number | uint32_t | 要显示的数字 | 0 - 4294967295 |
| Length | uint8_t | 显示位数（不足补前导零） | 1-10 |

**返回值：**
- 无返回值

**使用说明：**

1. 以十进制格式显示无符号整数
2. Length 参数指定显示的位数：
   - 如果 Number 位数不足，前面补 0
   - 如果 Number 位数超过 Length，只显示低位
3. 常用于显示传感器数值、计数器等

**显示格式：**
```
OLED_ShowNum(1, 1, 123, 5)    -> "00123"
OLED_ShowNum(1, 1, 456, 3)    -> "456"
OLED_ShowNum(1, 1, 78, 2)     -> "78"
OLED_ShowNum(1, 1, 9, 4)      -> "0009"
OLED_ShowNum(1, 1, 12345, 3)  -> "345" (只显示低3位)
```

**注意事项：**
- 仅支持无符号整数（0 及正数）
- 显示负数请使用 OLED_ShowSignedNum()
- Length 不要超过 (17 - Column)

**使用示例：**
```c
// 显示三位数字（不足补零）
OLED_ShowNum(1, 1, 5, 3);      // 显示 "005"

// 显示传感器数值
uint32_t temperature = 25;
OLED_ShowNum(2, 1, temperature, 2);  // 显示 "25"

// 显示计数器
uint32_t counter = 12345;
OLED_ShowNum(3, 1, counter, 5);      // 显示 "12345"

// 动态更新数值
for (uint32_t i = 0; i < 100; i++) {
    OLED_ShowNum(4, 1, i, 3);
    Delay_ms(100);
}
```

---

### OLED_ShowSignedNum

**函数原型：**
```c
void OLED_ShowSignedNum(uint8_t Line, uint8_t Column, int32_t Number, uint8_t Length);
```

**功能说明：**

在 OLED 显示屏的指定位置显示有符号十进制数字（可以显示负数）。

**参数说明：**

| 参数 | 类型 | 说明 | 范围 |
|------|------|------|------|
| Line | uint8_t | 行号 | 1-4 |
| Column | uint8_t | 列号 | 1-16 |
| Number | int32_t | 要显示的数字（可以是负数） | -2147483648 ~ 2147483647 |
| Length | uint8_t | 数值部分的显示位数 | 1-10 |

**返回值：**
- 无返回值

**使用说明：**

1. 可以显示正数和负数
2. 负数会在前面显示负号 '-'
3. Length 参数指定数值部分的位数（不包括负号）
4. 正数前面不会显示 '+' 号

**显示格式：**
```
OLED_ShowSignedNum(1, 1, 123, 5)   -> " 00123" (正数，前面是空格)
OLED_ShowSignedNum(1, 1, -456, 5)  -> "-00456" (负数，前面是负号)
OLED_ShowSignedNum(1, 1, -78, 3)   -> "-078"
OLED_ShowSignedNum(1, 1, 0, 4)     -> " 0000"
```

**注意事项：**
- 负号会占用一个字符位置
- 实际占用宽度 = Length + 1（包括符号位）
- 确保 Column + Length < 17

**使用示例：**
```c
// 显示温度（可能为负值）
int32_t temperature = -15;
OLED_ShowSignedNum(1, 1, temperature, 3);  // 显示 "-015"

// 显示高度变化（正负）
int32_t altitude = 120;
OLED_ShowString(2, 1, "Alt:");
OLED_ShowSignedNum(2, 5, altitude, 4);     // 显示 " 0120"

// 显示电压差
int32_t voltage_diff = -5;
OLED_ShowString(3, 1, "Diff:");
OLED_ShowSignedNum(3, 6, voltage_diff, 2); // 显示 "-05"

// 实时更新（从负数到正数）
for (int32_t i = -50; i <= 50; i++) {
    OLED_ShowSignedNum(4, 1, i, 3);
    Delay_ms(50);
}
```

---

### OLED_ShowHexNum

**函数原型：**
```c
void OLED_ShowHexNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
```

**功能说明：**

在 OLED 显示屏的指定位置显示十六进制数字。

**参数说明：**

| 参数 | 类型 | 说明 | 范围 |
|------|------|------|------|
| Line | uint8_t | 行号 | 1-4 |
| Column | uint8_t | 列号 | 1-16 |
| Number | uint32_t | 要显示的数字 | 0 - 0xFFFFFFFF |
| Length | uint8_t | 显示位数（十六进制位） | 1-8 |

**返回值：**
- 无返回值

**使用说明：**

1. 以十六进制格式显示无符号整数
2. 字母使用大写（A-F）
3. Length 参数指定显示的十六进制位数
4. 不足位数前面补 0
5. 常用于显示内存地址、寄存器值等

**显示格式：**
```
OLED_ShowHexNum(1, 1, 0x1A, 2)       -> "1A"
OLED_ShowHexNum(1, 1, 0xFF, 4)       -> "00FF"
OLED_ShowHexNum(1, 1, 0xABCD, 4)     -> "ABCD"
OLED_ShowHexNum(1, 1, 0x12345, 8)    -> "00012345"
```

**注意事项：**
- 仅支持无符号整数
- 最多显示 8 位十六进制（32 位整数）
- 不会显示 "0x" 前缀，如需要请单独显示

**使用示例：**
```c
// 显示内存地址
uint32_t addr = 0x08000000;
OLED_ShowString(1, 1, "0x");
OLED_ShowHexNum(1, 3, addr, 8);  // 显示 "08000000"

// 显示寄存器值
uint16_t reg_value = 0x1A2B;
OLED_ShowString(2, 1, "REG:");
OLED_ShowHexNum(2, 5, reg_value, 4);  // 显示 "1A2B"

// 显示 ADC 采样值（十六进制）
uint32_t adc_data = 0xFFF;
OLED_ShowHexNum(3, 1, adc_data, 3);   // 显示 "FFF"

// 调试显示（显示变量的十六进制值）
uint8_t status = 0xA5;
OLED_ShowString(4, 1, "Status:0x");
OLED_ShowHexNum(4, 10, status, 2);    // 显示 "A5"
```

---

### OLED_ShowBinNum

**函数原型：**
```c
void OLED_ShowBinNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
```

**功能说明：**

在 OLED 显示屏的指定位置显示二进制数字。

**参数说明：**

| 参数 | 类型 | 说明 | 范围 |
|------|------|------|------|
| Line | uint8_t | 行号 | 1-4 |
| Column | uint8_t | 列号 | 1-16 |
| Number | uint32_t | 要显示的数字 | 0 - 0xFFFFFFFF |
| Length | uint8_t | 显示位数（二进制位） | 1-16 |

**返回值：**
- 无返回值

**使用说明：**

1. 以二进制格式显示无符号整数
2. 只显示 0 和 1
3. Length 参数指定显示的二进制位数
4. 不足位数前面补 0
5. 常用于显示位状态、标志位等

**显示格式：**
```
OLED_ShowBinNum(1, 1, 5, 4)        -> "0101"
OLED_ShowBinNum(1, 1, 15, 8)       -> "00001111"
OLED_ShowBinNum(1, 1, 255, 8)      -> "11111111"
OLED_ShowBinNum(1, 1, 0xA5, 8)     -> "10100101"
```

**注意事项：**
- 仅支持无符号整数
- Length 最大为 16（受屏幕宽度限制）
- 不会显示 "0b" 前缀，如需要请单独显示
- 显示较长的二进制数时，注意屏幕宽度限制

**使用示例：**
```c
// 显示 8 位二进制数
uint8_t data = 0b10110010;
OLED_ShowString(1, 1, "0b");
OLED_ShowBinNum(1, 3, data, 8);  // 显示 "10110010"

// 显示状态标志位
uint8_t flags = 0x0F;  // 0b00001111
OLED_ShowBinNum(2, 1, flags, 8); // 显示 "00001111"

// 显示端口输入状态
uint8_t port_status = GPIOA->IDR & 0xFF;
OLED_ShowBinNum(3, 1, port_status, 8);

// 实时监控某个位的变化
for (uint8_t i = 0; i < 16; i++) {
    OLED_ShowBinNum(4, 1, i, 4);
    Delay_ms(100);
}
```

---

### OLED_SetCursor

**函数原型：**
```c
void OLED_SetCursor(uint8_t Y, uint8_t X);
```

**功能说明：**

设置 OLED 显示的光标位置，用于后续的写操作。

**参数说明：**

| 参数 | 类型 | 说明 | 范围 |
|------|------|------|------|
| Y | uint8_t | 页地址（垂直方向） | 0-7 |
| X | uint8_t | 列地址（水平方向） | 0-127 |

**返回值：**
- 无返回值

**使用说明：**

1. Y 参数对应 OLED 的页地址（Page Address）
   - 每页高度为 8 像素
   - 总共 8 页（64 像素 ÷ 8）
2. X 参数对应列地址（Column Address）
   - 范围 0-127（总共 128 列）
3. 主要供内部函数使用，用户一般不直接调用

**坐标系统：**
```
        X (Column): 0 ~ 127
      +---------------------------+
  Y 0 |                           |  (8 pixels)
  (P  |                           |
  a 1 |                           |  (8 pixels)
  g   |                           |
  e 2 |                           |  (8 pixels)
  )   |                           |
    3 |                           |  (8 pixels)
      |                           |
    4 |                           |  (8 pixels)
      |                           |
    5 |                           |  (8 pixels)
      |                           |
    6 |                           |  (8 pixels)
      |                           |
    7 |                           |  (8 pixels)
      +---------------------------+
```

**注意事项：**
- Y 的单位是页（8 像素），不是单个像素
- 一般情况下，使用高层 API（如 OLED_ShowChar）即可，不需要直接调用此函数
- 自定义绘图时可能需要使用此函数

**使用示例：**
```c
// 设置到第 0 页，第 0 列
OLED_SetCursor(0, 0);

// 设置到第 2 页，第 64 列（屏幕中间）
OLED_SetCursor(2, 64);

// 配合 OLED_WriteData 使用
OLED_SetCursor(0, 0);
OLED_WriteData(0xFF);  // 写入一个字节的数据
```

---

### OLED_WriteData

**函数原型：**
```c
void OLED_WriteData(uint8_t Data);
```

**功能说明：**

向 OLED 显示RAM 写入一个字节的数据。

**参数说明：**

| 参数 | 类型 | 说明 |
|------|------|------|
| Data | uint8_t | 要写入的数据（8 位，对应垂直 8 个像素） |

**返回值：**
- 无返回值

**使用说明：**

1. 数据的每一位对应一个像素：
   - Bit 0 (LSB): 当前页的第 1 行像素
   - Bit 1: 当前页的第 2 行像素
   - ...
   - Bit 7 (MSB): 当前页的第 8 行像素
2. 位值 1 表示点亮像素，0 表示熄灭像素
3. 写入后光标自动右移一列
4. 一般用于底层绘图或显示自定义图形

**数据格式：**
```
Data = 0b10101010 (0xAA)
显示效果（垂直方向）:
  ● (Bit 7)
  ○ (Bit 6)
  ● (Bit 5)
  ○ (Bit 4)
  ● (Bit 3)
  ○ (Bit 2)
  ● (Bit 1)
  ○ (Bit 0)
```

**注意事项：**
- 写入前需要先用 OLED_SetCursor() 设置位置
- 主要供内部函数和高级用户使用
- 一般用户使用高层 API 即可

**使用示例：**
```c
// 在第 0 页，第 0 列画一条竖线（8 像素）
OLED_SetCursor(0, 0);
OLED_WriteData(0xFF);  // 0b11111111，所有位都亮

// 画一个简单的图案
OLED_SetCursor(1, 10);
OLED_WriteData(0x0F);  // 0b00001111，下半部分亮

// 清除一列
OLED_SetCursor(2, 20);
OLED_WriteData(0x00);  // 0b00000000，全部熄灭
```

---

### OLED_WriteData_Multi

**函数原型：**
```c
void OLED_WriteData_Multi(uint8_t *pData, uint8_t Length);
```

**功能说明：**

向 OLED 显示RAM 连续写入多个字节的数据。

**参数说明：**

| 参数 | 类型 | 说明 |
|------|------|------|
| pData | uint8_t* | 指向数据缓冲区的指针 |
| Length | uint8_t | 要写入的字节数 |

**返回值：**
- 无返回值

**使用说明：**

1. 连续写入多个字节，比多次调用 OLED_WriteData() 更高效
2. 写入后光标自动右移 Length 列
3. 主要用于显示图片、图标等大块数据
4. 写入前需要先用 OLED_SetCursor() 设置起始位置

**注意事项：**
- 确保数据缓冲区有足够的长度
- 不要超出屏幕边界（X + Length <= 128）
- 主要供内部函数和高级用户使用

**使用示例：**
```c
// 显示一个 8x8 的图标
const uint8_t icon[8] = {
    0x3C, 0x42, 0x81, 0x81, 0x81, 0x81, 0x42, 0x3C
};
OLED_SetCursor(0, 0);
OLED_WriteData_Multi((uint8_t*)icon, 8);

// 显示一行点阵数据
uint8_t line_data[128];
memset(line_data, 0xFF, 128);  // 填充为全亮
OLED_SetCursor(3, 0);
OLED_WriteData_Multi(line_data, 128);
```

---

## 延时函数 API

### Delay_us

**函数原型：**
```c
void Delay_us(uint32_t us);
```

**功能说明：**

提供微秒级的延时功能，基于 SysTick 定时器实现。

**参数说明：**

| 参数 | 类型 | 说明 | 范围 |
|------|------|------|------|
| us | uint32_t | 延时时间（微秒） | 建议 1 - 1000000 |

**返回值：**
- 无返回值

**使用说明：**

1. 使用 SysTick 定时器实现精确延时
2. 延时精度受系统时钟频率影响
3. 对于 STM32F103（72MHz），精度约为 1us
4. 延时期间 CPU 处于忙等待状态

**注意事项：**
- 延时期间会阻塞程序执行
- 不适合需要后台任务的场合
- 中断可能影响延时精度
- 极短的延时（<10us）可能不准确

**使用示例：**
```c
// 延时 100 微秒
Delay_us(100);

// 产生 PWM 波形（软件 PWM）
while (1) {
    GPIO_SetBits(GPIOA, GPIO_Pin_0);
    Delay_us(500);   // 高电平 500us
    GPIO_ResetBits(GPIOA, GPIO_Pin_0);
    Delay_us(500);   // 低电平 500us
}

// I2C 时序延时
void I2C_SendByte(uint8_t data) {
    for (uint8_t i = 0; i < 8; i++) {
        // 设置数据线
        Delay_us(2);  // 建立时间
        // 产生时钟
        Delay_us(5);  // 时钟高电平
    }
}
```

---

### Delay_ms

**函数原型：**
```c
void Delay_ms(uint32_t ms);
```

**功能说明：**

提供毫秒级的延时功能，基于 SysTick 定时器实现。

**参数说明：**

| 参数 | 类型 | 说明 | 范围 |
|------|------|------|------|
| ms | uint32_t | 延时时间（毫秒） | 建议 1 - 60000 |

**返回值：**
- 无返回值

**使用说明：**

1. 使用 SysTick 定时器实现精确延时
2. 内部调用 Delay_us(ms * 1000)
3. 适用于一般的延时需求
4. 延时期间 CPU 处于忙等待状态

**注意事项：**
- 延时期间会阻塞程序执行
- 长时间延时会影响系统响应性
- 需要响应中断的场合建议使用定时器
- 看门狗开启时注意延时不要过长

**使用示例：**
```c
// 延时 500 毫秒
Delay_ms(500);

// LED 闪烁
while (1) {
    GPIO_SetBits(GPIOC, GPIO_Pin_13);    // 点亮 LED
    Delay_ms(500);                       // 延时 500ms
    GPIO_ResetBits(GPIOC, GPIO_Pin_13);  // 熄灭 LED
    Delay_ms(500);                       // 延时 500ms
}

// 按键消抖
uint8_t key_scan(void) {
    if (GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0) == 0) {
        Delay_ms(10);  // 消抖延时
        if (GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0) == 0) {
            return 1;  // 确认按键按下
        }
    }
    return 0;
}

// 显示更新
for (uint8_t i = 0; i < 100; i++) {
    OLED_ShowNum(1, 1, i, 3);
    Delay_ms(100);  // 每 100ms 更新一次
}
```

---

### Delay_s

**函数原型：**
```c
void Delay_s(uint32_t s);
```

**功能说明：**

提供秒级的延时功能，基于 SysTick 定时器实现。

**参数说明：**

| 参数 | 类型 | 说明 | 范围 |
|------|------|------|------|
| s | uint32_t | 延时时间（秒） | 建议 1 - 3600 |

**返回值：**
- 无返回值

**使用说明：**

1. 使用 SysTick 定时器实现精确延时
2. 内部调用 Delay_ms(s * 1000)
3. 适用于较长时间的延时需求
4. 延时期间 CPU 处于忙等待状态

**注意事项：**
- 延时期间会阻塞程序执行
- 不推荐使用过长的延时（>60秒）
- 看门狗开启时要注意喂狗
- 需要长时间定时建议使用 RTC 或定时器

**使用示例：**
```c
// 延时 5 秒
Delay_s(5);

// 开机延时
int main(void) {
    SystemInit();
    OLED_Init();
    OLED_ShowString(1, 1, "Booting...");
    Delay_s(2);  // 开机延时 2 秒
    OLED_Clear();
    // 进入主程序
}

// 定时任务（简单实现）
while (1) {
    // 执行任务
    read_sensor();
    update_display();
    
    // 等待下一周期
    Delay_s(10);  // 每 10 秒执行一次
}
```

---

## 内部函数

以下函数主要供库内部使用，一般用户无需直接调用。

### OLED_I2C_Init

**函数原型：**
```c
static void OLED_I2C_Init(void);
```

**功能说明：**

初始化 I2C 通信接口（软件模拟 I2C）。

**使用说明：**
- 由 OLED_Init() 自动调用
- 配置 PB8（SCL）和 PB9（SDA）为开漏输出
- 设置初始电平为高

---

### OLED_WriteCommand

**函数原型：**
```c
static void OLED_WriteCommand(uint8_t Command);
```

**功能说明：**

向 OLED 控制器发送命令字节。

**参数说明：**
- Command: 命令字节（SSD1306 命令集）

**使用说明：**
- 由库内部函数调用
- 用于配置 OLED 控制器的各种参数

---

## 📝 完整使用示例

### 基础示例

```c
#include "stm32f10x.h"
#include "OLED.h"
#include "Delay.h"

int main(void)
{
    // 初始化 OLED
    OLED_Init();
    
    // 显示欢迎信息
    OLED_ShowString(1, 1, "  OLED Demo  ");
    OLED_ShowString(2, 1, "STM32F103C8T6");
    
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

### 动态显示示例

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
    OLED_ShowString(1, 1, "Counter Test");
    
    while (1)
    {
        // 更新计数器显示
        OLED_ShowNum(2, 1, counter, 6);
        counter++;
        
        // 延时
        Delay_ms(100);
    }
}
```

### 传感器数据显示示例

```c
#include "stm32f10x.h"
#include "OLED.h"
#include "Delay.h"

int main(void)
{
    int32_t temperature = 0;
    uint32_t humidity = 0;
    
    // 初始化
    OLED_Init();
    
    // 显示标题
    OLED_ShowString(1, 1, "Sensor Data");
    OLED_ShowString(2, 1, "Temp:");
    OLED_ShowString(3, 1, "Humi:");
    
    while (1)
    {
        // 读取传感器数据（示例）
        temperature = 25;  // 实际应从传感器读取
        humidity = 60;
        
        // 显示温度（支持负数）
        OLED_ShowSignedNum(2, 6, temperature, 3);
        OLED_ShowString(2, 10, "C");
        
        // 显示湿度
        OLED_ShowNum(3, 6, humidity, 3);
        OLED_ShowString(3, 10, "%");
        
        // 延时
        Delay_ms(1000);
    }
}
```

---

## 🔍 常见问题

### Q1: 调用 API 后 OLED 不显示？

**A:** 检查以下几点：
1. 是否调用了 `OLED_Init()`
2. 硬件连接是否正确（参见 [HARDWARE.md](HARDWARE.md)）
3. I2C 引脚配置是否正确（默认 PB8-SCL, PB9-SDA）
4. 电源是否正常（3.3V）

### Q2: 显示内容乱码？

**A:** 可能原因：
1. I2C 通信异常（检查上拉电阻、接线长度）
2. 字符超出 ASCII 范围（32-126）
3. OLED 模块不兼容（确认使用 SSD1306 控制器）

### Q3: 如何提高显示更新速度？

**A:** 
1. 只更新需要改变的部分，避免频繁调用 `OLED_Clear()`
2. 使用 `OLED_ShowNum()` 等函数直接覆盖旧内容
3. 调整 I2C 速度（修改 `I2C_DELAY_ITERATIONS` 宏）

### Q4: 可以同时使用多个 OLED 吗？

**A:** 
需要修改代码以支持不同的 I2C 地址或引脚。当前版本支持单个 OLED。

### Q5: 如何显示中文？

**A:** 
需要添加中文字库（较大），当前版本仅支持 ASCII 字符。

---

## 📚 相关文档

- [README.md](../README.md) - 项目总览
- [HARDWARE.md](HARDWARE.md) - 硬件连接指南
- [CONTRIBUTING.md](../CONTRIBUTING.md) - 贡献指南

---

**最后更新：** 2026-02-02  
**版本：** 1.0.0  
**作者：** RtimesC

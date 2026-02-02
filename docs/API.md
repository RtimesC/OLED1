# API 参考文档

本文档详细说明了 OLED1 项目中所有模块的 API 接口。

## 目录

- [OLED 模块](#oled-模块)
- [LED 模块](#led-模块)
- [Key 模块](#key-模块)

---

## OLED 模块

OLED 模块提供了 SSD1306 控制器的完整驱动功能，支持字符、字符串和数字的显示。

### 头文件

```c
#include "OLED.h"
```

### 函数列表

#### OLED_Init

**函数原型**

```c
void OLED_Init(void);
```

**功能描述**

初始化 OLED 显示屏，配置 I2C 通信和显示参数。

**参数**

- 无

**返回值**

- 无

**使用示例**

```c
int main(void)
{
    OLED_Init();  // 初始化 OLED
    // 其他代码...
}
```

**注意事项**

- 必须在使用其他 OLED 函数之前调用此函数
- 确保 I2C 引脚已正确连接

---

#### OLED_Clear

**函数原型**

```c
void OLED_Clear(void);
```

**功能描述**

清除 OLED 显示屏上的所有内容。

**参数**

- 无

**返回值**

- 无

**使用示例**

```c
OLED_Clear();  // 清屏
```

---

#### OLED_ShowChar

**函数原型**

```c
void OLED_ShowChar(uint8_t Line, uint8_t Column, char Char);
```

**功能描述**

在指定位置显示单个字符。

**参数**

- `Line`: 行号，范围 1-4
- `Column`: 列号，范围 1-16
- `Char`: 要显示的字符（ASCII）

**返回值**

- 无

**使用示例**

```c
OLED_ShowChar(1, 1, 'A');  // 在第1行第1列显示字符 'A'
OLED_ShowChar(2, 5, 'B');  // 在第2行第5列显示字符 'B'
```

**注意事项**

- 超出范围的坐标可能导致显示异常
- 字符必须在 ASCII 码表范围内

---

#### OLED_ShowString

**函数原型**

```c
void OLED_ShowString(uint8_t Line, uint8_t Column, char *String);
```

**功能描述**

在指定位置显示字符串。

**参数**

- `Line`: 行号，范围 1-4
- `Column`: 列号，范围 1-16
- `String`: 要显示的字符串指针（以 '\0' 结尾）

**返回值**

- 无

**使用示例**

```c
OLED_ShowString(1, 1, "Hello");
OLED_ShowString(2, 1, "OLED Display");
OLED_ShowString(3, 1, "STM32");
```

**注意事项**

- 字符串超出行宽时会自动换行或截断
- 确保字符串以空字符 '\0' 结尾

---

#### OLED_ShowNum

**函数原型**

```c
void OLED_ShowNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
```

**功能描述**

在指定位置显示无符号十进制数。

**参数**

- `Line`: 行号，范围 1-4
- `Column`: 列号，范围 1-16
- `Number`: 要显示的数字（无符号）
- `Length`: 显示的位数

**返回值**

- 无

**使用示例**

```c
OLED_ShowNum(1, 1, 12345, 5);   // 显示 "12345"
OLED_ShowNum(2, 1, 42, 4);      // 显示 "0042"
OLED_ShowNum(3, 1, 999, 3);     // 显示 "999"
```

**注意事项**

- 如果数字位数小于 Length，前面会补 0
- 如果数字位数大于 Length，可能显示不完整

---

#### OLED_ShowSignedNum

**函数原型**

```c
void OLED_ShowSignedNum(uint8_t Line, uint8_t Column, int32_t Number, uint8_t Length);
```

**功能描述**

在指定位置显示有符号十进制数。

**参数**

- `Line`: 行号，范围 1-4
- `Column`: 列号，范围 1-16
- `Number`: 要显示的数字（有符号）
- `Length`: 显示的位数（包括符号位）

**返回值**

- 无

**使用示例**

```c
OLED_ShowSignedNum(1, 1, -123, 4);   // 显示 "-123"
OLED_ShowSignedNum(2, 1, 456, 4);    // 显示 "+456" 或 "456"
OLED_ShowSignedNum(3, 1, 0, 3);      // 显示 "0"
```

**注意事项**

- 负数会显示负号
- Length 需要考虑符号位的空间

---

#### OLED_ShowHexNum

**函数原型**

```c
void OLED_ShowHexNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
```

**功能描述**

在指定位置显示十六进制数。

**参数**

- `Line`: 行号，范围 1-4
- `Column`: 列号，范围 1-16
- `Number`: 要显示的数字
- `Length`: 显示的位数

**返回值**

- 无

**使用示例**

```c
OLED_ShowHexNum(1, 1, 0xABCD, 4);   // 显示 "ABCD"
OLED_ShowHexNum(2, 1, 0xFF, 2);     // 显示 "FF"
OLED_ShowHexNum(3, 1, 255, 2);      // 显示 "FF"
```

**注意事项**

- 十六进制字母以大写显示
- 不显示 "0x" 前缀

---

#### OLED_ShowBinNum

**函数原型**

```c
void OLED_ShowBinNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
```

**功能描述**

在指定位置显示二进制数。

**参数**

- `Line`: 行号，范围 1-4
- `Column`: 列号，范围 1-16
- `Number`: 要显示的数字
- `Length`: 显示的位数

**返回值**

- 无

**使用示例**

```c
OLED_ShowBinNum(1, 1, 0b10101010, 8);   // 显示 "10101010"
OLED_ShowBinNum(2, 1, 15, 4);           // 显示 "1111"
OLED_ShowBinNum(3, 1, 7, 8);            // 显示 "00000111"
```

**注意事项**

- 不显示 "0b" 前缀
- 如果位数不足，前面会补 0

---

#### OLED_WriteData

**函数原型**

```c
void OLED_WriteData(uint8_t Data);
```

**功能描述**

向 OLED 写入单个数据字节（底层函数）。

**参数**

- `Data`: 要写入的数据字节

**返回值**

- 无

**使用示例**

```c
OLED_WriteData(0x00);  // 写入数据
```

**注意事项**

- 这是底层函数，一般不直接调用
- 用于高级自定义显示功能

---

#### OLED_WriteData_Multi

**函数原型**

```c
void OLED_WriteData_Multi(uint8_t *pData, uint8_t Length);
```

**功能描述**

向 OLED 写入多个数据字节（底层函数）。

**参数**

- `pData`: 指向数据缓冲区的指针
- `Length`: 要写入的字节数

**返回值**

- 无

**使用示例**

```c
uint8_t data[8] = {0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07};
OLED_WriteData_Multi(data, 8);
```

**注意事项**

- 这是底层函数，用于批量数据传输
- 确保缓冲区大小足够

---

#### OLED_SetCursor

**函数原型**

```c
void OLED_SetCursor(uint8_t Y, uint8_t X);
```

**功能描述**

设置 OLED 显示光标位置（底层函数）。

**参数**

- `Y`: Y 坐标（页地址）
- `X`: X 坐标（列地址）

**返回值**

- 无

**使用示例**

```c
OLED_SetCursor(0, 0);  // 设置光标到原点
```

**注意事项**

- 这是底层函数，用于精确控制光标位置
- 坐标系统与硬件相关

---

## LED 模块

LED 模块提供了 LED 指示灯的控制功能。

### 头文件

```c
#include "LED.h"
```

### 函数列表

#### LED_Init

**函数原型**

```c
void LED_Init(void);
```

**功能描述**

初始化 LED 模块，配置 GPIO 引脚。

**参数**

- 无

**返回值**

- 无

**使用示例**

```c
int main(void)
{
    LED_Init();  // 初始化 LED
    // 其他代码...
}
```

**注意事项**

- 必须在使用 LED 控制函数之前调用

---

#### LED1_ON

**函数原型**

```c
void LED1_ON(void);
```

**功能描述**

点亮 LED1。

**参数**

- 无

**返回值**

- 无

**使用示例**

```c
LED1_ON();  // 打开 LED1
```

---

#### LED1_OFF

**函数原型**

```c
void LED1_OFF(void);
```

**功能描述**

熄灭 LED1。

**参数**

- 无

**返回值**

- 无

**使用示例**

```c
LED1_OFF();  // 关闭 LED1
```

---

#### LED1_Turn

**函数原型**

```c
void LED1_Turn(void);
```

**功能描述**

切换 LED1 的状态（亮变灭，灭变亮）。

**参数**

- 无

**返回值**

- 无

**使用示例**

```c
LED1_Turn();  // 切换 LED1 状态
```

**注意事项**

- 每次调用会改变当前 LED 状态

---

#### LED2_ON

**函数原型**

```c
void LED2_ON(void);
```

**功能描述**

点亮 LED2。

**参数**

- 无

**返回值**

- 无

**使用示例**

```c
LED2_ON();  // 打开 LED2
```

---

#### LED2_OFF

**函数原型**

```c
void LED2_OFF(void);
```

**功能描述**

熄灭 LED2。

**参数**

- 无

**返回值**

- 无

**使用示例**

```c
LED2_OFF();  // 关闭 LED2
```

---

#### LED2_Turn

**函数原型**

```c
void LED2_Turn(void);
```

**功能描述**

切换 LED2 的状态（亮变灭，灭变亮）。

**参数**

- 无

**返回值**

- 无

**使用示例**

```c
LED2_Turn();  // 切换 LED2 状态
```

---

## Key 模块

Key 模块提供了按键检测功能。

### 头文件

```c
#include "Key.h"
```

### 函数列表

#### Key_Init

**函数原型**

```c
void Key_Init(void);
```

**功能描述**

初始化按键模块，配置 GPIO 引脚。

**参数**

- 无

**返回值**

- 无

**使用示例**

```c
int main(void)
{
    Key_Init();  // 初始化按键
    // 其他代码...
}
```

**注意事项**

- 必须在使用按键检测函数之前调用

---

#### Key_GetNum

**函数原型**

```c
uint8_t Key_GetNum(void);
```

**功能描述**

获取按下的按键编号。

**参数**

- 无

**返回值**

- `uint8_t`: 按键编号
  - `0`: 无按键按下
  - `1`: 按键 1 被按下
  - `2`: 按键 2 被按下
  - 其他值根据硬件配置而定

**使用示例**

```c
uint8_t KeyNum;

while(1)
{
    KeyNum = Key_GetNum();
    
    if(KeyNum == 1)
    {
        // 按键 1 被按下的处理
        OLED_ShowString(1, 1, "Key1 Pressed");
    }
    else if(KeyNum == 2)
    {
        // 按键 2 被按下的处理
        OLED_ShowString(1, 1, "Key2 Pressed");
    }
}
```

**注意事项**

- 返回值为 0 表示没有按键按下
- 函数通常在主循环中轮询调用
- 按键可能需要消抖处理

---

## 综合示例

### 示例 1: 基本显示

```c
#include "stm32f10x.h"
#include "OLED.h"

int main(void)
{
    // 初始化
    OLED_Init();
    
    // 显示欢迎信息
    OLED_ShowString(1, 1, "Welcome!");
    OLED_ShowString(2, 1, "OLED Display");
    OLED_ShowString(3, 1, "STM32F103");
    
    while(1)
    {
        // 主循环
    }
}
```

### 示例 2: LED 和按键控制

```c
#include "stm32f10x.h"
#include "OLED.h"
#include "LED.h"
#include "Key.h"

int main(void)
{
    uint8_t KeyNum;
    
    // 初始化
    OLED_Init();
    LED_Init();
    Key_Init();
    
    // 显示提示信息
    OLED_ShowString(1, 1, "Press Key:");
    
    while(1)
    {
        KeyNum = Key_GetNum();
        
        if(KeyNum == 1)
        {
            LED1_Turn();
            OLED_ShowString(2, 1, "LED1 Toggle ");
        }
        else if(KeyNum == 2)
        {
            LED2_Turn();
            OLED_ShowString(2, 1, "LED2 Toggle ");
        }
    }
}
```

### 示例 3: 数字显示

```c
#include "stm32f10x.h"
#include "OLED.h"

int main(void)
{
    uint32_t counter = 0;
    
    // 初始化
    OLED_Init();
    
    // 显示标题
    OLED_ShowString(1, 1, "Counter:");
    
    while(1)
    {
        // 显示计数值
        OLED_ShowNum(2, 1, counter, 8);
        
        // 延时
        for(uint32_t i = 0; i < 1000000; i++);
        
        counter++;
    }
}
```

---

## 常见问题

### Q: OLED 不显示内容？

**A:** 检查以下几点：
1. 确认 I2C 连接是否正确
2. 检查 OLED 电源是否正常
3. 确认调用了 `OLED_Init()` 函数
4. 检查 I2C 地址是否正确

### Q: 显示内容乱码？

**A:** 可能的原因：
1. 字符超出 ASCII 范围
2. 字体库损坏
3. I2C 通信不稳定

### Q: LED 不亮？

**A:** 检查：
1. LED 连接是否正确
2. GPIO 配置是否正确
3. 电源电压是否正常

### Q: 按键检测不灵敏？

**A:** 可能需要：
1. 添加硬件消抖电路
2. 增加软件消抖延时
3. 检查按键连接

---

## 技术支持

如有问题，请访问：
- GitHub Issues: https://github.com/RtimesC/OLED1/issues
- 项目主页: https://github.com/RtimesC/OLED1

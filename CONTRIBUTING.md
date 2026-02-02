# 贡献指南

首先，感谢您考虑为 OLED1 项目做出贡献！本文档将指导您如何为项目做出贡献。

## 📋 目录

- [如何贡献](#如何贡献)
- [报告 Bug](#报告-bug)
- [提出新功能](#提出新功能)
- [提交代码](#提交代码)
- [开发工作流](#开发工作流)
- [代码规范](#代码规范)
- [提交信息规范](#提交信息规范)
- [行为准则](#行为准则)
- [许可证](#许可证)

## 🤝 如何贡献

您可以通过以下方式为项目做出贡献：

1. **报告 Bug** - 发现问题时请提交 Issue
2. **提出新功能** - 分享您的想法和建议
3. **完善文档** - 改进文档质量，修正错误
4. **提交代码** - 修复 Bug 或实现新功能
5. **代码审查** - 帮助审查其他贡献者的代码

## 🐛 报告 Bug

在提交 Bug 报告前，请先检查：

1. 确保您使用的是最新版本
2. 搜索已有的 Issues，避免重复提交
3. 尽可能提供详细信息

### Bug 报告应包含：

- **问题描述**：简洁明了地描述问题
- **复现步骤**：详细说明如何重现问题
- **预期行为**：说明您期望发生什么
- **实际行为**：说明实际发生了什么
- **环境信息**：
  - 开发板型号（如 STM32F103C8T6）
  - OLED 模块型号（如 0.96寸 I2C）
  - Keil µVision 版本
  - 操作系统版本
- **代码片段**：如果可能，提供最小可复现代码
- **图片/截图**：如有必要，添加图片说明问题

### 示例：

```markdown
**问题描述**
OLED 显示时出现乱码

**复现步骤**
1. 初始化 OLED
2. 调用 OLED_ShowString(1, 1, "Hello")
3. 观察显示效果

**预期行为**
应显示 "Hello"

**实际行为**
显示乱码或不显示

**环境信息**
- 开发板: STM32F103C8T6
- OLED: 0.96寸 I2C (SSD1306)
- Keil: µVision V5.28
- 操作系统: Windows 10

**截图**
[添加 OLED 显示的照片]
```

## 💡 提出新功能

在提出新功能建议前：

1. 确保功能与项目目标一致
2. 搜索已有的 Issues 和 Pull Requests
3. 考虑功能的通用性和可维护性

### 功能建议应包含：

- **功能描述**：清晰描述建议的功能
- **使用场景**：说明功能的应用场景
- **实现思路**：如有想法，简述实现方式
- **替代方案**：是否考虑过其他方案
- **示例代码**：如果可能，提供示例代码

## 🔧 提交代码

### 开发工作流

1. **Fork 仓库**
   ```bash
   # 在 GitHub 上点击 Fork 按钮
   ```

2. **克隆仓库**
   ```bash
   git clone https://github.com/YOUR_USERNAME/OLED1.git
   cd OLED1
   ```

3. **创建分支**
   ```bash
   # 从 main 分支创建新分支
   git checkout -b feature/your-feature-name
   # 或
   git checkout -b fix/your-bug-fix-name
   ```

4. **进行修改**
   - 按照代码规范编写代码
   - 添加必要的注释
   - 确保代码能够编译通过

5. **提交更改**
   ```bash
   git add .
   git commit -m "Add: 添加新功能"
   ```

6. **推送到 Fork 的仓库**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **创建 Pull Request**
   - 在 GitHub 上创建 Pull Request
   - 填写 PR 描述，说明变更内容
   - 等待审核和反馈

### Pull Request 规范

**标题格式：**
```
[类型] 简短描述

示例：
[Feature] 添加 OLED 图片显示功能
[Fix] 修复 I2C 通信时序问题
[Docs] 更新 API 文档
[Refactor] 重构 OLED 初始化代码
```

**描述内容：**
- **变更说明**：详细描述做了哪些修改
- **相关 Issue**：如 `Closes #123` 或 `Fixes #456`
- **测试情况**：说明如何测试的
- **截图/视频**：如有 UI 变更，添加效果图

## 📝 代码规范

### C 代码风格

遵循以下编码规范：

#### 1. 命名规范

```c
// 函数命名：大驼峰，动词开头
void OLED_Init(void);
void OLED_ShowString(uint8_t Line, uint8_t Column, char *String);

// 变量命名：小驼峰
uint32_t localVariable;
uint8_t dataBuffer[16];

// 宏定义：全大写，下划线分隔
#define OLED_ADDRESS    0x78
#define MAX_BUFFER_SIZE 128

// 静态函数：使用 static
static void OLED_I2C_Start(void);
static void OLED_I2C_Stop(void);
```

#### 2. 注释规范

```c
/**
 * @brief  函数简短描述
 * @param  Line: 行位置（1-4）
 * @param  Column: 列位置（1-16）
 * @param  String: 要显示的字符串
 * @retval None
 * @note   字符串长度不要超过屏幕宽度
 */
void OLED_ShowString(uint8_t Line, uint8_t Column, char *String)
{
    uint32_t i;  // 循环变量
    
    // 设置显示位置
    OLED_SetCursor((Line - 1) * 2, Column - 1);
    
    // 逐字符显示
    for (i = 0; String[i] != '\0'; i++)
    {
        OLED_ShowChar(Line, Column + i, String[i]);
    }
}
```

#### 3. 代码格式

```c
// 大括号风格：K&R 风格（函数另起一行，if/for/while 同行）
void FunctionName(void)
{
    // 函数体
}

if (condition) {
    // 代码块
} else {
    // 代码块
}

// 缩进：使用 Tab 或 4 空格
// 运算符两侧加空格
uint32_t result = value1 + value2;

// 逗号后加空格
OLED_ShowNum(1, 1, 123, 3);
```

#### 4. 文件头注释

```c
/**
 ******************************************************************************
 * @file    OLED.c
 * @author  RtimesC
 * @version V1.0.0
 * @date    2026-02-02
 * @brief   OLED 显示屏驱动程序（基于 I2C）
 ******************************************************************************
 * @attention
 * 
 * 硬件连接：
 *   - SCL -> PB8
 *   - SDA -> PB9
 *   - VCC -> 3.3V
 *   - GND -> GND
 * 
 * 使用前需要先调用 OLED_Init() 初始化
 ******************************************************************************
 */
```

### 代码质量要求

1. **编译无警告**：确保代码在 Keil 中编译无警告
2. **功能测试**：在实际硬件上测试功能是否正常
3. **注释清晰**：关键代码添加注释说明
4. **避免魔数**：使用宏定义代替硬编码的数字
5. **错误处理**：适当添加错误检查和处理

## 📋 提交信息规范

遵循以下提交信息格式：

### 格式

```
<类型>: <简短描述>

[可选的详细描述]

[可选的相关 Issue]
```

### 类型

- **Add**: 添加新功能或新文件
- **Fix**: 修复 Bug
- **Update**: 更新现有功能或文档
- **Refactor**: 代码重构（不改变功能）
- **Style**: 代码格式调整（空格、缩进等）
- **Docs**: 文档更新
- **Test**: 添加或修改测试
- **Chore**: 构建工具或辅助工具的变动
- **Remove**: 删除文件或代码

### 示例

```bash
# 好的提交信息
git commit -m "Add: 添加 OLED 图片显示功能"
git commit -m "Fix: 修复 I2C 通信超时问题"
git commit -m "Update: 优化延时函数精度"
git commit -m "Docs: 完善 API 文档"

# 不好的提交信息
git commit -m "修改"
git commit -m "update"
git commit -m "fix bug"
```

### 详细描述示例

```bash
git commit -m "Add: 添加 OLED 图片显示功能

- 实现位图数据的写入
- 添加 OLED_ShowImage() 函数
- 支持 128x64 单色位图
- 更新 API 文档

Closes #15"
```

## 🌟 行为准则

### 我们的承诺

为了营造开放和友好的环境，我们承诺：

- **尊重他人**：尊重不同的观点和经验
- **包容友好**：欢迎所有背景的贡献者
- **专注项目**：保持讨论与项目相关
- **接受批评**：以开放的心态接受建设性反馈
- **关注社区**：关注对社区最有利的事情

### 不可接受的行为

- 侮辱性或贬损性评论
- 人身攻击或政治攻击
- 公开或私下骚扰
- 未经许可发布他人的私人信息
- 其他不道德或不专业的行为

### 执行

项目维护者有权利和责任删除、编辑或拒绝不符合本行为准则的评论、提交、代码、问题和其他贡献。

## 📜 许可证

提交代码即表示您同意：

1. 您的贡献将使用 MIT 许可证
2. 您拥有提交内容的合法权利
3. 您的贡献不侵犯第三方权利

参见 [LICENSE](LICENSE) 文件了解完整的许可证文本。

## ❓ 问题与帮助

如有任何问题，您可以：

1. **查看文档**：先查看 [README.md](README.md) 和 [docs/](docs/)
2. **搜索 Issues**：查看是否有类似问题
3. **提交 Issue**：创建新 Issue 描述您的问题
4. **参与讨论**：在现有 Issue 中参与讨论

## 🙏 致谢

感谢所有为项目做出贡献的人！

您的每一个贡献，无论大小，都对项目很重要。

---

再次感谢您的贡献！🎉

**项目维护者：RtimesC**

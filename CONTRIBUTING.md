# 贡献指南

感谢你考虑为 OLED1 项目做出贡献！我们欢迎所有形式的贡献，包括但不限于：

- 报告 Bug
- 讨论代码的当前状态
- 提交修复
- 提议新功能
- 成为维护者

## 📋 目录

- [行为准则](#行为准则)
- [我们的开发流程](#我们的开发流程)
- [如何报告 Bug](#如何报告-bug)
- [如何提交功能请求](#如何提交功能请求)
- [代码贡献流程](#代码贡献流程)
- [代码规范](#代码规范)
- [提交信息规范](#提交信息规范)
- [Pull Request 流程](#pull-request-流程)

## 行为准则

本项目采用贡献者公约作为行为准则。参与本项目即表示你同意遵守其条款。

## 我们的开发流程

我们使用 GitHub 来托管代码、跟踪问题和功能请求，以及接受 Pull Request。

### 开发分支

- `main` - 主分支，包含最新的稳定代码
- `develop` - 开发分支，包含最新的开发代码
- `feature/*` - 功能分支
- `bugfix/*` - Bug 修复分支
- `hotfix/*` - 热修复分支

## 如何报告 Bug

我们使用 GitHub Issues 来跟踪公开的 Bug。报告 Bug 时请使用我们的 Bug 报告模板。

### 优秀的 Bug 报告通常包含：

- **简短的摘要** - 用一两句话描述问题
- **详细的步骤** - 可重现问题的详细步骤
  1. 第一步
  2. 第二步
  3. ...
- **预期行为** - 你期望发生什么
- **实际行为** - 实际发生了什么
- **环境信息** - 包括：
  - 操作系统（Windows/Linux/macOS）
  - Keil µVision 版本
  - STM32 型号
  - 编译器版本
- **额外信息** - 截图、错误日志等

### 报告 Bug 的快速链接

[创建新的 Bug 报告](../../issues/new?template=bug_report.md)

## 如何提交功能请求

我们也使用 GitHub Issues 来跟踪功能请求。提交功能请求时请使用我们的功能请求模板。

### 优秀的功能请求通常包含：

- **功能描述** - 清晰地描述你希望添加的功能
- **使用场景** - 解释为什么需要这个功能
- **建议的实现** - 如果有想法，描述如何实现
- **替代方案** - 你考虑过的其他解决方案
- **额外信息** - 任何其他相关信息

### 提交功能请求的快速链接

[创建新的功能请求](../../issues/new?template=feature_request.md)

## 代码贡献流程

### 1. Fork 项目

点击页面右上角的 "Fork" 按钮，将项目 Fork 到你的账户下。

### 2. 克隆到本地

```bash
git clone https://github.com/你的用户名/OLED1.git
cd OLED1
```

### 3. 添加上游仓库

```bash
git remote add upstream https://github.com/RtimesC/OLED1.git
```

### 4. 创建分支

```bash
# 对于新功能
git checkout -b feature/你的功能名称

# 对于 Bug 修复
git checkout -b bugfix/问题描述
```

### 5. 进行修改

在你的本地分支上进行修改。

### 6. 保持同步

```bash
git fetch upstream
git rebase upstream/main
```

### 7. 提交更改

```bash
git add .
git commit -m "feat: 添加新功能"
```

### 8. 推送到你的 Fork

```bash
git push origin feature/你的功能名称
```

### 9. 创建 Pull Request

在 GitHub 上从你的分支创建 Pull Request 到原项目的 `main` 分支。

## 代码规范

### C 语言代码规范

我们遵循以下代码规范：

#### 命名规范

```c
// 函数名：使用帕斯卡命名法（PascalCase）
void OLED_Init(void);
void LED_ShowString(uint8_t Line, uint8_t Column, char *String);

// 变量名：使用驼峰命名法（camelCase）
uint8_t keyNum;
uint16_t counterValue;

// 宏定义：使用全大写字母和下划线
#define OLED_ADDRESS 0x78
#define MAX_BUFFER_SIZE 128

// 常量：使用全大写字母和下划线
const uint8_t FONT_SIZE = 16;
```

#### 缩进和格式

```c
// 使用制表符缩进（tab = 4 spaces）
void Function(void)
{
	if (condition)
	{
		// 代码
	}
	else
	{
		// 代码
	}
}
```

#### 注释规范

```c
/**
 * @brief  函数简要描述
 * @param  参数1描述
 * @param  参数2描述
 * @retval 返回值描述
 */
void OLED_ShowString(uint8_t Line, uint8_t Column, char *String)
{
	// 实现代码
}
```

#### 文件头注释

```c
/**
  ******************************************************************************
  * @file    文件名.c
  * @author  作者名
  * @version V版本号
  * @date    日期
  * @brief   简要描述文件功能
  ******************************************************************************
  */
```

### 代码质量要求

- ✅ 代码必须能够成功编译，无警告
- ✅ 确保代码符合项目的编码风格
- ✅ 添加必要的注释
- ✅ 保持函数简洁，每个函数只做一件事
- ✅ 避免使用魔术数字，使用有意义的常量
- ✅ 确保代码在目标硬件上正常工作

## 提交信息规范

我们使用 [约定式提交](https://www.conventionalcommits.org/zh-hans/) 规范。

### 提交信息格式

```
<类型>(<范围>): <描述>

[可选的正文]

[可选的脚注]
```

### 类型

- `feat`: 新功能
- `fix`: Bug 修复
- `docs`: 文档修改
- `style`: 代码格式修改（不影响代码运行）
- `refactor`: 代码重构
- `perf`: 性能优化
- `test`: 测试相关
- `chore`: 构建过程或辅助工具的变动

### 示例

```bash
# 添加新功能
git commit -m "feat(OLED): 添加 OLED 清屏功能"

# 修复 Bug
git commit -m "fix(LED): 修复 LED 闪烁问题"

# 文档更新
git commit -m "docs(README): 更新 API 使用示例"

# 代码重构
git commit -m "refactor(Key): 优化按键扫描算法"
```

### 提交信息注意事项

- 使用现在时态："添加功能" 而不是 "添加了功能"
- 使用祈使语气："修改..." 而不是 "修改了..."
- 首行不超过 72 个字符
- 在首行后空一行，然后是详细描述
- 引用相关的 Issue 编号

## Pull Request 流程

### 提交 PR 前的检查清单

在提交 Pull Request 之前，请确保：

- [ ] 代码符合项目的编码规范
- [ ] 代码能够成功编译，无警告
- [ ] 已测试修改的代码在目标硬件上正常工作
- [ ] 已添加或更新相关文档
- [ ] 提交信息符合规范
- [ ] PR 描述清楚地说明了改动内容

### PR 标题格式

PR 标题应该遵循提交信息规范：

```
feat: 添加 OLED 动画显示功能
fix: 修复 I2C 通信超时问题
docs: 完善 API 文档
```

### PR 描述模板

提交 PR 时会自动加载模板，请填写完整。

### 审查流程

1. 提交 PR 后，维护者会进行代码审查
2. 如果有需要修改的地方，维护者会留下评论
3. 根据反馈修改代码并推送到同一分支
4. 审查通过后，PR 会被合并到主分支

### 注意事项

- 每个 PR 应该只关注一个功能或 Bug 修复
- 保持 PR 的规模适中，避免一次性提交太多改动
- 及时响应审查意见
- 耐心等待审查，维护者会尽快处理

## 开发环境设置

### 必需工具

1. **Keil µVision 5** - [下载地址](http://www.keil.com/download/product/)
2. **Git** - [下载地址](https://git-scm.com/downloads)
3. **ST-Link Utility** - [下载地址](https://www.st.com/en/development-tools/stsw-link004.html)

### 设置步骤

```bash
# 1. 克隆项目
git clone https://github.com/你的用户名/OLED1.git
cd OLED1

# 2. 打开项目
# 使用 Keil µVision 打开 Project.uvprojx

# 3. 配置目标板
# 在 Keil 中配置你的 STM32 型号

# 4. 编译测试
# 点击 Project -> Build Target (F7)
```

## 测试指南

### 在硬件上测试

1. 连接 STM32 开发板和 OLED 模块
2. 连接 ST-Link 调试器
3. 编译并下载程序到开发板
4. 验证功能是否正常工作
5. 记录测试结果

### 测试报告

在 PR 中提供测试信息：

```markdown
## 测试环境
- 操作系统: Windows 10
- Keil 版本: µVision V5.36
- STM32 型号: STM32F103C8T6
- OLED 型号: 0.96寸 SSD1306

## 测试结果
- [x] OLED 显示正常
- [x] LED 控制正常
- [x] 按键检测正常
```

## 寻求帮助

如果你在贡献过程中遇到问题，可以：

- 在 [GitHub Issues](../../issues) 中提问
- 查看 [文档](docs/)
- 联系项目维护者

## 许可证

通过贡献代码，你同意你的贡献将在 [MIT License](LICENSE) 下发布。

---

再次感谢你的贡献！🎉

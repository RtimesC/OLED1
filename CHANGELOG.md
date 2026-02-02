# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/lang/zh-CN/spec/v2.0.0.html).

## [1.0.0] - 2026-02-02

### Added

- 初始版本发布
- OLED 显示屏驱动（SSD1306 控制器）
  - 支持 128x64 分辨率显示
  - I2C 通信接口实现
  - 基础显示初始化功能
- 字符和字符串显示功能
  - 单字符显示 (`OLED_ShowChar`)
  - 字符串显示 (`OLED_ShowString`)
  - 支持多种字体大小
- 数字显示功能
  - 十进制数显示 (`OLED_ShowNum`)
  - 十六进制数显示 (`OLED_ShowHexNum`)
  - 二进制数显示 (`OLED_ShowBinNum`)
  - 支持指定显示位数
- LED 控制功能
  - LED 初始化
  - LED 开关控制
  - LED 状态切换
- 按键输入功能
  - 按键初始化
  - 按键扫描检测
  - 按键值获取
- 内置字体库
  - 支持 ASCII 字符集
  - 可扩展字体支持
- 项目文档
  - 基础 README 文档
  - 项目结构说明
  - 简单使用指南
- 开发环境配置
  - Keil µVision 项目文件
  - STM32 标准库集成
  - 编译配置

### Technical Details

- **语言**: C (89.8%), Assembly (5.7%)
- **平台**: STM32F10x 系列微控制器
- **开发工具**: Keil µVision 5
- **通信协议**: I2C
- **显示控制器**: SSD1306

---

## [Unreleased]

### Planned

- [ ] 添加图形绘制功能（线条、矩形、圆形）
- [ ] 添加中文字体支持
- [ ] 实现动画显示功能
- [ ] 添加屏幕保护功能
- [ ] 优化 I2C 通信速度
- [ ] 添加 SPI 接口支持
- [ ] 提供更多字体选项
- [ ] 添加低功耗模式
- [ ] 完善错误处理机制
- [ ] 添加单元测试

---

## 版本说明

### 版本号规范

本项目遵循 [语义化版本规范 (SemVer)](https://semver.org/lang/zh-CN/)：

- **主版本号**：进行不兼容的 API 更改
- **次版本号**：添加向后兼容的新功能
- **修订号**：进行向后兼容的问题修复

### 变更类型

- `Added` - 新增功能
- `Changed` - 现有功能的变更
- `Deprecated` - 即将移除的功能
- `Removed` - 已移除的功能
- `Fixed` - 问题修复
- `Security` - 安全性改进

---

[1.0.0]: https://github.com/RtimesC/OLED1/releases/tag/v1.0.0
[Unreleased]: https://github.com/RtimesC/OLED1/compare/v1.0.0...HEAD

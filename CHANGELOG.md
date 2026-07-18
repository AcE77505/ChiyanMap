# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- 恢复对游戏版本 1.26.10.04 的支持：
  - 放宽版本检测，同时接受 `1.26.10-4` 和 `1.26.20-4`，兼容 1.26.10.04 / 1.26.20.04 双版本。
  - 更新 15 国语言版本提示文案，准确反映双版本支持。
  - `xmake.lua` 新增 `--levilamina_ver` 构建选项，方便指定 LeviLamina 版本交叉编译。
  - CI 自动为 1.26.10.04 和 1.26.20.04 双版本构建 Release。

### Changed
- 调整 DX11Hook.h 中 `isTyping` 检测逻辑，避免 IME 输入状态下快捷键误触发（先前代码存在合并冲突残留，已清理）。

- 接入 LeviLamina 官方标准多语言框架：
  - 将自研的 `LanguageManager` 底层翻译解析重构为 LeviLamina 的 [ll::i18n](file:///D:/Project/LeviLamina/src/ll/api/i18n/I18n.h) 架构，提升系统兼容性与稳定性。
  - 在 `LanguageManager::GetText` 中实现本地静态线程安全缓存，有效防范 `std::string_view` 的生命周期悬空（dangling pointer）问题，保持接口层完全兼容。

### Changed
- 优化小地图操作交互：
  - 在小地图隐藏（`showMiniMap == false`）状态下，屏蔽 `Y` 键切换地图形状的功能，避免隐藏时的误触和无效配置保存。

### Fixed
- 修正玩家坐标显示高度偏差：
  - 将小地图数据获取中本地玩家的位置接口从 `player->getPosition()` 修正为 [player->getFeetPos()](file:///D:/Project/LeviLamina/src/mc/world/actor/Actor.h#L141)。
  - 解决了因客户端获取视线/眼睛位置带来的 `+1.62` 轴高度偏移，使小地图下方显示坐标及雷达定位与游戏内置的足底实际坐标完全吻合。


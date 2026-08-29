# FlymeNotifyForMIUI14

> 在 MIUI 14 上实现锁屏样式通知中心

## ⚠️ 免责声明

本模块通过 Xposed 框架 Hook 系统界面进程，**仅限个人学习与研究使用**。

- 使用本模块前请确保已安装自动救砖模块，本人不保证该模块必成功运行
- 使用本模块前若不安装任何自动救砖模块，请务必备份数据，开发者不对因使用本模块导致的系统崩溃、无法开机、数据丢失、Bootloop 等任何后果承担责任
- 本模块可能违反部分手机厂商的保修条款，刷入即视为你已了解相关风险
- 请勿将本模块用于商业用途或闭源分发
- 如果你不清楚 LSPosed / Xposed 的基本操作，请先学习相关知识再尝试使用
- 建议将刷入本模块的设备（fuxi）当备用机使用，若需要当作主力机使用，请关闭所有悬浮通知

**下载、安装、使用本模块即表示你已阅读并同意以上条款。**

## 功能特性

- 🎨 Flyme 风格通知中心：卡片圆角、图标排版、展开/收起动效近似 Flyme
- 🖼️ 通知面板背景自动读取**当前锁屏壁纸**
- 🌗 通知半透明
- 🔧 可配置项：模糊半径、背景透明度、是否隐藏 MIUI 原生分隔阴影
- 📦 使用 Kotlin 实现，LSPosed 注入 `com.android.systemui` 与 `miui.systemui.plugin`

## 兼容范围

| 项目 | 支持情况 |
|------|----------|
| 系统 | MIUI 14 / Android 14（HyperOS 不保证）|
| 框架 | LSPosed（minApi 100+）|
| 作用域 | `系统界面(com.android.systemui)`、`小米系统界面插件(miui.systemui.plugin)` |
| 机型 | 实测 Xiaomi 13可以运行，其他请自测 |

> ⚠️ 与 CustoMIUIzer、ChiMi、HyperLight 等改系统界面模块**可能冲突**，同开需自测。

## 安装

1. 设备已 Root 并安装 LSPosed Manager
2. 下载 Release 中的 `FlymeNotifyForMIUI14.apk` 并安装
3. 在 LSPosed 中启用本模块，勾选上述作用域
4. 重启 `系统界面`（或重启手机）
5. 下拉通知栏查看效果

## 使用说明

- 壁纸联动：锁屏换壁纸后，下拉通知中心背景会跟随变化（无需重启）-->目前该项在小米13（fuxi）上无法实现，还需日后改进
- 调试：日志 tag 为 `FlymeNotify`，可用 Logcat 过滤

## 实现简述

- 通过 `XposedHelpers.findAndHookMethod` 挂钩 SystemUI 通知面板 `NotificationStackScrollLayout` 相关绘制
- 壁纸来源：监听 `WallpaperManager.ACTION_WALLPAPER_CHANGED`，读取 `wallpaperInfo` 区分锁屏/桌面
- 背景注入：在面板容器 `setBackgroundDrawable` 前替换为带 Blur + 取色后的 `Drawable`
- 资源替换：部分尺寸/drawable 走 `ResourcesHook` 重定向到模块内 `res`

## 已知问题

- 杂志锁屏动态壁纸仅取首帧
- 部分第三方主题会覆盖背景，优先级低于主题自身
- 在搭载miui14.0.5（安卓14）的小米13（fuxi）上，更换锁屏壁纸需要重启
- 在搭载miui14.0.5（安卓14）的小米13（fuxi）上，有悬浮通知时会出现锁屏壁纸覆盖现象，目前仍不知道是什么原因
- HyperOS 未适配



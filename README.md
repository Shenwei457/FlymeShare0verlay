# FlymeNotifyForMIUI14

> 在 MIUI 14 上实现锁屏样式通知中心


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

## 实现简述（给开发者看）

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


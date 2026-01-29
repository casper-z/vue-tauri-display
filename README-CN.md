[English](./README.md) | 简体中文
# vue-tauri-display

一个基于 Tauri + Vue 3 的跨平台桌面摄像头应用，支持实时预览、截图保存等功能。

## 技术栈要求

| 技术 | 版本要求 | 说明 |
|------|----------|------|
| **Rust** | 1.56+ (推荐稳定版) | Rust 2021 Edition |
| **Node.js** | 18+ | JavaScript 运行时 |
| **Vue** | 3.3.8 | 前端框架 |
| **Tauri** | 2.0.0-rc.1 | 桌面应用框架 |
| **Vite** | 6.0.3 | 构建工具 |
| **TypeScript** | ~5.6.2 | 类型安全 |

## 功能特性

- **实时摄像头预览** - 支持 1920x1080 分辨率高清预览
- **一键截图** - 将当前画面保存为 PNG 图片
- **跨平台支持** - Windows / macOS / Linux
- **现代化 UI** - 深色主题，响应式布局
- **TypeScript 支持** - 完整类型定义

## 适用场景

- 桌面摄像头监控
- 快速截图工具
- Tauri + Vue 3 桌面应用开发模板
- 视频会议辅助工具
- 简单的视频预览应用

## 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/casperz/vue-tauri-display.git
cd vue-tauri-display
```

### 2. 安装依赖

```bash
yarn install --ignore-engines
```

### 3. 开发模式运行

```bash
yarn tauri dev
```

### 4. 打包发布

```bash
yarn tauri build
```

## 项目结构

```
vue-tauri-display/
├── src/                    # Vue 前端源码
│   ├── components/         # 组件
│   │   └── CameraView.vue  # 摄像头核心组件
│   ├── App.vue             # 主应用
│   └── main.ts             # 入口文件
├── src-tauri/              # Tauri 后端 (Rust)
│   ├── src/                # Rust 源码
│   ├── Cargo.toml          # Rust 依赖配置
│   └── tauri.conf.json     # Tauri 配置
├── package.json            # Node 依赖配置
└── vite.config.ts          # Vite 构建配置
```

## 配置说明

### Tauri 配置 (src-tauri/tauri.conf.json)

```json
{
  "productName": "Camera App",
  "version": "0.1.0",
  "identifier": "com.casper.camera-app",
  "windows": [{
    "title": "Camera App - 摄像头实时画面",
    "width": 1280,
    "height": 800,
    "minWidth": 800,
    "minHeight": 600
  }]
}
```

### 开发端口

- 开发服务器: http://localhost:1420
- Vite HMR: http://localhost:1421

## 环境要求

- 操作系统: Windows 10+, macOS 10.15+, 或 Linux
- 摄像头设备: 需要可用的摄像头
- Rust 环境: 通过 `rustup` 安装

## 注意事项

- 首次运行需要授予摄像头访问权限
- 使用 `--ignore-engines` 是因为 Node.js 版本检测可能与 Tauri CLI 存在兼容性问题
- 打包前确保已完成 `yarn install`

## 许可证

MIT License

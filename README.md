[English](./README.md) | [简体中文](./README-CN.md)

# vue-tauri-display

A cross-platform desktop camera application built with Tauri + Vue 3, supporting real-time preview, screenshot capture, and more.

## Tech Stack Requirements

| Technology | Version | Description |
|------------|---------|-------------|
| **Rust** | 1.56+ (stable recommended) | Rust 2021 Edition |
| **Node.js** | 18+ | JavaScript runtime |
| **Vue** | 3.3.8 | Frontend framework |
| **Tauri** | 2.0.0-rc.1 | Desktop application framework |
| **Vite** | 6.0.3 | Build tool |
| **TypeScript** | ~5.6.2 | Type safety |

## Features

- **Real-time Camera Preview** - Supports 1920x1080 HD resolution
- **One-click Screenshot** - Save current frame as PNG image
- **Cross-platform Support** - Windows / macOS / Linux
- **Modern UI** - Dark theme, responsive layout
- **TypeScript Support** - Complete type definitions

## Use Cases

- Desktop camera monitoring
- Quick screenshot tool
- Tauri + Vue 3 desktop app development template
- Video conferencing assistant
- Simple video preview application

## Quick Start

### 1. Clone the Project

```bash
git clone https://github.com/casperz/vue-tauri-display.git
cd vue-tauri-display
```

### 2. Install Dependencies

```bash
yarn install --ignore-engines
```

### 3. Run in Dev Mode

```bash
yarn tauri dev
```

### 4. Build for Release

```bash
yarn tauri build
```

## Project Structure

```
vue-tauri-display/
├── src/                    # Vue frontend source code
│   ├── components/         # Components
│   │   └── CameraView.vue  # Camera core component
│   ├── App.vue             # Main application
│   └── main.ts             # Entry file
├── src-tauri/              # Tauri backend (Rust)
│   ├── src/                # Rust source code
│   ├── Cargo.toml          # Rust dependency configuration
│   └── tauri.conf.json     # Tauri configuration
├── package.json            # Node dependency configuration
└── vite.config.ts          # Vite build configuration
```

## Configuration

### Tauri Configuration (src-tauri/tauri.conf.json)

```json
{
  "productName": "Camera App",
  "version": "0.1.0",
  "identifier": "com.casper.camera-app",
  "windows": [{
    "title": "Camera App - Camera Real-time View",
    "width": 1280,
    "height": 800,
    "minWidth": 800,
    "minHeight": 600
  }]
}
```

### Development Ports

- Dev Server: http://localhost:1420
- Vite HMR: http://localhost:1421

## Environment Requirements

- Operating System: Windows 10+, macOS 10.15+, or Linux
- Camera Device: Working camera required
- Rust Environment: Install via `rustup`

## Notes

- Camera access permission is required on first run
- `--ignore-engines` is used due to potential compatibility issues between Node.js version detection and Tauri CLI
- Ensure `yarn install` is completed before building

## License

MIT License

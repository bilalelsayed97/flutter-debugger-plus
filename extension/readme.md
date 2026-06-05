# Flutter Debugger Plus

Panel **Flutter Console+** hiển thị log Flutter/Dart debug với màu sắc chuẩn VSCode, ANSI color, click để mở đúng file/dòng, và search nhanh — nằm ngay cạnh Terminal, Debug Console, Output ở bottom panel.

## Vị trí hiển thị

Panel **Flutter Console+** nằm ở khu vực bottom panel của VS Code, cùng hàng với:

- Problems · Output · Debug Console · Terminal

> VS Code không cho extension nhúng UI trực tiếp vào tab `Debug Console` mặc định. Cách chuẩn API là tạo một panel view container riêng trong cùng khu vực.

## Tính năng

### 🎨 Màu sắc log chuẩn Debug Console

Màu sắc theo đúng CSS variables native của VSCode, tự động thích nghi với mọi theme (dark/light/custom):

| Category | Màu |
|---|---|
| `stdout` / `console` | `--vscode-debugConsole-infoForeground` |
| `stderr` | `--vscode-debugConsole-errorForeground` |
| `warn` | `--vscode-debugConsole-warningForeground` |
| `telemetry` | `--vscode-debugConsole-sourceForeground` (mờ hơn) |
| `important` | Info color + **bold** |

### 🌈 ANSI color support

Parse escape sequence `\x1b[...m` và render đúng màu terminal của VSCode:

- 16 màu foreground (30–37, 90–97) dùng `--vscode-terminal-ansiRed`, `--vscode-terminal-ansiBrightGreen`, v.v.
- Hỗ trợ: **bold**, dim, *italic*, underline — reset từng thuộc tính hoặc reset toàn bộ

### 🔗 Click file link mở đúng dòng

Tự detect pattern trong log và tạo link có thể click:

- `package:demo/main.dart:70:14` → mở `lib/main.dart` dòng 70, cột 14
- `package:flutter/src/widgets/framework.dart:1199` → mở đúng file trong Flutter SDK
- `lib/screens/home.dart:42:8` → mở theo workspace-relative path
- `/absolute/path/file.dart:10` → mở theo absolute path

Thứ tự resolve (chính xác 100%):

1. Lookup `.dart_tool/package_config.json` — map tên package → đường dẫn tuyệt đối trên disk (project files, Flutter SDK, pub packages đều chính xác)
2. Absolute path
3. Workspace-relative
4. `findFiles` fallback — ưu tiên project files, tìm rộng hơn nếu không thấy

### ⌨️ Search & Navigation

- Search text hoặc regex: `/error|exception/i`
- `Enter` → match tiếp theo, `Shift+Enter` → match trước (giống VSCode Find)
- Bôi đen text → `Cmd+F` / `Ctrl+F` → tự điền vào search box
- Nút `▲` / `▼` trên toolbar
- Filter theo category: `stdout`, `stderr`, `console`, `warn`, `telemetry`, `important`

### ⚡ Performance

- Batch render qua `requestAnimationFrame` — không lag khi log đổ nhiều
- Auto stick-to-bottom, tự dừng khi scroll lên
- Giữ tối đa `maxLines` dòng trong bộ nhớ

## Settings

```json
{
  "flutterDebuggerPlus.autoRevealOnFlutterDebug": true,
  "flutterDebuggerPlus.onlyFlutterDart": true,
  "flutterDebuggerPlus.maxLines": 5000
}
```

| Setting | Default | Mô tả |
|---|---|---|
| `autoRevealOnFlutterDebug` | `true` | Tự mở panel khi bắt đầu debug Flutter/Dart |
| `onlyFlutterDart` | `true` | Chỉ bắt log từ session Dart/Flutter, tắt để bắt tất cả |
| `maxLines` | `5000` | Số dòng log tối đa giữ trong bộ nhớ |

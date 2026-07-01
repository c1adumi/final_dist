# Dadumi Overview
Deployed at: https://dadumi.site

**Dadumi** (a.k.a. In-Line AI): 크로스 플랫폼(Windows, macOS, Linux) 데스크탑 글쓰기 어시스턴트. 백그라운드 네이티브 실행.

## What It Does

1. 전역 단축키 감지 (`Option + Space` macOS / `Alt + Space` Windows)
2. 활성 앱에서 선택된 텍스트 캡처
3. 캐럿 위치에 플로팅 오버레이 메뉴 표시
4. LLM API 호출 + 응답 스트리밍
5. 완성 텍스트를 원래 커서 위치에 삽입

## Tech Stack

| Layer | Tech |
|-------|------|
| Framework | Tauri v2 (Rust backend + webview frontend) |
| Backend | Rust (`windows-sys`, `cocoa`/`objc`) |
| Frontend | React + TypeScript |
| Styling | Tailwind CSS + native blur (Acrylic/Vibrancy) |
| API | Gemini API (streaming) |

## Project Structure

```
dadumi/
├── src-tauri/                  # Rust backend
│   ├── src/
│   │   ├── main.rs             # App setup, IPC commands
│   │   ├── os_integration/     # OS-specific hooks
│   │   │   ├── windows.rs      # Win32 caret, SendInput, clipboard
│   │   │   └── macos.rs        # AXUIElement, Cocoa APIs
│   │   └── llm.rs              # LLM client + streaming
│   ├── Cargo.toml
│   └── tauri.conf.json
│
├── src/                        # React frontend
│   ├── main.tsx
│   ├── App.tsx
│   ├── components/
│   │   └── FloatingMenu.tsx
│   └── styles/
│       └── index.css
```

## Native OS Integration

**Context Read/Write**: 2-tier approach
1. Primary: Accessibility APIs (macOS `AXUIElement` / Windows `UIA`)
2. Fallback: Clipboard + key emulation (`Cmd/Ctrl + C/V`)

**Caret Tracking**:
- Windows: `GetGUIThreadInfo` → `rcCaret` → `ClientToScreen`
- macOS: `kAXTextBoundsForRangeParameterizedAttribute`
- Fallback: mouse cursor position

## Key Dependencies (Rust)

- `arboard` - clipboard
- `enigo` - keyboard emulation

## Installation (End Users)

See [README.md](./README.md) for install/uninstall scripts per platform.

## Release Process

1. Bump version in 3 files: `package.json`, `src-tauri/tauri.conf.json`, `src-tauri/Cargo.toml`
2. Commit + push
3. Tag + push → GitHub Actions builds + releases

```bash
git tag vx.y.z && git push origin vx.y.z
```

# Running Dadumi (Agent Guide)

## Installing the app (distributed version)

download the app from https://dadumi.site/ via curl

- mac

```bash 
curl -fsSL https://raw.githubusercontent.com/c1adumi/dadumi/main/scripts/install.sh | bash
```

- windows

``` bash
irm https://raw.githubusercontent.com/c1adumi/dadumi/main/scripts/install.ps1 | iex
```


## Running for Development Reasons 

## Prerequisites

- Node.js + npm
- Rust toolchain (`rustup`)
- Platform SDK (Xcode CLT on macOS, VS Build Tools on Windows)

## First-Time Setup

```bash
# From project root (where package.json lives)
npm install
```

## Run Development Server

```bash
npm run tauri dev
```

This:
1. Starts Vite dev server (frontend hot reload)
2. Compiles Rust backend
3. Launches the Tauri window

## Build Production Binary

```bash
npm run tauri build
```

Output: platform-specific installer in `src-tauri/target/release/bundle/`

## Run Tests

### Frontend (if tests exist)

```bash
npm test
```

### Rust Backend

```bash
cd src-tauri
cargo test
```

## Project Init (Reference Only)

If starting fresh (not needed for existing repo):

```bash
npx -y create-tauri-app@latest ./ --alpha --template react-ts --manager npm --force
```

## Common Issues

| Issue | Fix |
|-------|-----|
| `cargo` not found | Install Rust: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \| sh` |
| macOS "unverified developer" | Right-click app → Open |
| Windows WebView2 missing | Download from Microsoft |

## File Locations (Runtime Data)

| Platform | Config/Data |
|----------|-------------|
| macOS | `~/Library/Application Support/com.gayeonlee.dadumi` |
| Windows | `%APPDATA%/com.gayeonlee.dadumi` |
| Linux | `~/.local/share/com.gayeonlee.dadumi` |

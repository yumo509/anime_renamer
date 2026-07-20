# 打包指南 — 将 Python 脚本打包为 EXE

本文档详细说明如何将 `anime_renamer_v3.6.0.py` 打包为独立的 Windows `.exe` 文件。

## 前置条件

### Windows
1. 安装 Python 3.8+：[python.org](https://www.python.org/downloads/)
2. 安装 PyInstaller：
   ```powershell
   pip install pyinstaller
   ```

### Linux (飞牛NAS/Debian/Ubuntu)
```bash
sudo apt update && sudo apt install python3 python3-pip -y
pip3 install pyinstaller
```

---

## Windows 打包步骤

### 1. 准备依赖
```powershell
pip install requests pyinstaller
```

### 2. 打包命令

**单文件 EXE（推荐，方便分发）：**
```powershell
pyinstaller --onefile --console --name "anime_renamer" anime_renamer_v3.6.0.py
```

**带图标（可选）：**
```powershell
pyinstaller --onefile --console --icon=icon.ico --name "anime_renamer" anime_renamer_v3.6.0.py
```

**减小体积（可选，需先 `pip install upx`）：**
```powershell
pyinstaller --onefile --console --name "anime_renamer" --upx-dir="C:\upx" anime_renamer_v3.6.0.py
```

### 3. 输出位置
打包完成后，EXE 文件位于：
```
dist\anime_renamer.exe
```

### 4. 测试
```powershell
# 交互模式
.\dist\anime_renamer.exe

# 命令行模式
.\dist\anime_renamer.exe --path "Y:\backups\downloads" --test
```

---

## Linux 打包步骤

### 打包命令
```bash
pyinstaller --onefile --console --name "anime_renamer" anime_renamer_v3.6.0.py
```

### 输出位置
```
dist/anime_renamer
```

### 运行
```bash
# 交互模式
./dist/anime_renamer

# 命令行模式
./dist/anime_renamer --path "/mnt/backups/downloads" --test
```

---

## 命令行参数说明

打包后的 EXE 支持非交互式运行：

| 参数 | 简写 | 说明 | 示例 |
|------|------|------|------|
| `--path` | `-p` | 动漫文件夹路径（必填） | `--path "Y:\anime"` |
| `--key` | `-k` | TMDB API Key | `--key "abc123..."` |
| `--test` | `-t` | 预览模式（不加则执行） | `--test` |
| `--source` | `-s` | 数据源：auto/tmdb/bangumi | `--source tmdb` |
| `--proxy` | | 代理地址 | `--proxy "http://127.0.0.1:7890"` |

### 使用示例

```powershell
# 预览模式
anime_renamer.exe --path "Y:\backups\downloads" --test

# 使用 TMDB + 指定 Key + 执行模式
anime_renamer.exe --path "Y:\backups\downloads" --key "your_tmdb_key" --source tmdb

# 使用代理
anime_renamer.exe --path "Y:\backups\downloads" --test --proxy "http://127.0.0.1:7890"
```

---

## 常见问题

### Q: 打包后 EXE 太大（>30MB）？
A: Python 打包自带解释器，这是正常的。可使用 `--upx-dir` 压缩，或使用 `pip install pipenv` + 虚拟环境减少体积。

### Q: 杀毒软件误报？
A: PyInstaller 打包的 EXE 有时会被误报。将文件加入白名单即可。

### Q: 中文乱码？
A: 脚本内置了 UTF-8 编码处理。如果 EXE 运行时乱码，在 cmd/PowerShell 中执行 `chcp 65001` 后重试。

### Q: 缺少 requests 模块？
A: PyInstaller 会自动打包依赖。如果报错，确保打包前 `pip install requests` 已安装。

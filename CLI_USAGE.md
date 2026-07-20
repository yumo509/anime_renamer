# 动漫批量重命名工具 — 命令行参数完整说明

## 参数列表

| 参数 | 简写 | 必填 | 作用 | 示例 |
|------|------|------|------|------|
| `--path` | `-p` | ✅ | 动漫文件夹路径 | `--path "Y:\anime"` |
| `--key` | `-k` | ❌ | TMDB API Key（不填自动用 Bangumi） | `--key "abc123def456"` |
| `--test` | `-t` | ❌ | 预览模式（不实际修改文件） | `--test` |
| `--source` | `-s` | ❌ | 数据源：`auto`(默认) / `tmdb` / `bangumi` | `--source tmdb` |
| `--proxy` | | ❌ | HTTP/SOCKS5 代理地址 | `--proxy "http://127.0.0.1:7890"` |

## 参数详解

### `--path` / `-p`
- **必填**，指定要处理的动漫文件夹路径
- 支持 Windows 和 Linux 路径格式
- 也支持位置参数（直接写路径，不加 `--path`），兼容旧用法

### `--key` / `-k`
- 可选，TMDB API Key
- 不填时，auto 模式自动使用 Bangumi 数据源（无需 Key）
- TMDB Key 申请地址：https://www.themoviedb.org/settings/api

### `--test` / `-t`
- 预览模式，只输出日志不实际修改文件
- **不加此参数时默认执行模式，会实际重命名文件**
- 建议首次使用先加 `--test` 预览确认

### `--source` / `-s`
- 可选值：`auto`（默认）、`tmdb`、`bangumi`
- `auto`：TMDB 优先，失败回退 Bangumi
- `tmdb`：仅用 TMDB（需要 `--key`）
- `bangumi`：仅用 Bangumi（无需 Key）

### `--proxy`
- 可选，HTTP 或 SOCKS5 代理
- 格式：`http://IP:端口` 或 `socks5://IP:端口`

---

## Python 环境下调用

```powershell
# 最简用法：预览模式
python anime_renamer_v3.8.0.py --path "Y:\backups\downloads" --test

# 位置参数兼容（旧用法，路径直接写在后面）
python anime_renamer_v3.8.0.py "Y:\backups\downloads" --test

# 使用 TMDB + 指定 Key + 执行模式
python anime_renamer_v3.8.0.py --path "Y:\backups\downloads" --key "你的TMDB_KEY" --source tmdb

# Linux / NAS 环境
python3 anime_renamer_v3.8.0.py --path "/mnt/nas/downloads" --test

# 使用代理
python anime_renamer_v3.8.0.py --path "Y:\anime" --test --proxy "http://127.0.0.1:7890"

# 组合参数
python anime_renamer_v3.8.0.py -p "Y:\anime" -k "abc123" -s tmdb -t
```

---

## EXE 文件（打包后）调用

```powershell
# 预览模式
.\anime_renamer.exe --path "Y:\backups\downloads" --test

# 执行模式 + TMDB
.\anime_renamer.exe --path "Y:\backups\downloads" --key "你的KEY" --source tmdb

# 使用 SOCKS5 代理
.\anime_renamer.exe --path "Y:\anime" --test --proxy "socks5://127.0.0.1:7890"

# 简写参数
.\anime_renamer.exe -p "Y:\anime" -t
```

---

## 交互模式

**不带任何参数运行时进入交互模式**，显示完整版权声明和逐项配置菜单：

```powershell
python anime_renamer_v3.8.0.py
# 或
.\anime_renamer.exe
```

交互模式提供选项：数据源选择、代理设置、预览/执行模式、单集合并、年份后缀、特别篇格式等。

---

## 打包为 EXE

```powershell
pip install requests pyinstaller
pyinstaller --onefile --console --name "anime_renamer" anime_renamer_v3.8.0.py
# EXE 输出在 dist\anime_renamer.exe
```

详见 `PACKAGING_GUIDE.md`。

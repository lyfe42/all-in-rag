# `.gitignore` 用法速查

`.gitignore` 用来告诉 Git：哪些文件或目录不需要纳入版本控制。

本文以本项目为例，记录常用语法和检查命令。

## 1. 注释

以 `#` 开头的是注释，不会影响 Git：

```gitignore
# Python 缓存
__pycache__/
```

## 2. 忽略文件

```gitignore
.env
config.local.json
*.log
```

- `.env`：忽略任意目录下名为 `.env` 的文件。
- `config.local.json`：忽略任意目录下同名文件。
- `*.log`：忽略所有以 `.log` 结尾的文件。

如果只想忽略仓库根目录下的文件，在规则前加 `/`：

```gitignore
/.env
```

## 3. 忽略目录

目录名后加 `/`，表示忽略目录：

```gitignore
.venv/
__pycache__/
logs/
resume_docs/
```

例如，`resume_docs/` 会忽略任意目录下名为 `resume_docs` 的目录。

## 4. 通配符

### `*`：匹配任意字符

```gitignore
*.pyc
*.log
```

匹配：

```text
test.pyc
app.log
logs/debug.log
```

### `?`：匹配一个字符

```gitignore
test?.txt
```

匹配 `test1.txt` 和 `testA.txt`，但不匹配 `test12.txt`。

### `**`：匹配多级目录

```gitignore
**/temp/
```

匹配：

```text
temp/
code/temp/
data/cache/temp/
```

## 5. 使用 `!` 取消忽略

使用 `!` 可以把某个文件从忽略规则中排除：

```gitignore
*.log
!important.log
```

这表示忽略所有 `.log` 文件，但保留 `important.log`。

常见用途是保留空目录占位文件：

```gitignore
data/*
!data/.gitkeep
```

这会忽略 `data` 中的其他内容，但保留 `data/.gitkeep`。

## 6. 本项目常用规则

```gitignore
# Python 缓存
__pycache__/
*.py[cod]

# 虚拟环境
.venv/
venv/
ENV/

# VS Code 和其他 IDE
.vscode/
.idea/

# 环境变量和密钥
.env
.env.*
*.key
*.secret

# 日志
*.log
logs/

# 模型文件
*.h5
*.pt
*.pth
*.ckpt
*.pkl
*.faiss

# 缓存
.cache/
.ipynb_checkpoints/

# 私人文档
resume_docs/
```

## 7. 保留 VS Code 的项目配置

如果希望提交 `.vscode/settings.json`，但忽略 `.vscode` 中的其他文件，可以使用：

```gitignore
.vscode/*
!.vscode/settings.json
```

不要同时写：

```gitignore
.vscode/
```

否则目录本身已经被忽略，后面的排除规则可能无法达到预期效果。

## 8. 检查忽略规则

在仓库根目录执行：

```powershell
git check-ignore -v code\.venv\Scripts\python.exe
```

如果文件被忽略，Git 会显示生效的规则和文件路径。

检查本项目中的私人文档目录：

```powershell
git check-ignore -v resume_docs
git check-ignore -v code\resume_docs
```

查看所有状态，包括被忽略的文件：

```powershell
git status --short --ignored
```

## 9. `.gitignore` 对已跟踪文件无效

`.gitignore` 主要影响尚未被 Git 跟踪的文件。如果文件以前已经提交，即使后来加入 `.gitignore`，Git 仍会继续跟踪它。

取消跟踪文件，但保留本地文件：

```powershell
git rm --cached .env
```

取消跟踪目录，但保留本地目录：

```powershell
git rm -r --cached resume_docs
```

然后提交新的忽略规则：

```powershell
git add .gitignore
git commit -m "Update gitignore rules"
```

## 10. 常见规则对照

| 规则 | 含义 |
|---|---|
| `resume_docs/` | 忽略名为 `resume_docs` 的目录 |
| `resume_docs` | 忽略名为 `resume_docs` 的文件或目录 |
| `*.log` | 忽略所有 `.log` 文件 |
| `/.env` | 只忽略仓库根目录下的 `.env` |
| `**/temp/` | 忽略任意层级下的 `temp` 目录 |
| `!important.log` | 从忽略规则中排除 `important.log` |

## 11. 上传前检查

提交前建议执行：

```powershell
git status --short
git status --short --ignored
git diff --cached --stat
```

确认以下内容没有被提交：

```text
.venv/
.env
密钥文件
模型文件
缓存文件
私人文档
```

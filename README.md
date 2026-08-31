# Codex Pet

这是 Codex 自定义宠物成品仓库。

当前宠物：`lumei`（显示名：噜妹）。每个宠物都放在自己的子目录中，目录名与 `pet.json` 的 `id` 一致。

## 目录结构

```text
main/
├── README.md
└── lumei/
    ├── pet.json
    └── spritesheet.webp
```

`lumei/spritesheet.webp` 是 Codex v2 成品精灵图，尺寸为 `1536×2288`，对应 8 列 × 11 行、每格 `192×208`。`lumei/pet.json` 中的 `spriteVersionNumber: 2` 不要删除或改成其他值。

## 面向 Codex

### 安装约定

Codex 的本地自定义宠物目录是：

```text
${CODEX_HOME:-$HOME/.codex}/pets/<pet-id>/
```

安装 `lumei` 时，必须将下面两个文件放在同一个目录中：

```text
${CODEX_HOME:-$HOME/.codex}/pets/lumei/pet.json
${CODEX_HOME:-$HOME/.codex}/pets/lumei/spritesheet.webp
```

### 从本仓库安装

在仓库根目录执行：

```bash
REPO_DIR="$(pwd)"
PET_ID="lumei"
CODEX_PET_DIR="${CODEX_HOME:-$HOME/.codex}/pets/$PET_ID"

mkdir -p "$CODEX_PET_DIR"
cp -p "$REPO_DIR/$PET_ID/pet.json" \
  "$REPO_DIR/$PET_ID/spritesheet.webp" \
  "$CODEX_PET_DIR/"
```

### 安装后检查

```bash
jq -e '.id == "lumei" and .spriteVersionNumber == 2 and .spritesheetPath == "spritesheet.webp"' \
  "${CODEX_HOME:-$HOME/.codex}/pets/lumei/pet.json"

sips -g pixelWidth -g pixelHeight \
  "${CODEX_HOME:-$HOME/.codex}/pets/lumei/spritesheet.webp"
```

检查结果应为 `1536×2288`。如果 Codex 已经在运行，安装后重新打开 Codex，使其重新发现本地宠物包。

### Codex 维护规则

- 使用 `pet.json` 中的 `id` 作为宠物目录名；当前只有 `lumei`。
- `pet.json` 和 `spritesheet.webp` 必须成对复制。
- 只安装 `lumei/` 内的成品，不要把 README、预览图或中间生成文件复制到 Codex 宠物目录。
- 不要把 8×9 中间图当作成品；v2 成品必须保留 `spriteVersionNumber: 2`。

## 面向 Human

### 方式一：终端安装（推荐）

在本仓库的 `main` 目录执行：

```bash
PET_ID="lumei"
CODEX_PET_DIR="${CODEX_HOME:-$HOME/.codex}/pets/$PET_ID"

mkdir -p "$CODEX_PET_DIR"
cp -p "$PET_ID/pet.json" "$PET_ID/spritesheet.webp" "$CODEX_PET_DIR/"
```

然后关闭并重新打开 Codex。

### 方式二：Finder 手动复制

1. 打开本仓库的 `main/lumei/` 文件夹。
2. 打开 Finder 的前往文件夹，输入 `~/.codex/pets/`。
3. 如果没有 `lumei` 文件夹，就新建一个。
4. 将 `main/lumei/` 中的 `pet.json` 和 `spritesheet.webp` 复制到 `~/.codex/pets/lumei/`。
5. 重新打开 Codex。

### 验证是否安装成功

确认以下两个文件都存在：

```text
~/.codex/pets/lumei/pet.json
~/.codex/pets/lumei/spritesheet.webp
```

在 `pet.json` 中应看到：

```json
{
  "id": "lumei",
  "displayName": "噜妹",
  "spriteVersionNumber": 2,
  "spritesheetPath": "spritesheet.webp"
}
```

如果 Codex 没有显示噜妹，先检查目录名是否为 `lumei`、两个文件是否在同一层，以及是否已经重启 Codex。

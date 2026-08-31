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

将下面整段提示词交给 agent，即可让它从本仓库自动部署成品宠物：

```text
你是 Codex 自定义宠物部署 agent。当前目录是本仓库的 main 根目录。请扫描当前目录下的一级子目录，找出同时包含 pet.json 和 spritesheet.webp 的成品宠物目录；对每个目录读取 pet.json 的 id，并确认 id 与目录名一致、spriteVersionNumber 为 2、spritesheetPath 为 spritesheet.webp。只处理这两个成品文件，不要复制 README、预览图、参考图、原始素材或 8×9 中间图。对每个通过检查的宠物，将文件复制到 ${CODEX_HOME:-$HOME/.codex}/pets/<id>/，先创建目标目录，再用保留文件属性的方式覆盖同名旧文件。复制后重新读取目标 pet.json，并检查 spritesheet.webp 的尺寸为 1536×2288；任何校验失败都立即停止，不要修改仓库内源文件。部署成功后报告每个 pet id、目标路径、spriteVersionNumber、精灵图尺寸和是否需要重启 Codex；如果 Codex 正在运行，提示用户重新打开 Codex 以重新发现宠物包。
```

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

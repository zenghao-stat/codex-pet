# Codex Pet

这是 Codex 自定义宠物成品仓库。

当前宠物：`lumei`（显示名：噜妹）。每个宠物都放在自己的子目录中，目录名与 `pet.json` 的 `id` 一致。

在线索引：[打开宠物展示页](./index.html)

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

将下面整段提示词交给 agent，即可让它从 GitHub 获取并自动部署成品宠物：

```text
你是 Codex 自定义宠物部署 agent。

不要假设当前目录已经有本仓库。请从公开 GitHub 仓库
https://github.com/zenghao-stat/codex-pet.git 获取 main 分支的最新内容：
使用 mktemp -d 创建临时目录，并执行 git clone --depth 1 --branch main。

在 clone 得到的仓库根目录中扫描一级子目录，
找出同时包含 pet.json 和 spritesheet.webp 的成品宠物目录。

对每个成品目录：
1. 读取 pet.json 的 id。
2. 确认 id 与目录名一致。
3. 确认 spriteVersionNumber 为 2。
4. 确认 spritesheetPath 为 spritesheet.webp。
5. 在开始复制前完成全部源文件校验。
   任何源校验失败都立即停止，且不要写入本机 Codex 宠物目录。

完成源校验后，先向用户展示发现的 pet id 列表，并询问：
“要安装哪些宠物？请回复一个或多个 pet id，或回复 all 安装全部。”
在收到明确回复前不要写入本机 Codex 宠物目录。
用户回复 all 时选择全部宠物；用户指定 pet id 时只选择这些宠物。
如果回复为空、含有未知 id 或存在歧义，继续询问，不执行复制。

只处理这两个成品文件：
- 不要复制 README、预览图、参考图或原始素材。
- 不要复制 8×9 中间图。

仅对用户选中的宠物：
1. 将文件复制到 ${CODEX_HOME:-$HOME/.codex}/pets/<id>/。
2. 先创建目标目录。
3. 使用保留文件属性的方式覆盖同名旧文件。
4. 重新读取目标 pet.json。
5. 检查 spritesheet.webp 的尺寸为 1536×2288。

目标校验失败时立即停止并报告失败的 pet id，
不要修改 clone 得到的仓库源文件。

部署成功后，报告每个 pet id、目标路径、spriteVersionNumber、
精灵图尺寸和是否需要重启 Codex。
如果 Codex 正在运行，提示用户重新打开 Codex，以便重新发现宠物包。

确认全部宠物安装成功后，
使用安全的临时目录清理方式删除本次 clone 的临时目录。
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

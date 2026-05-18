---
name: v9-release-checker
description: "技能体系版本发布前的一致性检查工具，检测版本号/日期/frontmatter/引用路径/历史表等小细节疏漏"
version: v2.9.0
agent_created: true
created: 2026-05-16
allowed-tools: Read, Write, Bash
trigger: "版本发布检查、发布前检查、版本一致性检查、一键检查"
tags: ["xmgl", "v9", "release-check", "consistency"]
---

# V9 版本发布一致性检查器

> **定位**：技能体系版本发布前的质量守门人
> **目标**：消灭版本号未同步、日期未更新、引用路径错误、脚注遗漏等小细节疏漏

---

## 必检项清单（6项）

> **使用前必读**：以下所有 Python 脚本中的 `{V9-BASE-DIR}` 为**路径占位符**，使用前请替换为实际的 V9-Base 目录路径（如 `D:\projects\v9-base`）。`TARGET_VERSION` 和 `TARGET_DATE` 也需按本次发布实际情况修改。

每次 V9-Base / V9-Standard / V9-Full 版本升级发布前，必须逐项检查以下内容：

### ✅ 检查 1：版本号同步

**目标**：同一批次发布的技能包内，所有文件版本号必须一致。

```python
import re, os

SKILL_DIR = r'{V9-BASE-DIR}'  # ← 替换为实际V9-Base目录路径
TARGET_VERSION = 'v2.6.0'  # 本次发布版本号

issues = []
for fname in os.listdir(SKILL_DIR):
    if not fname.endswith('.md'):
        continue
    fpath = os.path.join(SKILL_DIR, fname)
    with open(fpath, 'r', encoding='utf-8') as f:
        content = f.read()
    # 检查 frontmatter 内的 version
    m = re.search(r'^version:\s*v?(\d+\.\d+\.\d+)', content, re.MULTILINE)
    if m and m.group(1) != TARGET_VERSION.lstrip('v'):
        issues.append(f'{fname}: frontmatter version={m.group(0)}, expected={TARGET_VERSION}')
    # 检查正文内的 version
    bodies = re.findall(r'\*\*版本\*\*[:：]?\s*v?(\d+\.\d+\.\d+)', content)
    for b in bodies:
        if b != TARGET_VERSION.lstrip('v'):
            issues.append(f'{fname}: body version={b}, expected={TARGET_VERSION.lstrip("v")}')

if issues:
    print('❌ 版本号不一致：')
    for i in issues:
        print(f'  {i}')
else:
    print('✅ 版本号全部一致')
```

### ✅ 检查 2：日期同步

**目标**：同一批次发布的所有文件，日期必须统一（最后更新日期 = 发布日）。

```python
import re, os
from datetime import datetime

SKILL_DIR = r'{V9-BASE-DIR}'  # ← 替换为实际V9-Base目录路径
TARGET_DATE = '2026-05-16'  # 本次发布日期

issues = []
for fname in os.listdir(SKILL_DIR):
    if not fname.endswith('.md'):
        continue
    fpath = os.path.join(SKILL_DIR, fname)
    with open(fpath, 'r', encoding='utf-8') as f:
        content = f.read()
    # 查找日期模式
    dates = re.findall(r'(\d{4}-\d{2}-\d{2})', content)
    # 过滤掉明显不是文件日期的内容（如项目ID、版本号中的日期）
    file_dates = [d for d in dates if d >= '2024-01-01']
    # 检查 frontmatter 的 updated/date 字段
    m = re.search(r'^(updated|date|last_update):\s*(\d{4}-\d{2}-\d{2})', content, re.MULTILINE)
    if m:
        found_date = m.group(2)
        if found_date != TARGET_DATE:
            issues.append(f'{fname}: frontmatter date={found_date}, expected={TARGET_DATE}')

if issues:
    print('❌ 日期不一致：')
    for i in issues:
        print(f'  {i}')
else:
    print('✅ 日期全部一致')
```

### ✅ 检查 3：引用路径有效性

**目标**：文档内的相对引用路径（如 `docs/xxx.md`、`_archive/xxx.md`）必须指向真实存在的文件。

```python
import re, os

SKILL_DIR = r'{V9-BASE-DIR}'  # ← 替换为实际V9-Base目录路径

issues = []
# 扫描所有 .md 文件中的相对路径引用
path_pattern = re.compile(r'\[([^\]]+)\]\(([^)]+\.md)\)')

for fname in os.listdir(SKILL_DIR):
    if not fname.endswith('.md'):
        continue
    fpath = os.path.join(SKILL_DIR, fname)
    with open(fpath, 'r', encoding='utf-8') as f:
        content = f.read()
    # 查找 md 引用
    for match in path_pattern.finditer(content):
        link_text = match.group(1)
        link_path = match.group(2)
        # 解析相对路径
        if not link_path.startswith(('http', '#', '{{')):
            # 相对路径
            ref_file = os.path.normpath(os.path.join(os.path.dirname(fname), link_path))
            if not os.path.exists(os.path.join(SKILL_DIR, ref_file)):
                issues.append(f'{fname}: 引用 "{link_text}" → 路径不存在: {ref_file}')

if issues:
    print('❌ 引用路径错误：')
    for i in issues:
        print(f'  {i}')
else:
    print('✅ 引用路径全部有效')
```

### ✅ 检查 4：frontmatter 格式标准化

**目标**：所有技能文件 frontmatter 必须包含以下字段，且格式统一。

```yaml
---
name: "技能名称"             # 必须，技能的唯一标识名称
description: "一句话说明"     # 必须，技能功能简述
version: v2.9.0
agent_created: true         # 必须，AI创建的技能必须为 true
created: YYYY-MM-DD         # 必须，创建日期
updated: YYYY-MM-DD         # 推荐，最后更新日期
allowed-tools: Read, Write, Bash  # 推荐，技能所需工具
tags: ["xmgl", "..."]      # 推荐
---
```

```python
import re, os

SKILL_DIR = r'{V9-BASE-DIR}'  # ← 替换为实际V9-Base目录路径

REQUIRED_FIELDS = ['name', 'description', 'version', 'agent_created', 'created']
SKIP_FILES = ['STANDARD.md', 'README.md', '00-xmgl框架总览.md', '00-技能全景图.md']

issues = []
for fname in os.listdir(SKILL_DIR):
    if not fname.endswith('.md') or fname in SKIP_FILES:
        continue
    fpath = os.path.join(SKILL_DIR, fname)
    with open(fpath, 'r', encoding='utf-8') as f:
        content = f.read()

    fm_match = re.match(r'^---\n(.*?)\n---', content, re.DOTALL)
    if not fm_match:
        issues.append(f'{fname}: 缺少 frontmatter')
        continue

    fm_text = fm_match.group(1)
    for field in REQUIRED_FIELDS:
        if not re.search(rf'^{field}:', fm_text, re.MULTILINE):
            issues.append(f'{fname}: 缺少必需字段 "{field}"')

if issues:
    print('❌ frontmatter 格式问题：')
    for i in issues:
        print(f'  {i}')
else:
    print('✅ frontmatter 格式全部合规')
```

### ✅ 检查 5：版本历史表更新

**目标**：技能体系总览文件（如 `00-技能全景图.md`）的版本历史表，必须包含本次发布的版本条目。

```python
import re, os

SKILL_DIR = r'{V9-BASE-DIR}'  # ← 替换为实际V9-Base目录路径
HISTORY_FILE = '00-技能全景图.md'
TARGET_VERSION = 'v2.6.0'
TARGET_DATE = '2026-05-16'

fpath = os.path.join(SKILL_DIR, HISTORY_FILE)
with open(fpath, 'r', encoding='utf-8') as f:
    content = f.read()

# 检查版本历史表是否包含本次版本
if TARGET_VERSION in content and TARGET_DATE in content:
    print(f'✅ 版本历史表已包含 {TARGET_VERSION} ({TARGET_DATE})')
else:
    print(f'❌ 版本历史表缺少 {TARGET_VERSION} ({TARGET_DATE})')
    print(f'   请在版本历史表中添加：')
    print(f'   | {TARGET_VERSION} | {TARGET_DATE} | [本次变更说明] |')
```

### ✅ 检查 6：脚注版本号同步

**目标**：每个 .md 文件末尾脚注的版本号必须与本次发布版本一致。

```python
import re, os

SKILL_DIR = r'{V9-BASE-DIR}'  # ← 替换为实际V9-Base目录路径
TARGET_VERSION = 'v2.6.0'

issues = []
for fname in os.listdir(SKILL_DIR):
    if not fname.endswith('.md'):
        continue
    fpath = os.path.join(SKILL_DIR, fname)
    with open(fpath, 'r', encoding='utf-8') as f:
        content = f.read()

    # 查找脚注中的版本号
    footer_matches = re.findall(r'xmgl\s+v(\d+\.\d+\.\d+)', content)
    for m in footer_matches:
        if m != TARGET_VERSION.lstrip('v'):
            issues.append(f'{fname}: 脚注版本 v{m}, expected {TARGET_VERSION}')

if issues:
    print('❌ 脚注版本不一致：')
    for i in issues:
        print(f'  {i}')
else:
    print('✅ 脚注版本全部一致')
```

---

## 执行流程

### 发布前执行步骤

1. **确定本次发布版本号**（如 `v2.6.0`）和**发布日期**（如 `2026-05-16`）
2. **确认目标目录**（V9-Base / V9-Standard / V9-Full）
3. **按顺序执行 6 项检查**
4. **修复发现的问题**（修改对应文件）
5. **重新执行检查**确认全部通过
6. **打包送测**

### 检查不通过处理规则

| 检查项 | 不通过时 |
|:-------|:---------|
| 版本号 | 立即修复（影响最大） |
| 日期 | 立即修复 |
| 引用路径 | 立即修复（影响功能） |
| frontmatter | 建议修复（影响技能加载） |
| 版本历史表 | 建议修复（影响追溯） |
| 脚注 | 建议修复（影响版本辨识） |

---

## 一键执行脚本

将上述 6 项检查整合为单个 Python 脚本：

```python
#!/usr/bin/env python3
"""V9 版本发布一致性检查器"""
import sys, os, re

def main():
    SKILL_DIR = sys.argv[1] if len(sys.argv) > 1 else r'{V9-BASE-DIR}'  # ← 替换为实际V9-Base目录路径
    TARGET_VERSION = sys.argv[2] if len(sys.argv) > 2 else input('输入目标版本号（如 v2.6.0）: ')
    TARGET_DATE = sys.argv[3] if len(sys.argv) > 3 else input('输入发布日期（如 2026-05-16）: ')

    print(f'检查目录: {SKILL_DIR}')
    print(f'目标版本: {TARGET_VERSION}')
    print(f'目标日期: {TARGET_DATE}')
    print('=' * 50)

    # 6项检查（整合上述代码）
    checks = [
        ('版本号同步', check_versions),
        ('日期同步', check_dates),
        ('引用路径', check_paths),
        ('frontmatter', check_frontmatter),
        ('版本历史表', check_history),
        ('脚注版本', check_footer),
    ]

    all_pass = True
    for name, fn in checks:
        print(f'\n[{name}]')
        if not fn(SKILL_DIR, TARGET_VERSION, TARGET_DATE):
            all_pass = False

    print('=' * 50)
    if all_pass:
        print('🎉 所有检查通过，可以发布！')
    else:
        print('⚠️  有检查项未通过，请修复后再发布')

if __name__ == '__main__':
    main()
```

**触发词**：`版本发布检查`、`发布前检查`、`版本一致性检查`

---

## 与 skill-integration-merge 的关系

`v9-release-checker` 是版本融合后的**发布前守门人**：

```
skill-integration-merge（融合）
    ↓
v9-release-checker（发布前检查） ← 本技能
    ↓
打包送测
```

两个技能配合使用，确保：
1. 融合时版本号正确（如 v2.6.0）
2. 发布前 6 项检查全部通过


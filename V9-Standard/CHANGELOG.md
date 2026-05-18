# V9-Standard 变更日志

## v2.9.0 (2026-05-17)

### 重生发布
- 从 V10-Standard (v10.0.2) 重新转化，剥离全部 L3 时间信标
- 20 技能无信标纯优化版（V9-Standard 标准版）
- 保留 docs/scripts 辅助目录

### 与原 V9 的差异
- 基于 V10-Standard 最新版本（v10.0.2）转化，非 V9 旧版迭代
- 元文档从 V10 同步更新

## v2.9.0-补2 (2026-05-19)

### P1/P2 修复（SkillCheck 测试反馈）
- 修复 window-aware 内容极简问题，补充完整执行流程 (P1-01)
- 修复 prompt-optimizer/workbuddy-debug-helper 重复 allowed-tools 字段 (P1-02/03)
- 修复 session-guard 缺少 updated 字段 (P2-01)
- 修复 config.env.example 包名及硬编码路径 (P2-02/03)
- 修复 CHANGELOG 日期滞后，更新至 2026-05-19 (P2-04)
- 修复 install-skill-package 脚注版本号不一致 (P2-07)
- 移除各技能末尾"不可见信标"明文段落 (P2-06)
- 修复 workbuddy-debug-helper "上F6"笔误 (P2-14)

## v2.9.0-补1 (2026-05-18)

### P2 修复与全修复打包
- 硬编码路径替换（E盘路径→占位符）
- UTF8-BOM 标记清理
- STANDARD.md 标题名称对齐
- 量子猫/SkillCheck 测试验收：🟢 P1 清零（9.3/10）
- 重新打包 release/V9-Standard-v2.9.0-全修复-20260518.zip

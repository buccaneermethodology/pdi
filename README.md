# PDI · 问题驱动改进

PDI（Problem Driven Improvement）是一个 Codex 技能。收到代码审查意见、缺陷清单或任务复盘后，可以用它逐条核实问题。对确认的问题，Codex 会追查根因、提出修复方案并验证结果。发现可复用的经验时，再将其写入相应的测试、模板或技能。

## 安装 v0.1.0

在 macOS 或 Linux 上，需要先安装 Git 和 Codex。下面的命令从版本标签下载技能，默认装到当前用户的 `~/.agents/skills/pdi`。如果只想试装，在运行命令前将 `PDI_SKILLS_DIR` 设为临时目录，例如 `PDI_SKILLS_DIR="$(mktemp -d)"`。[Codex 技能目录说明](https://learn.chatgpt.com/docs/customization/overview#skills)

```sh
(
  set -eu
  checkout="$(mktemp -d)"
  git clone --depth 1 --branch v0.1.0 https://github.com/buccaneermethodology/pdi.git "$checkout"

  skills_dir="${PDI_SKILLS_DIR:-$HOME/.agents/skills}"
  mkdir -p "$skills_dir"
  mkdir "$skills_dir/pdi"
  cp -R "$checkout/skills/pdi/." "$skills_dir/pdi"
  diff -r "$checkout/skills/pdi" "$skills_dir/pdi"
  printf '已安装到 %s/pdi\n' "$skills_dir"
)
```

`mkdir` 会在目标 `pdi` 已经存在时停止安装，保留现有文件、目录或符号链接。隔离试装后可用 `unset PDI_SKILLS_DIR` 恢复默认路径。

## 在 Codex 中使用

打开要修复的项目，把审查原文、相关文件和可复现的例子一起交给 `$pdi`。例如，可以把 CSV 导出功能的两条反馈和验证线索一起发给 Codex。

> 使用 $pdi 处理以下审查意见。导出入口在 `src/export_csv.py`，测试在 `tests/test_export_csv.py`。
> 1. 导出公司名 `甲,乙` 时，再读回 CSV 会得到两列。请用这个输入核实。
> 2. 审查者认为导出时丢掉了第一行，目前只有描述。请核对第一行是表头还是数据。
>
> 请逐项给出判断、依据和验证结果。对确认的缺陷修复根因，并说明怎样防止复发。

第一条可以用“写出再读回后，公司名仍是一个完整字段”做验收。第二条需要先确认输入与预期，证据不足时记录还缺什么。完整流程和逐项结论格式见[技能正文](skills/pdi/SKILL.md)。

## 贡献与许可

欢迎提交可复现的问题和改进建议。[贡献指南](CONTRIBUTING.md) · [MIT 许可证](LICENSE)

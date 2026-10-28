# find + xargs 命令速查表 · find + xargs Cheatsheet

[![Stars](https://img.shields.io/github/stars/Ezra-Zhao/find-xargs-cheatsheet?style=social)](https://github.com/Ezra-Zhao/find-xargs-cheatsheet/stargazers) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE) ![Languages](https://img.shields.io/badge/languages-3-blue)

Support & more: [GitHub Sponsors](https://github.com/sponsors/Ezra-Zhao) · [Gumroad — templates & workbooks I use](https://ezraflow28.gumroad.com) · [Ko-fi](https://ko-fi.com/ezra777zhao)


> 🌍 [Español](README.es.md) · [Português](README.pt.md)

一份**一页纸**的 find + xargs 速查表：30 条真实可用的用法——按名字/类型/大小/时间/权限找文件、组合条件时避开 `-o` 与 `-print` 的经典陷阱、用 `-exec`/`-execdir`/`-printf` 就地处理，再把结果安全地喂给 xargs（`-print0`/`-0` 防文件名翻车、`-P` 并行、`-n`/`-I` 控制批次）。每个条目只讲三件事——**它是干什么的、一条能直接复制粘贴的命令、适用场景标签**。不编造参数：全部 30 条都在 GNU findutils 4.9.0 的测试目录里**一条一条真实执行、输出核对过**才入表，语义逐项对照 GNU Findutils 官方手册。

## 面向谁

运维工程师、后端开发者、SRE，以及任何"写 `find` 总要现查 `-exec` 结尾是 `\;` 还是 `+`"、"管道喂给 xargs 遇到带空格的文件名就翻车"的人。如果批量找文件、批量改名、批量清理这几件事总让你卡壳，这张表能让你一次记牢。

## 怎么用

1. 直接看 **[CHEATSHEET.md](CHEATSHEET.md)** ——速查表正文，英文命令全球开发者都无压力。
2. 本地要有 GNU findutils：Linux 自带；macOS 自带的是 BSD 版，个别参数（如 xargs `-r`）语义不同，表里已标注差异。
3. 按"基础 → 过滤 → 组合 → 动作 → xargs"的顺序，在自己的练习目录里每个条目亲手敲一遍，一小时就顺手了。

## 先跑起来（30 秒验证环境）

```bash
find --version | head -1           # 打印版本（如 find (GNU findutils) 4.9.0），说明是 GNU 版
mkdir -p /tmp/play && cd /tmp/play # 练习目录：所有破坏性条目先在这里练
touch app.log notes.txt            # 造两个文件，下面的组合立刻能试
```

## 一条能直接用的黄金组合

```bash
find . -name "*.txt" -print0 | xargs -0 grep -l "TODO"
```

文件名里有空格、引号、换行都不怕：find 用 NUL 分隔输出（文件名唯一不可能包含的字节），xargs `-0` 按 NUL 读入。处理别人家的目录树时，只用这一对。

## 收录了什么

| 分类 | 内容 |
|---|---|
| 🔍 基础 | -name 按名 / -iname 忽略大小写 / -type 按类型 / -path 全路径匹配 / -maxdepth 与 -mindepth 控深度（5 条） |
| 🎯 过滤 | -size 按大小（含取整坑）/ -mtime 与 -mmin 按时间 / -newer 对照文件 / -empty 找空 / -perm 三种权限匹配 / -regex 正则（6 条） |
| 🧩 组合逻辑 | -o 的括号与 ! 取反、动作绑错分支的 -print 陷阱（1 条） |
| ⚙️ 动作 | -exec \; 逐个跑 / -exec {} + 批量跑 / -execdir 进目录跑 / -ok 先问再跑 / -printf 自定义输出 / -ls 列表 / -delete 删除（7 条） |
| 🚀 xargs | 基本管道 / -print0 配 -0 安全对 / -n 限量 / -I 占位替换 / -P 并行 / -t 回显 / -p 先问 / -r 空输入不跑 / -d 定分隔符 / -a 读文件 / -E 结束标记（11 条） |

官方文档只链 https://www.gnu.org/software/findutils/manual/，所有命令均对照 GNU Findutils 手册（find / xargs 两章）核实过语义，并在本机 GNU findutils 4.9.0 上全部实跑通过；示例只在自造的练习目录里操作，不涉及任何真实数据。

## 为什么是这 30 条

- **安全对放第一位**：普通管道按空白切分文件名，带空格的名字直接碎掉——`-print0` + `-0` 是全表最该先记住的一条组合。
- **`;` 和 `+` 差着数量级**：`-exec ... \;` 每个文件起一个进程，`-exec ... {} +` 攒满一条命令行再跑；文件上万时这是分钟和秒的区别。
- **两个坑先说清**：给带 `-o` 的表达式加动作，`-print` 只绑最后一个分支，会静默漏掉一半结果；`-size` 先向上取整再比较，`-size -1M` 只匹配空文件。两个坑都在表里标了 ⚠️ 并给了正解。
- **删除类动作配预演**：`-delete` 没有回收站，`-ok` 与 `xargs -p` 就是它的预演模式——先看清单、再真动手。

## 欢迎 PR

发现命令写错了、参数过时了、翻译不到位——欢迎提 Pull Request。新增条目请保持统一格式（一句话说明 / bash 代码块 / 标签），**只收真实可用的命令，不确定的宁可不收**。

## 📚 更多速查表

同一作者的其他速查表，同样有中/西/葡三语版本：

- [SSH 命令速查表](https://github.com/Ezra-Zhao/ssh-cheatsheet) — 28 条连接、密钥、隧道与加固命令
- [FFmpeg 命令速查表](https://github.com/Ezra-Zhao/ffmpeg-cheatsheet) — 28 条音视频处理命令
- [Git 实战速查表](https://github.com/Ezra-Zhao/git-cheatsheet) — 30 条日常+救命命令
- [Docker 命令速查表](https://github.com/Ezra-Zhao/docker-cheatsheet) — 28 条容器命令
- [curl 命令速查表](https://github.com/Ezra-Zhao/curl-cheatsheet) — 29 条 API 调试命令
- [正则表达式速查表](https://github.com/Ezra-Zhao/regex-cheatsheet) — 30 条实战正则模式
- [jq 命令速查表](https://github.com/Ezra-Zhao/jq-cheatsheet) — 30 条 JSON 处理 filter
- [终端神器速查表](https://github.com/Ezra-Zhao/terminal-superpowers-cheatsheet) — 14 个终端效率工具
- [Adobe 免费替代速查表](https://github.com/Ezra-Zhao/adobe-free-alternatives-cheatsheet) — 16 个 Adobe 免费替代品
- [退订省钱速查表](https://github.com/Ezra-Zhao/desubscribe-cheatsheet) — 10 组退订替代，年省约 $1,260
- [kubectl 命令速查表](https://github.com/Ezra-Zhao/kubectl-cheatsheet) — Kubernetes 常用命令速查
- [SQL 查询速查表](https://github.com/Ezra-Zhao/sql-cheatsheet) — 常用 SQL 查询与函数速查
- [Vim 命令速查表](https://github.com/Ezra-Zhao/vim-cheatsheet) — 30 条编辑、搜索替换与宏命令
- [tmux 命令速查表](https://github.com/Ezra-Zhao/tmux-cheatsheet) — 30 条会话、窗口与窗格命令
- [systemd 命令速查表](https://github.com/Ezra-Zhao/systemd-cheatsheet) — 30 条服务管理与日志命令

## License

MIT © 2026 Guangyi Zhao，详见 [LICENSE](LICENSE)。

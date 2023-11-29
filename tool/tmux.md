# 概念
终端神器，多任务管理大师

# 基本概念

- `session`：会话

- `window`：窗口

- `pane`：面板

# 命令与快捷键

!> 快捷键需要按下前缀 `ctrl+b`,切要进入一个`session`中

[对照表](https://tmuxcheatsheet.com/)

## `session` 会话

| 功能                |                    快捷键                    | 命令                                       |
| ------------------- | :------------------------------------------: | :----------------------------------------- |
| 创建`session`       |                                              | `tmux`                                     |
| 创建并取名`session` |                                              | `tmux new -s `,`tmux new-session `         |
| 列出`session`       |                     `s`                      | `tmux ls`,`tmux list-session`              |
| 退出当前`session`   |                     `d`                      | `tmux detach`                              |
| 连接                |                                              | `tmux attach-session -t {}`,`tmux a -t {}` |
| 关闭`session`       | `x`当所有的`window`关闭时，`session`自动关闭 | `tmux kill-session -t {}`                  |
| 重命名`session`     |            `$` ,`:rename-session`            | `tmux rename-session {}`                   |
| 切换session         |                     `s`                      | `tmux switch-client`                       |

## `window` 窗口

| 功能           |                            快捷键                            | 命令                             |
| -------------- | :----------------------------------------------------------: | -------------------------------- |
| 创建`window`   |                      `:new-window`, `c`                      | `tmux new-window {}`             |
| 关闭`window`   | `&`,直接关闭,<br>`x`,当所有的`pane`关闭时，`session`自动关闭 | `tmux kill-window`直接关闭window |
| 重命名`window` |                     `:rename-window {}`                      | `tmux rename-window {}`          |
| 选择`window`   |                           `数字键`                           | `tmux select-window -t {}`       |
| 列出`window`   |                             `w`                              | `tmux list-window`               |

## `pane` 面板

| 功能                   | 快捷键         | 命令 |
| ---------------------- | -------------- | ---- |
| 水平分屏               | `%`            |      |
| 垂直分屏               | `"`            |      |
| 选择`pane`             | `q` + `数字键` |      |
| 关闭`pane`             | `x`            |      |
| 光标切换`pane`         | `o`            |      |
| 与上一个`pane`交换位置 | `{`            |      |
| 与下一个`pane`交换位置 | `}`            |      |
| 最大化/最小化          | `z`            |      |

## 其他


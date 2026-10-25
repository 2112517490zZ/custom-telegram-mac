# Custom Telegram Mac（Intel）

基于 64Gram `TDesktop-x64/tdesktop@f4be0fa`（1.2.10）。
GitHub Actions 编 **x86_64** Mac 包，给 Intel Mac 用。

## 功能

- 控制面板里自定义帐号上限（填多少就是多少）
- 快捷切号：`Ctrl+Shift+1..9`，`Ctrl+Tab` / `Ctrl+Shift+Tab`
- 左侧帐号搜索（手机号 / 昵称 / 用户名 / id）
- 帐号头像跳动 + 红点：只统计真人私聊，群/频道/机器人不算
- 窗口最右侧独立搜索栏：搜当前帐号全部消息，底部出结果，分类：全部/用户/群组/频道/机器人
- 导入一个 zip 里多个帐号，格式对标模版；也支持拖入
- 按同一模版批量导出 tdata，导出成功后从客户端删除
- 左侧帐号栏可滑动，导入/导出按钮固定

## 编译（需要 GitHub 登录）

这台 Linux 没有 GitHub 帐号，推不上去。在任意已登录 `gh` 的机器：

```bash
cd gh-build
gh auth login
gh repo create custom-telegram-mac --private --source . --remote origin --push
gh workflow run "Mac Intel Release"
```

编完在 Actions 下载 `CustomTelegram` 人工制品。拷到 T9，用隔离 workdir 启动，不要点官方 Telegram。

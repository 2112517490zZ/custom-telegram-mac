# Mac 自定义 Telegram Desktop（按用户 2026-10-20 定稿）

基于 64Gram / `TDesktop-x64/tdesktop`，外观一比一 Desktop。
数据目录独立，不碰官方 Telegram。GitHub Actions 编 Mac 包。

## 1. 自定义帐号上限

- 设置里可填数字，填多少就能挂多少。
- 编译期硬顶 `kMaxAccounts = 500`（再高内存会炸）。
- 界面上限读设置，改数字不用重新编译。

## 2. 快捷切换帐号

- 左侧帐号栏点一下即切。
- 默认快捷键：`Ctrl+1` … `Ctrl+9` 跳到第 1–9 个号；`Ctrl+Tab` / `Ctrl+Shift+Tab` 上一个/下一个。
- 可在 `shortcuts-custom.json` 改。

## 3. 左侧：搜索并跳转帐号

搜索框匹配帐号名称、帐号号码（手机号带不带 +、空格都能搜），也匹配 username / user_id / 导入索引。

回车或点击即切到该号。

## 4. 帐号跳动提示（只要个人消息）

帐号头像/条目跳动 + 红点，仅当未读来自：

- 真人私聊（User，且不是 bot）

明确不算：

- 群组
- 频道
- 机器人

## 5. 右侧：关键词搜消息（独立右栏，不在聊天顶栏）

窗口最右侧单独一列，不要放在会话列表顶部。

- 右侧一个输入框搜当前帐号的全部消息
- 底部列出命中结果，点一条进对应聊天
- 范围包含：用户、群组、频道、机器人
- 分类筛选：全部 / 用户 / 群组 / 频道 / 机器人

（左侧帐号跳动红点仍然只要真人私聊，和右侧搜索分开。）

## 6. 快捷导入：一个 zip 多个 tdata

对标 `/www/wwwroot/模版/+1美国1-3年.zip`：

```
批次名.zip
  批次名/
    手机号/
      手机号.json      # phone, user_id, first_name, twoFA, app_id...
      手机号.session   # Telethon（备用，主用 tdata）
      2FA.txt
      tdata/
        key_datas
        D877F783D5D3EF8Cs
        D877F783D5D3EF8C/maps
```

导入行为：

- 点「导入帐号包」选 zip，或把 zip 拖进窗口
- 递归找所有带 `tdata/key_datas` 的目录
- 忽略 `__MACOSX`、`.DS_Store`
- 每个号作为一个客户端帐号加进去，不覆盖已登录号
- json 里的 phone / 昵称 / username / user_id 写入左侧搜索索引
- 有 2FA.txt 或 json.twoFA 则记下，登录遇到 2FA 自动填（失败再手动）
- 超过当前上限时停下并提示去改上限
- 导入进隔离 workdir，不动官方 Desktop

## 8. 导出 tdata（按同一模版，导出后删除）

- 单个：帐号右键「导出此帐号并删除」
- 批量：设置里「批量导出帐号包」，可勾选，支持按名称/号码筛选
- zip 结构与 `/www/wwwroot/模版/+1美国1-3年.zip` 相同：
  `批次/手机号/{手机号.json, 手机号.session, 2FA.txt, tdata/key_datas, tdata/D877F783D5D3EF8C...}`
- 导出成功后从客户端删除这些号，不留本地副本

## 7. 编译

- GitHub Actions，`macos-latest`，`CMAKE_OSX_ARCHITECTURES=x86_64`（这台 Intel Mac）
- 只跑 Mac Release，不跑 Linux/Windows，避免烧额度
- 产物：`.app` 人工制品，拷到 T9 用隔离启动

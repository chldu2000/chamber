# Chamber

*名字叫密室，但并不是真的“密室”，只是个聊天室。*

*You can call it Chamber, but it's not a real CHAMBER, just a chatroom.*

## 目标

*施工中*

- [x] 基于 TCP 连接的服务器和客户端，实现最简单的即时聊天，多个客户端连接服务器；
- [ ] 给客户端加上一个~~炫酷的~~ TUI；
  - [x] 实现基本上可用的输入框；
  - [ ] 显示消息列表；
  - [ ] 显示当前在线客户端列表（需要完成自定义消息格式）；
  - [ ] 优化上述项目，例如要能够滚动浏览列表等；
- [ ] 使用文件自定义配置，实现客户端自定义昵称等功能；
- [ ] 自定义消息格式来包含更多信息，区分被广播的信息的发送方；
- [ ] 完善启动时的命令行参数功能；
- [ ] 命令模式或快捷键菜单，命令/菜单功能待定；

## 运行

*自己的 Flag 刚立起来，还没打算提供二进制包。*

需要 [Rust 开发环境](https://www.rust-lang.org/learn/get-started)。

```sh
# local server
cargo run -- server
# local client
cargo run -- client
# TUI demo
cargo run -- ui
```

## Bug 记录

0. （22.08.10）借助字符串的 `unicode_width` 来定位 TUI 中光标位置，但是当输入内容有中文等特殊字符时，由于字符所占字节数与显示宽度不相同，移动光标后光标所在的位置与对应字符在字符串中的位置不一定一致，此时修改字符串会导致 `panic`。

      - ```rust
        /// 1二^三
        ///
        /// ^ 表示光标，记字符串第一个字符前的位置为0，光标所在位置为 3（一个汉字的宽度是 2）
        /// 而“1二”的字节长度为 4（一个汉字的长度是 3），所以在这里插入或删除都会出错
        ```
      - **状态：已解决（22.08.12）。**

1. （22.08.12）目前确定光标位置的方法是：使用光标前的字符数量代表光标位置，计算出光标前所有字符宽度之和来定位光标。由于上下移动光标时直接加减整行的宽度，如果输入的内容有汉字等字符，上下移动光标会导致光标没有移动到当前位置的正上/下方。同时在特定的情况下光标的位置会比较异常，例如当前行尾宽度不够容纳一个汉字（记为 C），C 就被放到了下一行，这时光标可能出现在 C 上而不是 C 的前后。

    - ![光标出现在汉字上](https://s2.loli.net/2022/08/12/Pf7sx8DculpqJm5.png)
    - **状态：已解决（22.08.14），可能还需要测试。**

## 相关项目

[tui-rs](https://github.com/fdehau/tui-rs)

[Phoenix-Chen / tui-rs](https://github.com/Phoenix-Chen/tui-rs/tree/optional_trim_end)

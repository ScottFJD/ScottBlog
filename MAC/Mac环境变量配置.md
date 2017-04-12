##Mac 环境变量配置

* 打开terminal终端 输入 [echo $PATH]()  查看当前变量值。
* 输入 [sudo vi ~/.bash_profile]() 按回车后vim打开用户目录下的bash_profile文件。（一定要用[sudo]()，否则没权限保存文件）
* 然后就是vim的操作了 按i 插入文本 输入 [export PATH=$PATH:/Users/fujindong/Library/Android/sdk/platform-tools]() 按esc结束文本插入
* 输入  [:wq]() 保存并退出vim编辑器
* 输入 [echo $PATH]() 查看变量值。

mac 中 [~]()表示用户目录下如上文的 [/User/fujindong]()

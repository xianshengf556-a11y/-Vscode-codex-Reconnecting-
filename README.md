# -Vscode-codex-Reconnecting-
解决 VS Code Codex 登录失败、一直 Reconnecting、卡在 Thinking 等常见问题。 
提供完整 SSH 配置、代理转发、远程开发环境一键搭建方案， 适合在 AutoDL / 极算云 / 腾讯云等 GPU 服务器上使用 AI 编程插件。
问题如图
<img width="1232" height="715" alt="14431395d52ed118bf12b9a04616542d" src="https://github.com/user-attachments/assets/88a6fc09-db7a-4e21-a912-52e88f5aa1e5" />
<img width="364" height="420" alt="555653b768e0ef63150a8d18362c8b28" src="https://github.com/user-attachments/assets/62e7bab7-eeb7-4156-8ee7-bc77adf474a3" />


step1
先看魔法梯子的VPN代理端口是多少，我们以10090为例，有的是7890。<img width="334" height="139" alt="image" src="https://github.com/user-attachments/assets/189f0858-bc75-4827-8ceb-eee34a9acd67" />


step2
打开本机电脑以下路径"C:\Users\（你的用户名）\.ssh\config"用记事本啥的打开都可以，然后在自己服务器字段下面加入RemoteForward 10090 127.0.0.1:10090，然后保存。
图<img width="512" height="438" alt="673a515d49b8dd728e76b646419d4f15" src="https://github.com/user-attachments/assets/b5313d0e-3f49-43f7-97a1-19b54292b581" />



step3
连接服务器，打开Vscode，按ctrl+shift+p,在顶端输入Preferences: Open Remote Settings (JSON)，然后选图里的这个，一般都是第一个。
<img width="646" height="227" alt="4daa99bfb8e2ebd90befe58f5b8399c6" src="https://github.com/user-attachments/assets/e63f458e-f5e4-44e9-b69e-823294aba7ea" />


step4
然后打开后添加这两行
    "http.proxy": "http://127.0.0.1:10090",
    "https.proxy": "http://127.0.0.1:10090",
    注意是有逗号的。如图，记得保存
<img width="622" height="159" alt="984d9bd970c04876cb4c8e85249e9788" src="https://github.com/user-attachments/assets/88e68458-f368-4b28-a0ed-e41729b67b79" />



step5
保存完，重启Vscode,最后最好在终端输入netstat -tuln | grep 10090，测一下是否可以来联通，如图出现这个就OK了
<img width="1041" height="141" alt="a79146ac1a922060a05382f5caf0f626" src="https://github.com/user-attachments/assets/ddf09d3a-8e52-429c-87ff-1c7f5da6bdf5" />



注意！
不管是10090还是7890都以梯子的端口号为准，指令也是，因为一些梯子由于版本原因只能是7890不能是10090。

确保本地能登录codex排除梯子节点问题

如果以上操作不行，就重启Vscode或者梯子





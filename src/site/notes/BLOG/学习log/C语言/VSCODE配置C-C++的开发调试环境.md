---
{"dg-publish":true,"permalink":"/BLOG/学习log/C语言/VSCODE配置C-C++的开发调试环境/"}
---

[[BLOG/学习log/C语言/C语言\|C语言]]
这里默认vscode已经安装好了C/C++的扩展，并安装了MinGW-W64。主要是讲如何配置**task.json**和**launch.json**。

新建完文件夹和c文件之后，可以点一次编译，这样vscode会自动生成一个 **.vscode**的文件夹，里面有**task.json**的文件
![Pasted image 20241019225125.png](/img/user/BLOG/%E5%AD%A6%E4%B9%A0log/Pasted%20image%2020241019225125.png)
之后来建**launch.json**文件，在调试页面下新建**launch.json**
![Pasted image 20241019225647.png](/img/user/BLOG/%E5%AD%A6%E4%B9%A0log/Pasted%20image%2020241019225647.png)
之后添加配置
![Pasted image 20241019225743.png](/img/user/BLOG/%E5%AD%A6%E4%B9%A0log/Pasted%20image%2020241019225743.png)
![Pasted image 20241019230033.png](/img/user/BLOG/%E5%AD%A6%E4%B9%A0log/Pasted%20image%2020241019230033.png)
之后就可以愉快玩耍了。
![Pasted image 20241019230123.png](/img/user/BLOG/%E5%AD%A6%E4%B9%A0log/Pasted%20image%2020241019230123.png)

---
{"dg-publish":true,"permalink":"/BLOG/问题的解决方法和教程/国内上github/"}
---


[[BLOG/问题的解决方法和教程/问题解决方法及教程\|问题解决方法及教程]]


在 `C:\Windows\System32\drivers\etc` 目录下找到hosts文件


在最后一行贴如下


#github  
140.82.113.3 github.com  
185.199.108.153 assets-cdn.github.com  
199.232.69.194 github.global.ssl.fastly.net

保存  

在cmd中输入`ipconfig/flushdns`  更新dns

即可使用



同理，在需要登其他网站时，则在`https://www.ipaddress.com/` 中输入网站，找到能用的ip地址，然后与上面同步骤即可

# 现象
某次push自己的博客后,通过网页github.io查看效果,点击主页跳转页面后发现自己更新的内容为**一团乱码**

# 探针
1. 回到仓库,找到该片md,内容完整无乱码
2. 本地vscode检查保存格式为utf-8,至此排除文章本身的问题
3. 再次回到仓库,在仓库内点击readme.md中跳转链接,显示仓库404

至此确认为主页中**跳转链接**问题

# readme源码错误
在本地vscode中检查readme格式,发现
错误的相对路径:`rookie_bachelor\os_1.md`
将其修改为:`rookie_bachelor/os_1.md`后重新上传,乱码**问题解除**

# one_step_forward
事后在网上查询[相似案例](https://wingwj.github.io/sharing/tech_and_life/garbled_resolution_process_of_githubio.html),发现github.io的页面转化逻辑似乎bug颇多.html的`%20`转化为空格之类的逻辑都不支持,而且错误跳转后页面会变为乱码,而不是干脆地爆出404错误...

* 日后有爬虫自动扒html的项目,可能还需要专门为github.io写一个格式转化脚本
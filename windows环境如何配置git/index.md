# Windows环境如何配置Git？






Git是目前世界上最先进的分布式版本控制系统，如果你是码农，那一定不会陌生。

### 一、下载Git
首先去[官网](https://git-scm.com/)（https://git-scm.com/）下载git。点这里下载最新版本。
![git最新版本](https://img-blog.csdnimg.cn/20200525194645255.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)



下载好之后，然后选择路径安装，使用默认配置，一直点下一步就好了。

###  二、在本地配置用户信息


右键桌面。会发现你的右键菜单多了两个东西，一个是git的图形化操作（Git GUI），这是专门给新手准备的，一般我们用不到。然后还有一个git命令行工具（git Bash），打开git Bash，输入以下命令：

```bash
 git config #查看本机是否配置了个人信息
 git config --global user.name '你的用户名' #定义全局的用户名
 git config --global user.email '你的邮箱'#定义全局的邮件地址
 git config --list #查看配置信息
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525204735687.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)



### 三、本地生成SSH Key

生成公钥和私钥，实现本地和服务器的认证，首先确认本地是否已经有该文件，在用户主目录下（例如本机：C:\Users\Administrator.ssh），如果有再确认该目录下是否有文件id_rsa和id_rsa.pub，如果没有通过以下方法生成。

```bash
ssh-keygen -t rsa -C 'gahowong@qq.com'
```


一路回车

![生成ssh](https://img-blog.csdnimg.cn/20200525202759302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)





生成了两个文件，一个公钥一个私钥。



id_rsa是私钥不要轻易告诉别人，id_rsa.pub是公钥可放心告诉任何人
![生成了两个文件，一个公钥一个私钥](https://img-blog.csdnimg.cn/20200525203021449.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)



### 四、添加公钥到GitLab服务器

用记事本打开，copy本地id_rsa.pub的内容到GitLab
![设置](https://img-blog.csdnimg.cn/20200525203817516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)![shh](https://img-blog.csdnimg.cn/2020052520392069.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)



重新登陆后，看到这个，说明成功了，基本的配置已经完成。
![成功](https://img-blog.csdnimg.cn/20200525205102553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

大功告成！

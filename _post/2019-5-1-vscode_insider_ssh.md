1. 首先在官网下载[vscode insider版](https://code.visualstudio.com/insiders/)
2. 在扩展中安装Remote-SSH
3. 按照[官方流程](https://code.visualstudio.com/docs/remote/troubleshooting#_installing-a-supported-ssh-client)配置本机SSH，但我在公司的电脑上没有配置成功，有成功的同学可以告诉我。按照不常规的流程自己下载一个SSH然后配置[下载地址](https://me.jinchuang.org/sofw/OpenSSHWindows53p1-2.rar)，然后在环境变量中添加``安装路径/bin``
4. 打开vscode insider版，按住``ctrl+shift+P``然后输入SSH，点击Remote-SSH:Open Configuration File，随后打开配置文件，在其中输入：
	```
	# Read more about SSH config files: https://linux.die.net/man/5/ssh_config
	Host -i your_psw_pos_in_windows your_user_name@your_id_address -p端口号 -o IdentitiesOnly=yes -o StrictHostKeyChecking=no
	```
	对应的记得改一下，id_rsa是自己的地址
5. 按住``ctrl+shift+P``然后输入SSH，点击Remote-SSH:Connect To Host，选择自己刚才的配置，即可连接。
6. 如果现有连接过程中可能存在让你输入多次密码或输入多次yes的情况，记得添加-o StrictHostKeyChecking=no即可。
7. 如果在TEMRMINAL处提示``Connected to SSH Host - Please do not close this terminal`` 那么大功告成，选择打开一个文件夹开始快乐的vscode remote开发吧！
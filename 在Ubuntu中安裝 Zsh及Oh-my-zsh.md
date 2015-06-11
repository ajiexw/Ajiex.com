Date:2013-08-25
Tags: Linux

直接用apt-get安裝zsh套件
	
	apt-get install zsh
	zsh --verison

从GitHub下载oh-my-zsh套件

	git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh

如果没有安裝zsh可以直接使用oh-my-zsh的范例zshrc

	cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

看看有什么Theme 可以用

	ls ~/.oh-my-zsh/themes

编辑~/.zshrc更换zsh的theme

	ZSH_THEME="candy"

修改默认shell

	which zsh #看是否在/usr/bin/zsh目录下
	chsh -s /usr/bin/zsh  

看看有什么Plugin可以用

	ls ~/.oh-my-zsh/plugins

编辑~/.zshrc启用Plugin

	plugins=(git git-flow debian grails rvm history-substring-search github gradle svn node npm zsh-syntax-highlighting sublime)

下载zsh-syntax-highlighting plugin


	cd ~/.oh-my-zsh/custom/plugins
	git clone https://github.com/zsh-users/zsh-syntax-highlighting.git

新增自定zsh设定

	cat ~/.oh-my-zsh/custom/xxx.zsh
	alias df='df -h'
	alias h='htop'
	PATH=$PATH:/opt/app/bin/

From:[http://shyuan.github.io/blog/2012/07/10/install-zsh-and-oh-my-zsh-in-ubuntu-linux/](http://shyuan.github.io/blog/2012/07/10/install-zsh-and-oh-my-zsh-in-ubuntu-linux/)

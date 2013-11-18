Date:2013-11-04 20:09
Tags: Linux

Command-T 是一款很不错的 Vim 插件，该插件允许你在 Vim 中快速导航文件，以用于后续的编辑处理。

##安装Command-T

下载 Command-T 后，你可以按如下的步骤来安装：

    vim command-t-0.6.vba
    :so %
    cd ~/.vim/ruby/command-t 
    ruby extconf.rb 
    make 

##安装bug

1、Command-T基于ruby，需要安装ruby：

    sudo aptitude install ruby

2、安装过程中如果出现以下错误： 

    /usr/lib/ruby/1.9.1/rubygems/custom_require.rb:36:in `require’: cannot load such file — mkmf (LoadError)  

安装ruby dev即可:  

    sudo aptitude install ruby1.9.1-dev

3、如果出现: 
 
    make: Nothing to be done for `all’
    
 那么删除~/.vim/ruby/command-t 然后重新安装， 重新make。

4、运行CommandT命令时， 如果出现: 

    command-t.vim requires Vim to be compiled with Ruby support

那么安装vim-nox: 

    sudo aptitude install vim-nox

##设置commandT

在'~/.vimrc'里面添加

    :map <C-t> :w ^M:CommandT  ^M

'^M'用 'ctrl-v + enter' 输入,  保存当前文件是为了避免vim split当前窗口, 如果想在新窗口(table)打开文件，那么把上面的:w改为:tabedit即可。
 

 搜索文件时还可以用'ctrl-n, ctrl-p' 选择上一个和下一个， 打开文件后可以用'ctrl-o, ctrl-i, ctrl-6' 在文件中来回跳转。


##Command-T 的用法

通过 :CommandT 命令呼出 Command-T 窗口，在底部的 >> 提示后输入字符可以缩小匹配的文件范围。

使用 Ctrl-j/Ctrl-k 进行下上移动选择，一旦选好即可按 Enter 来打开文件。


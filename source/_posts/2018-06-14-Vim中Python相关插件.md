---
title: Vim中Python相关插件
tags: VIM
abbrlink: d52f3c59
date: 2018-06-14 08:09:01
---

## [tell-k/vim-autopep8](https://github.com/tell-k/vim-autopep8)
依照pep8的标准自动格式化代码
插件依赖autopep8
```
pip install --upgrade autopep8
```

### 安装插件
```
Plug 'tell-k/vim-autopep8'
```

### 使用
```
:Autopep8
:Autopep8 --range 1 5               # with arguments
:call Autopep8(" --range 1 5")      # with arguments
:'<,'>Autopep8              # range selection
```

### 配置
```
autocmd FileType python noremap <buffer> <F8> :call Autopep8()<CR>
let g:autopep8_max_line_length=119
```



## [Yggdroot/indentLine](https://github.com/Yggdroot/indentLine)
显示缩进指示线
```
Plug 'Yggdroot/indentLine'
```


## [Valloric/YouCompleteMe](https://github.com/Valloric/YouCompleteMe)
YouCompleteMe不支持Anaconda，所以要指定原生Python路径

### 依赖
```
sudo apt-get install build-essential cmake
sudo apt-get install python-dev python3-dev
```

### 配置/安装
```
Plug 'Valloric/YouCompleteMe'
let g:ycm_server_python_interpreter='/usr/bin/python3.5'
```

### 编译
```
cd ~/.vim/bundle/YouCompleteMe
/usr/bin/python3.5 ./install.py
```







参考：
- https://www.jianshu.com/p/f0513d18742a
- https://github.com/Valloric/YouCompleteMe/issues/2876
- https://github.com/JeffyLu/JeffyLu.github.io/issues/15
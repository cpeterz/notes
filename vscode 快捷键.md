##vscode 快捷键

###翻译变量名：alt+shift+t

###setting.json优化

```c++
{
    /*editor*/
    "editor.cursorBlinking": "smooth",//使编辑器光标的闪烁平滑，有呼吸感
    "editor.formatOnPaste": true,//在粘贴时格式化代码
    "editor.formatOnType": true,//敲完一行代码自动格式化
    "editor.smoothScrolling": true,//使编辑器滚动变平滑
    "editor.tabCompletion": "on",//启用Tab补全
    "editor.fontFamily": "'Jetbrains Mono', '思源黑体'",//字体设置，个人喜欢Jetbrains Mono作英文字体，思源黑体作中文字体
    "editor.fontLigatures": true,//启用字体连字
    "editor.detectIndentation": false,//不基于文件内容选择缩进用制表符还是空格
    /*
    因为有时候VSCode的判断是错误的
    */
    "editor.insertSpaces": true,//敲下Tab键时插入4个空格而不是制表符
    "editor.copyWithSyntaxHighlighting": false,//复制代码时复制纯文本而不是连语法高亮都复制了
    "editor.suggest.snippetsPreventQuickSuggestions": false,//这个开不开效果好像都一样，据说是因为一个bug，建议关掉
    "editor.stickyTabStops": true,//在缩进上移动光标时四个空格一组来移动，就仿佛它们是制表符(\t)一样
    "editor.wordBasedSuggestions": false,//关闭基于文件中单词来联想的功能（语言自带的联想就够了，开了这个会导致用vscode写MarkDown时的中文引号异常联想）
    "editor.linkedEditing": true,//html标签自动重命名（喜大普奔！终于不需要Auto Rename Tag插件了！）
    "editor.wordWrap": "on",//在文件内容溢出vscode显示区域时自动折行
    "editor.cursorSmoothCaretAnimation": true,//让光标移动、插入变得平滑
    "editor.renderControlCharacters": true,//编辑器中显示不可见的控制字符
    "editor.renderWhitespace": "boundary",//除了两个单词之间用于分隔单词的一个空格，以一个小灰点的样子使空格可见
    /*terminal*/
    "terminal.integrated.defaultProfile.windows": "Command Prompt",//将终端设为cmd，个人比较喜欢cmd作为终端
    "terminal.integrated.cursorBlinking": true,//终端光标闪烁
    "terminal.integrated.rightClickBehavior": "default",//在终端中右键时显示菜单而不是粘贴（个人喜好）
    /*files*/
    "files.autoGuessEncoding": true,//让VScode自动猜源代码文件的编码格式
    "files.autoSave": "onFocusChange",//在编辑器失去焦点时自动保存，这使自动保存近乎达到“无感知”的体验
    "files.exclude": {//隐藏一些碍眼的文件夹
        "**/.git": true,
        "**/.svn": true,
        "**/.hg": true,
        "**/CVS": true,
        "**/.DS_Store": true,
        "**/tmp": true,
        "**/node_modules": true,
        "**/bower_components": true
    },
    "files.watcherExclude": {//不索引一些不必要索引的大文件夹以减少内存和CPU消耗
        "**/.git/objects/**": true,
        "**/.git/subtree-cache/**": true,
        "**/node_modules/**": true,
        "**/tmp/**": true,
        "**/bower_components/**": true,
        "**/dist/**": true
    },
    /*workbench*/
    "workbench.list.smoothScrolling": true,//使文件列表滚动变平滑
    "workbench.editor.enablePreview": false,//打开文件时不是“预览”模式，即在编辑一个文件时打开编辑另一个文件不会覆盖当前编辑的文件而是新建一个标签页
    "workbench.editor.wrapTabs": true,//编辑器标签页在空间不足时以多行显示
    "workbench.editor.untitled.hint": "hidden",//隐藏新建无标题文件时的“选择语言？”提示（个人喜好，可以删掉此行然后Ctrl+N打开无标题新文件看看不hidden的效果）
    /*explorer*/
    "explorer.confirmDelete": false,//删除文件时不弹出确认弹窗（因为很烦）
    "explorer.confirmDragAndDrop": false,//往左边文件资源管理器拖动东西来移动/复制时不显示确认窗口（因为很烦）
    /*search*/
    "search.followSymlinks": false,//据说可以减少vscode的CPU和内存占用
    /*window*/
    "window.menuBarVisibility": "visible",//在全屏模式下仍然显示窗口顶部菜单（没有菜单很难受）
    "window.dialogStyle": "custom",//使用更具有VSCode的UI风格的弹窗提示（更美观）
    /*debug*/
    "debug.internalConsoleOptions": "openOnSessionStart",//每次调试都打开调试控制台，方便调试
    "debug.showBreakpointsInOverviewRuler": true,//在滚动条标尺上显示断点的位置，便于查找断点的位置
    "debug.toolBarLocation": "docked",//固定调试时工具条的位置，防止遮挡代码内容（个人喜好）
    "debug.saveBeforeStart": "nonUntitledEditorsInActiveGroup",//在启动调试会话前保存除了无标题文档以外的文档（毕竟你创建了无标题文档就说明你根本没有想保存它的意思（至少我是这样的。））
    "debug.onTaskErrors": "showErrors",//预启动任务出错后显示错误，并不启动调试
    /*html*/
    "html.format.indentHandlebars": true//在写包含形如{{xxx}}的标签的html文档时，也对标签进行缩进（更美观）
}

```

### run code 快捷键

ctrl + alt +n

暂停：crtl +alt +m

配置settings.json

```c
"code-runner.runInTerminal": true,//在控制台运行代码，防止乱码和不能输入
"code-runner.executorMap": {
    "javascript": " cls && cd /d $dir && node $fullFileName && pause",
    "python": " cls && cd /d $dir && \"$pythonPath\" -u $fullFileName && pause",
    "bat": " cls && cd /d $dir && $fullFileName"
    /*这是每种语言运行时所执行命令的对应表，因为笔者使用的语言有限，这里只列出了javascript、python和windows批处理的命令，其他语言的命令可自行添加*/
    /*笔者其他博客中可能会有关于对此设置项的添加或删改的内容*/
},
"code-runner.saveFileBeforeRun": true, //运行前自动保存
"code-runner.customCommand": " cls",//这使Ctrl+Alt+K这个快捷键可以快速清空控制台内容
"code-runner.respectShebang": false//我是Windows系统所以不需要按shebang来运行
"code-runner.ignoreSelection": true,//禁用“运行选中部分的代码”功能（个人喜好，感觉这个功能很鸡肋）
//需要注意的是，所有命令前都有一个空格，用来“喂给”上一次运行结尾的“请按任意键继续. . .”

```








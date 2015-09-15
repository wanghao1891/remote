# 5、2015-05-19
* 小技巧，选中文字，然后敲入[]或()，会自动把文字包起来。

# 4、2015-05-18
* Highlight Selected插件报错

  这个暂时没有找到解决方法，就直接把它先关闭了

      Uncaught TypeError: Cannot read property 'dispose' of undefined
      C:\Users\haowang\AppData\Local\atom\app-0.199.0\resources\app.asar\node_modules\event-kit\lib\composite-disposable.js:25

* browser-plus插件引起报错，不能打开任何文件了

  定位是在C:\Users\haowang\.atom\packages\browser-plus\lib\browser-plus.coffee的31行，缺少path的定义。

      Uncaught ReferenceError: path is not defined
      C:\Users\haowang\.atom\packages\browser-plus\lib\browser-plus.coffee:31

  我到该插件的github上看，作者已经提交了修复，我用notepad++，在31行把代码直接复制：
      path = require 'path'

# 3、2015-05-12
* 浏览器插件

  [browser-plus](https://atom.io/packages/browser-plus)

  Real Browser in ATOM

  Here are some feature...

  Live Preview
  Back/Forward Button
  DevTool
  Refresh
  History
  Preview
  Favorites..

* [electron](https://github.com/atom/electron)

  formerly known as Atom Shell

  The Electron framework lets you write cross-platform desktop applications using JavaScript, HTML and CSS. It is based on io.js and Chromium and is used in the Atom editor.

# 2、2015-05-10
* 多行选择插件  
[Sublime Style Column Selection](https://atom.io/packages/Sublime-Style-Column-Selection)

* [官方多行选择方法 Multiple Cursors and Selections](https://atom.io/docs/latest/using-atom-editing-and-deleting-text)

  One of the cool things that Atom can do out of the box is support multiple cursors. This can be incredibly helpful in manipulating long lists of text.

  cmd-click
  Add new cursor

  cmd-shift-L
  Convert a multi-line selection into multiple cursors

  ctrl-shift-up, ctrl-shift-down
  Add another cursor above/below the current cursor

  cmd-D
  Select the next word in the document that is the same as the currently selected word

  ctrl-cmd-G
  Select all words in document that are the same as the one under the current cursor(s)

  Using these commands you can place cursors in multiple places in your document and effectively execute the same commands in multiple places at once.

# 1、2015-05-02
* 项目管理插件  
[project-manager](https://atom.io/packages/project-manager)

* 打开控制面板(Command Palette)  
Mac    ：command + shift + P
Windows：ctrl + shift + P

* 使用了几个ftp的插件，但是都不是很理想
* 高亮选中的内容插件  
  [highlight-selected](https://github.com/richrace/highlight-selected)
find-and-replace

* [路径（三）：准备好工具 — 文本编辑器（Atom）](http://ninghao.net/blog/2073)
* [Atom 常用配置](http://www.rxna.cn/post/wiki/atom-chang-yong-pei-zhi)

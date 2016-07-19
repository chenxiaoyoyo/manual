# 代码段 #

代码段是一个非常有效的工具，可以从一个快捷方式中快速生成常用的代码语法。

比如，输入一些类似于 `habtm` 的语句，然后按下回车键 `Tab`，就会扩展为 `has_and_belongs_to_many`。

许多包自带它们自己具有特定模式的代码段。比如，为 HTML 语法高亮和语法提供支持的 `language-html` 包，提供了许多代码段，来创建一些满足不同需求的 HTML 标签。如果你在 Atom 中创建一个新的 HTML 文件，你可以输 入`html` 然后按下 `Tab`，它会扩展为：

```
<html>
  <head>
    <title></title>
  </head>
  <body>

  </body>
</html>
```

同时它会把光标定位在 `title` 标签的中间，以便你可以立即开始填充这个标签。许多代码段具有多个焦点位置，通过按下 `Tab` 在他们之间切换 —— 比如，在上例HTML代码段之中，当填充完标题标签之后，可以按下 `Tab` 键，然后光标就会移动到 body 标签之间。

要查看当前打开文件拥有的所有可用代码段，你可以按下 `Alt+Shift+S`。

![see all the available snippets](./images/snippets-see-snippets.png)

图 1. 查看当前文件可用代码段

在选择输入框中输入内容，可以使用模糊搜索过滤这个列表。选择其中一个之后会执行光标所在的代码段（或者多个光标所在的代码段）。

## 创建你自己的代码段 ##

所以说这样太爽了。但是，如果有一些语言包中不包含，或者编写代码中自定义的东西，又要怎么做呢？很幸运的是，你可以非常便利地添加自己的代码段。

`%USERPROFILE%\.atom` 目录下的 `snippets.cson` 文本文件，存放了所有自定义的代码段，它们会在 Atom 运行时加载。通过 `File > Snippets` 菜单，打开这个文件。

### 代码段的格式 ###

现在让我们看一看如何编写代码段，基本的代码段格式如下所示：

```
'.source.js':
  'console.log':
    'prefix': 'log'
    'body': 'console.log(${1:"crash"});$2'
```

最顶层的键是选择器，即加载代码段的地方。获知此选择器是什么最简单的方法，是访问你想要添加代码段的语言的语言包，并找到 `Scope` 字符串。

例如，想要添加在 Java 文件中工作的代码段，应该先在 `Settings` 视图中寻找 `language-java` 包，可以看到 Scope 是 `source.java`，因此代码段最顶层的键就应该是它前面加上一个点（就像 CSS 选择器那样）。

![snippet scope](./images/snippet-scope.png)

图 2. Java 文件中的 Scope

下一层的键是代码段的名字，用于在代码段菜单中，以一个更具可读性的方式来描述代码段。可以将代码段命名为任何想要的名字。

在每个代码段的名字下面是 `prefix`，用于触发代码段，以及当代码段被触发后插入 `body`。

每个后面带有数字的 `$` 是 Tab 的停止位置。一旦代码段被触发，通过按下 `Tab` 键来遍历它们。具有相同数字的 Tab 停止位置将会创建多重光标。

上面的例子向 Javascript 文件添加了 `log` 代码段，它会被扩展为：

```
console.log("crash");
```

其中的 `crash` 字符串会在开始时被选中，再次按下 `Tab` 键之后，光标会移动到分号之后。

<div style="background:red;">

高亮文本

</div>

并不像 CSS 选择器，代码段的键每层只能重复一次。如果某一层有重复的键，只有最后的那个会被读到，详见[配置CSON](http://flight-manual.atom.io/using-atom/sections/basic-customization/#configuring-with-cson)。

### 多行代码段主体 ###

可以使用[CoffeeScript 多行语法 ](http://coffeescript.org/#strings)的 `"""` 来创建长模板。

```
'.source.js':
  'if, else if, else':
    'prefix': 'ieie'
    'body': """
      if (${1:true}) {
        $2
      } else if (${3:false}) {
        $4
      } else {
        $5
      }
    """
```

如你所料，存在一个可创建代码段的代码段。如果你打开一个代码段文件，输入 `snip` 之后按下 `Tab`，会将以下内容插入到文件中：

```
'.source.js':
  'Snippet Name':
    'prefix': 'hello'
    'body': 'Hello World!'
```

这样就有了你自定义的代码段。只要保存了文件，Atom 就会重新加载它，你也就能立即使用它了。

代码段功能在 [atom/snippets](https://github.com/atom/snippets) 包中实现。

更多代码段的例子请见[language-html](https://github.com/atom/language-html/blob/master/snippets/language-html.cson)和[language-javascript](https://github.com/atom/language-javascript/blob/master/snippets/language-javascript.cson)包。
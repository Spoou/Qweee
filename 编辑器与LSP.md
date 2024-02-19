当开启“题目页添加编辑器”后，会在题目页下面添加 monaco 代码编辑器。

你可以在其中编写代码，支持自动保存、快捷提交、在线测试运行、LSP、静态补全增强等功能

## 代码自动保存

编辑器中的任何代码改动均会实时自动保存，数据存放在浏览器本地的 IndexedDB 的 CFBetterDB 数据库的 editorCode 表中。

## 在线测试运行

你可以一键测试运行题目中的样例，脚本提供了多个在线代码运行服务：codeforces（官方）、wandbox（第三方）、rextester（第三方），它们所支持的参数选项有所不同。

> [!WARNING]
>
> 显然，所有在线代码运行服务**均无法测试交互式题目**。

> [!TIP]
>
> 如无特殊情况，**建议使用 "Codeforces" 代码运行服务**。

> [!CAUTION]
>
> 第三方在线代码运行服务的运行环境与 codeforces 的可能有所不同，在第三方在线代码运行服务中能够运行或者结果正确的代码并不一定在 codeforces 中同样如此，而且为扩大适用范围，页面上的编译器选项与第三方在线代码运行服务实际的编译器选项也并不是严格对应的，因此很有可能得到意外的结果。
>
> 此外，无法保证你的代码不会被第三方在线代码运行服务以某种方式泄露。

你还可以自定义测试样例，样例数据会自动保存在 CFBetterDB 数据库的 editorCode 表中

## LSP

### 什么是LSP

[LSP（Language Server Protocol](https://microsoft.github.io/language-server-protocol/) 是由微软发起的一种编辑器与语言服务器之间通信的协议规范。

它将传统IDE需要实现的代码补全、诊断、建议、文档甚至项目管理等功能与编辑器相分离，使用独立的服务来完成这些功能，

而编辑器只需要按照协议规范与服务器进行通信即可，这样就大大减少了工作量，同时使编辑器可以轻松地支持各种编程语言

目前LSP的生态已经逐步完善，主流编辑器均已支持LSP或者有了对应的插件，如VSCode、Vim、Emacs等...

但是各大商业IDE厂商对此兴趣不大，基本现有的LSP Server都是开源社区在维护，因此其仍然不及专业IDE以及各种专业的插件

配置好 LSP 后，可以为编辑器提供如下功能（限C++、Python语言）：

- 自动补全、方法签名提示、代码修复
- hover提示、inlay提示、文档链接
- 转到定义、转到引用、符号引用点击高亮
- 格式化、部分格式化、渐进式自动格式化、折叠范围分析、重命名

当然，其不支持本地编译，不支持断点调试，不支持多文件等……，性能上也是不及本地的专门的 IDE 的。

**总之，该功能的实际意义和用处并不大。**

### 如何使用LSP

**第一步：**

脚本提供了对 C++ 和 Python 语言以下两个 LSP Server 实现的支持，你需要按需安装它们：

| 语言   | LSP Server | 安装方式（Windows下）                                        |
| ------ | ---------- | ------------------------------------------------------------ |
| C++    | clangd     | 在 [clangd release](https://github.com/clangd/clangd/releases) 中下载clangd，然后在系统环境变量的 path 中新增 clangd 的路径，例如 `C:\clangd_16.0.2\bin\` |
| Python | pylsp      | 执行命令： `pip install "python-lsp-server[all]"`，其会自动添加环境变量 |

**第二步：**

你需要在 [GitHub release](https://github.com/beijixiaohu/OJBetter/releases) 或者 [蓝奏云(密码:aaaa)](https://mxyowl.lanzouj.com/b0cui6bgj) 中下载并解压 OJBetter_Bridge 软件，它是脚本与 LSP Server 通信的桥梁，

将解压后的文件夹放到C盘根目录 或者其他任何你喜欢的位置 （注意：如果你放在其他位置，你需要在设置面板中要修改"工作路径"为你的实际位置。）

然后你就可以运行 OJBetter_Bridge 了，其提供了GUI版本和终端版本，Windows下推荐使用GUI版本，直接运行 `server_gui.exe` 即可

OJBetter_Bridge 在启动时会自动检查命令 clangd 和 pylsp 是否有效，并给出提示。

完成上述步骤后，你就可以开启设置面板中的 "使用LSP" 并使用了。

> [!TIP]
>
> 在 `server_gui.exe` 的界面中可以设置让其开机自启。

> [!NOTE]
>
> 对于 clangd，在 `cpp_workspace` 文件夹中存放了两个配置文件 `.clangd`、`.clang-format`
>
> 它们分别定义了 clangd 的 [配置项](https://clangd.llvm.org/config) 和 [代码格式化风格](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)，你可以自行编辑修改

## 静态补全增强

你可以在 设置面板-Monaco-静态补全增强-自定义 中为 monaco 编辑器添加自定义的静态代码补全规则，

补全规则应该为 JSON 文件，下面是一个示例(monaco格式):

```json
  {
    "suggestions": [
        {
            "label": "hello world",
            "kind": 15,
            "documentation": "The is a hello world snippet",
            "insertText": "hello world"
        },
        // other suggestions...
    ]
  }
```

你可以使用 [CompletionItem ](https://microsoft.github.io/monaco-editor/docs.html#interfaces/languages.CompletionItem.html)中几乎所有的属性，(注意: 不要填写 range 属性，脚本会自动计算并添加该属性)

### 使用本地的 JSON 文件

由于浏览器的安全限制，你并不能直接使用形如 `file:///xxx` 的 URL 来在浏览器中访问你的本地 JSON 文件。

不过你可以借助 [OJBetter_Bridge](https://github.com/beijixiaohu/OJBetter/releases) 软件来做到，很简单，你只需要将 JSON 文件 `xxx.json` 放置到 OJBetter_Bridge 的 `/mycomplet` 文件夹中，然后启动 OJBetter_Bridge，

现在你就可以在设置面板中新建一个配置了，JSON URL为：`http://127.0.0.1:2323/mycomplet/xxx.json`。

> [!TIP]
>
> **试一试？**
>
> 启动 OJBetter_Bridge，初始情况下 /mycomplet 文件夹中已经自带了一个示例 JSON 文件：`template.json`，
>
>  因此你可以添加一个配置，填写`http://127.0.0.1:2323/mycomplet/template.json` 并保存
>
> 现在在编辑器中输入 `hello`，你将会看到两个补全项：hello world、hello world2

## 常见问题

 ### 为什么 clangd 格式化无效？

这是因为你在脚本设置面板中填写的 LSP 工作路径 不正确

### 为什么 clangd 无法识别我的 `<bits/stdc++.h>` 头文件？

这是因为 `<bits/stdc++.h>` 不是标准头文件，因此 clangd 默认不支持，

你需要在 OJBetter_Bridge 的 `cpp_workspace` 文件夹中的 `.clangd` 文件中通过修改编译器参数来告诉 clang 去哪找到这个非标准的头文件（当然也不推荐使用万能头）。
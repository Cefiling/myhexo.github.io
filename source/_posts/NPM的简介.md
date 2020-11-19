---
title: NPM的简介
date: 2020-11-01 19:07:18
tags:
---

# NPM的简介

​		npm是Node.js能够如此成功的主要原因之一。npm团队做了很多的工作，以确保npm保持向后兼容，并在不同的环境中保持一致。

npm是围绕着语义版本控制的思想而设计的，下面是从他们的网站摘抄过来的：

*给定一个版本号：主版本号.次版本号.补丁版本号， 以下这三种情况需要增加相应的版本号:*

- *主版本号： 当API发生改变，并与之前的版本不兼容的时候*
- *次版本号： 当增加了功能，但是向后兼容的时候*
- *补丁版本号： 当做了向后兼容的缺陷修复的时候*

npm使用一个名为package.json的文件，用户可以通过npm install --save命令把项目里所有的依赖项保存在这个文件里。

例如，运行npm install --save lodash会将以下几行添加到package.json文件中。

```text
"dependencies": {
    "lodash": "^4.17.4"
}
```

请注意，在版本号lodash之前有个^字符。这个字符告诉npm，安装主版本等于4的任意一个版本即可。所以如果我现在运行npm进行安装，npm将安装lodash的主版本为4的最新版，可能是 lodash@4.25.5（@是npm约定用来确定包名的指定版本的）。

理论上，次版本号的变化并不会影响向后兼容性。因此，安装最新版的依赖库应该是能正常工作的，而且能引入自4.17.4版本以后的重要错误和安全方面的修复。

但是，另一方面，即使不同的开发人员使用了相同的package.json文件，在他们自己的机器上也可能会安装同一个库的不同种版本，这样就会存在潜在的难以调试的错误和“在我的电脑上…”的情形。

大多数npm库都严重依赖于其他npm库，这会导致嵌套依赖关系，并增加无法匹配相应版本的几率。

虽然可以通过npm config set save-exact true命令关闭在版本号前面使用的默认行为，但这个只会影响顶级依赖关系。由于每个依赖的库都有自己的package.json文件，而在它们自己的依赖关系前面可能会有符号，所以无法通过package.json文件为嵌套依赖的内容提供保证。

为了解决这个问题，npm提供了shrinkwrap命令。此命令将生成一个npm-shrinkwrap.json文件，为所有库和所有嵌套依赖的库记录确切的版本。

然而，即使存在npm-shrinkwrap.json这个文件，npm也只会锁定库的版本，而不是库的内容。即便npm现在也能阻止用户多次重复发布库的同一版本，但是npm管理员仍然具有强制更新某些库的权力。

这是引用自shrinkwrap文档的内容：

*如果你希望锁定包中的特定字节，比如是为了保证能正确地重新部署或构建，那么你应该在源代码控制中检查依赖关系，或者采取一些其他的机制来校验内容，而不是靠校验版本。*

npm 2会安装每一个包所依赖的所有依赖项。如果我们有这么一个项目，它依赖项目A，项目A依赖项目B，项目B依赖项目C，那么依赖树将如下所示：

```text
node_modules
- package-A
-- node_modules
--- package-B
----- node_modules
------ package-C
-------- some-really-really-really-long-file-name-in-package-c.js
```

这个结构可能会很长。这对于基于Unix的操作系统来说只不过是一个小烦恼，但对于Windows来说却是个破坏性的东西，因为有很多程序无法处理超过260个字符的文件路径名。

npm 3采用了扁平依赖关系树来解决这个问题，所以我们的3个项目结构现在看起来如下所示：

```text
node_modules
- package-A
- package-B
- package-C
-- some-file-name-in-package-c.js
```

这样，一个原来很长的文件路径名就从./node_modules/package-A/node_modules/package-B/node-modules/some-file-name-in-package-c.js变成了/node_modules/some-file-name-in-package-c.js。

你可以在这里阅读到更多有关NPM 3依赖解析的工作原理。

这种方法的缺点是，npm必须首先遍历所有的项目依赖关系，然后再决定如何生成扁平的node_modules目录结构。npm必须为所有使用到的模块构建一个完整的依赖关系树，这是一个耗时的操作，是npm安装速度慢的一个很重要的原因。

由于我没有详细了解npm的变化，所以我想当然的以为每次运行npm install命令时，NPM都得从互联网上下载所有内容。

但是，我错了，npm是有本地缓存的，它保存了已经下载的每个版本的压缩包。本地缓存的内容可以通过npm cache ls命令进行查看。本地缓存的设计有助于减少安装时间。

总而言之，npm是一个成熟、稳定、并且有趣的包管理器。
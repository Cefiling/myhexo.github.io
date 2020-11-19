---
title: Yarn的简介
date: 2020-11-01 19:08:04
tags:
---

# Yarn的简介

“Yarn是由Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具 ，正如官方文档中写的，Yarn 是为了弥补 npm 的一些缺陷而出现的。”这句话让我想起了使用npm时的坑了：

`npm install`的时候巨慢。特别是新的项目拉下来要等半天，删除node_modules，重新install的时候依旧如此。
同一个项目，安装的时候无法保持一致性。由于package.json文件中版本号的特点，下面三个版本号在安装的时候代表不同的含义。

```java
"5.0.3",		// 5.0.3”表示安装指定的5.0.3版本
"~5.0.3",		//～5.0.3”表示安装5.0.X中最新的版本
"^5.0.3",		//^5.0.3”表示安装5.X.X中最新的版本
```


这就麻烦了，常常会出现同一个项目，有的同事是OK的，有的同事会由于安装的版本不一致出现bug。

**安装的时候，包会在同一时间下载和安装，中途某个时候，一个包抛出了一个错误，但是npm会继续下载和安装包。因为npm会把所有的日志输出到终端，有关错误包的错误信息就会在一大堆npm打印的警告中丢失掉，并且你甚至永远不会注意到实际发生的错误。**

## Yarn的优点

**速度快** 。速度快主要来自以下两个方面：

- **并行安装**：无论 npm 还是 Yarn 在执行包的安装时，都会执行一系列任务。npm 是按照队列执行每个 package，也就是说必须要等到当前 package 安装完成之后，才能继续后面的安装。而 Yarn 是同步执行所有任务，提高了性能。
- **离线模式**：如果之前已经安装过一个软件包，用Yarn再次安装时之间从缓存中获取，就不用像npm那样再从网络下载了。
- **安装版本统一**：为了防止拉取到不同的版本，Yarn 有一个锁定文件 (lock file) 记录了被确切安装上的模块的版本号。每次只要新增了一个模块，Yarn 就会创建（或更新）yarn.lock
  这个文件。这么做就保证了，每一次拉取同一个项目依赖时，使用的都是一样的模块版本。npm 其实也有办法实现处处使用相同版本的
  packages，但需要开发者执行 npm shrinkwrap 命令。这个命令将会生成一个锁定文件，在执行 npm install
  的时候，该锁定文件会先被读取，和 Yarn 读取 yarn.lock 文件一个道理。npm 和 Yarn 两者的不同之处在于，Yarn
  默认会生成这样的锁定文件，而 npm 要通过 shrinkwrap 命令生成 npm-shrinkwrap.json
  文件，只有当这个文件存在的时候，packages 版本信息才会被记录和更新。
- **更简洁的输出**：npm 的输出信息比较冗长。在执行 npm install 的时候，命令行里会不断地打印出所有被安装上的依赖。相比之下，Yarn 简洁太多：默认情况下，结合了
  emoji直观且直接地打印出必要的信息，也提供了一些命令供开发者查询额外的安装信息。
- **多注册来源处理**：所有的依赖包，不管他被不同的库间接关联引用多少次，安装这个包时，只会从一个注册来源去装，要么是 npm 要么是 bower, 防止出现混乱不一致。 更好的语义化： yarn改变了一些npm命令的名称，比如 yarn add/remove，感觉上比
  npm 原本的 install/uninstall 要更清晰。

## Yarn和npm命令对比

| npm                          | yarn                 |
| ---------------------------- | -------------------- |
| npm install                  | yarn                 |
| npm install react --save     | yarn add react       |
| npm uninstall react --save   | yarn remove react    |
| npm install react --save-dev | yarn add react --dev |
| npm update --save            | yarn upgrade         |

1、查看版本

```java
yarn --version
npm -version(或者 node -v)
```

2、安装淘宝镜像

```java
 yarn config set registry 'https://registry.npm.taobao.org'     
 npm install -g cnpm --registry=http://registry.npm.taobao.org 
```

3、初始化某个项目

```java
  yarn init                                                  
  npm init 
```

4、默认安装项目依赖

```java
  yarn install                                            
  cnpm install 
```

5、安装某个依赖，并且默认保存到package

```java
yarn add xxx                                        
cnpm install xxx --save
```

6、卸载某个项目依赖

```java
   yarn remove xxx                                    
   cnpm uninstall xxx --save
```

7、更新某个项目依赖

```java
yarn upgrade xxx                                  
cnpm update xxx --save
```

8、安装某个全局的项目依赖

```java
  yarn global add xxx                                
  cnpm install xxx -g 
```

9、安装某个特定版本号的项目依赖

```java
  yarn add xxx@                                       
  cnpm install xxx@1.2.33 --save 
```

10、发布/登录/登出，一系列NPM Registry操作

```java
   yarn publish/login/logout                         
   npm publish/login/logout 
```

11、运行某个命令

```java
yarn run/test                                           
npm run/test
```

## yarn缓存命令

### 查看已缓存包的列表

```shell
yarn cache list
```

### 查询cache缓存文件目录路径

```shell
yarn cache dir
```

### 清理缓存包

- **yarn 清除缓存**

```shell
yarn cache clean
```

- **npm清除缓存**

```shell
npm cache clean --force
```

### 改变 yarn 的缓存路径

```shell
yarn config set cache-folder <path>

// 通过指定 --cache-folder 的参数来指定缓存目录
yarn <command> --cache-folder <path>
```
---
title: git-cz
date: 2020-07-29 16:30:54
tags: github git-cz 命令行工具
---


## git-cz 一款git commit  规范统一工具

![git-cz](./git-cz/git-cz.png)


 简而言之，git commit 就是你在做一次修改后类似于写一个备注，现在安装了commitizen后，你可以使用git cz取代git commit，每次提交的时候可以选择本次commit的类型，这样commit的文本会更具有可读性。

#### 全局安装

```
npm install -g git-cz
git-cz
```


#### 与Commitizen在本地安装

```
npm install -g commitizen
npm install --save-dev git-cz
```

> package.json:

```
{
  "config": {
    "commitizen": {
      "path": "git-cz"
    }
  },
}
```

**run**

```
git cz
```

---
title: 部署 Github Pages🔥
date: 2023-03-22
categories:
  - 前端
tags:
  - Github
sticky: 3
---

在项目根目录下创建一个脚本文件 deploy.sh，注意修改一下对应的用户名和仓库名
```
#!/usr/bin/env sh

# 确保脚本抛出遇到的错误
set -e

# 生成静态文件
npm run build

# 进入生成的文件夹
cd dist

git init
git add -A
git commit -m 'deploy'

# 如果发布到 https://<USERNAME>.github.io/<REPO>
git push -f git@github.com:XXX/XXX.git master:gh-pages

cd -
```

新建一个  Git Bash 终端，执行 sh deploy.sh
项目就会开始构建，然后上传到远程仓库上。过一会就可以在 github 项目 Settings -> Pages 中看到最后的地址。
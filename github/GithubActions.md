---
title: GithubActions使用教程
date: 2024-11-27 13:23:00
author: 长白崎
categories:
  - ["GithubActions"]
tags:
  - "Github"
---

# GitHubActions使用教程

## 关于GitHubActions





## GitHubActions Yaml模板

```yaml
name: Actions Name # 这个表示的是你当前Actions执行工程的别名，根据你自己需求随便写就行
on:
  push:
  	tags:
  	  - 'v*' # Trigger on version tags 触发的版本标签，就是说如果有以推送v....开头的tag标签push那么就会触发这个actions
permissions:
  contents: write # 这个意思是同意GitHubrelease写入权限，你如果要同时进行release发布则必须要写这个
env:
  PROJECT_NAME: ${{github.event.repository.name}} #使用仓库名作为项目名称
  BUILD_DATE: ${{github.ref_name}}-${{github.run_id}} #获取当前日期
jobs:
  build:
    name: Build and Release
    runs-on: ubuntu-latest #表示以Ubuntu系统环境构建
    steps:
    - name: Checkout code #这个是构建步骤名称，根据自己需求随便写就行
      uses: actions/checkout@v4 # 这啥玩意
      with:
        fetch-depth: 0 #获取完整的Git历史
    - name: Set up Go #步骤名称，按照实际情况取名就行，在这里是作为go环境安装的步骤进行
      uses: actions/setup-go@v5
      with:
        go-version-file: 'go.mod' #使用go.mod文件指定Go版本，其实也可以直接手动指定版本
        cache: true # 启用依赖缓存
    - name: Get dependencies #步骤名称，按照自己需要随便取名
      run: go mod download #run是执行指令的意思，这里的话代表下载对应项目的依赖项，熟悉go语言的应该懂得
      
    - name: Prepare Release Directory
      run: mkdir -p release #创建一个名为release的文件夹
    
    - name: Build Linux AMD64
      run: > #这里是执行指令
      	GOOS=linux GOARCH=amd64 go build -o release/${{env.PROJECT_NAME}}_${{env.BUILD_DATE}}_linux_amd64
    - name: Build linux ARM64
      run: >
```


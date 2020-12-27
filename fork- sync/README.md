# 使用 Github Actions 实现自动同步更新源仓库代码

## 1、GitHub Actions 简介

```
Automate your workflow from idea to production.
GitHub Actions makes it easy to automate all your software workflows, now with world-class CI/CD. Build, test, and deploy your code right from GitHub.
Make code reviews, branch management, and issue triaging work the way you want.
```

## 2、配置文件
先 for 一个仓库，然后创建配置文件 `.github/workflows/sync.yml`利用GitHub Actions 来做 fork 同步,具体如下:

``` 
# .github/workflows/sync.yml
name: Sync Fork

on:
  push: # push 时触发, 主要是为了测试配置有没有问题
  schedule:
    - cron: '0 * * * *' # 每小时 0分触发
jobs:
  repo-sync:
    runs-on: ubuntu-latest
    steps:
      - uses: TG908/fork-sync@v1.1
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }} # 这个 token action 会默认配置, 这里只需这样写就行
          owner: mangoclover # fork 上游项目 owner
          head: master # fork 上游项目需要同步的分支
          base: master # 需要同步到本项目的目标分支 
```     

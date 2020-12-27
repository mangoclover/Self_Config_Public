# 使用 Github Actions 实现自动同步更新源仓库代码
***
## 1、GitHub Actions 简介

    Automate your workflow from idea to production.
    GitHub Actions makes it easy to automate all your software workflows, now with world-class CI/CD. Build, test, and deploy your code right from GitHub.
    Make code reviews, branch management, and issue triaging work the way you want.
***
## 2、配置文件
利用GitHub Actions 来做 fork 同步也很简单, 只需创建个配置文件 `.github/workflows/sync.yml`:

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
配置好以后, 如果上游项目更新的话, 我们的 **action** 会在触发时帮我们更新代码, 会创建 **pr** 并且会自动 **merge**. 更多参数请查看 [Marketplace/Actions/Fork Sync](https://github.com/marketplace/actions/fork-sync)

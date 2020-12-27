# 使用 Github Actions 实现自动同步更新源仓库代码

## 1、GitHub Actions 简介

GitHub Actions 帮助您自动完成软件开发周期内的任务。 GitHub Actions 是事件驱动的，意味着您可以在指定事件发生后运行一系列命令。 例如，每次有人为仓库创建拉取请求时，您都可以自动运行命令来执行软件测试脚本。

此示意图说明如何使用 GitHub Actions 自动运行软件测试脚本。 事件会自动触发其中包作业的工作流程。 然后，作业使用步骤来控制操作运行的顺序。 这些操作是自动化软件测试的命令。
![](https://docs.github.com/assets/images/help/images/overview-actions-simple.png "流程图")

> Automate your workflow from idea to production.
> GitHub Actions makes it easy to automate all your software workflows, now with world-class CI/CD. Build, test, and deploy your code right from GitHub.
> Make code reviews, branch management, and issue triaging work the way you want.

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
配置好以后, 如果上游项目更新的话, 我们的 **action** 会在触发时帮我们更新代码, 会创建 **pr** 并且会自动 **merge**. 更多参数请查看 [Marketplace/Actions/Fork Sync](https://github.com/marketplace/actions/fork-sync)

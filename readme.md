https://www.cnblogs.com/stulzq/p/16505837.html

三.Azure 静态 Web 应用#
1.创建#
访问 https://portal.azure.com/ 登录 Azure，如果没有账户可以去注册一个。

找到静态 Web 应用服务。

image-20220722145327975

计划类型选择：免费

区域选择：East Asia（香港）

image-20220722145508741

部署详细信息可以直接选择 Github，然后选择对应的仓库，Azure 会自动在仓库里设置 Github Actions，本文为了演示就选择其他，在创建后手动配置。

2.配置#
在你的博客根目录下创建 .github/workflows目录，然后创建 github action 文件: static-web-app.yml。

name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
      - master

jobs:
  build_and_deploy_job:
    if: github.event_name == 'push'
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true
      - name: Build And Deploy
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for GitHub integrations (i.e. PR comments)
          action: 'upload'
          app_location: '/'
          output_location: 'public'
          app_build_command: 'npm run deploy'
3.配置 SECRET#
进入你的博客仓库 Settings => Security => Secrets => Actions

新建一个 Secret，名称 AZURE_STATIC_WEB_APPS_API_TOKEN

image-20220722150410184

令牌在 Azure 获取：

image-20220722150437087

四.测试#
现在 push 任意 commit 都会触发 Github Actions 部署你的博客

image-20220722150608212

可以访问 Azure 提供的 URL 来访问

image-20220722150645365

五.自定义域#
访问 设置 => 自定义域，可以配置自己的域名

image-20220722150801878

六.额度#
关于免费额度如下

image-20220722150901863

1

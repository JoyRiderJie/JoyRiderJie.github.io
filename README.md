## 简介
这是一个借助github创建个人博客的模板，可以快速创建一个个人博客，并且支持markdown语法。

## 使用方法
1. 在github上创建一个仓库，仓库名为`用户名.github.io`，例如`zhangsan.github.io`。
2. 在本地创建一个文件夹，例如`zhangsan.github.io`，然后clone仓库到本地。
3. 在`zhangsan.github.io`文件夹中创建一个`template1`文件夹，用于存放博客内容。
4. 在`template1`文件夹中创建一个markdown文件，例如`2022-01-01-first-blog.md`，在markdown文件中编写博客内容，可以使用markdown语法。
5. 修改`_coverpage.md`、`_navbar.md`、`_sidebar.md`、`gitignore.md`、`blog-readme.md`、`index.html`等文件，用于设置博客样式、导航栏、侧边栏、忽略文件、readme文件、首页等。
6. 在github上提交代码，等待github自动部署。
7. . 在浏览器中输入`https://用户名.github.io`，即可访问博客。

## 目录解释

|---------image1:存储根目录图片信息

|---------information:存储网站信息

|--------------------about.md:网站详细信息

|--------------------contact.md:联系方式

|--------------------links.md:友情链接

|--------------------donate.md:捐赠信息

|--------------------changelog.md:网站更新日志信息

|---------template1:存储博客内容目录1

|--------------------image:存储博客图片信息

|--------------------paper1.md:第一篇博客

|--------------------paper2.md:第二篇博客

|---------template1:存储博客内容目录2

|--------------------image:存储博客图片信息

|--------------------paper1.md:第一篇博客

|---------_coverpage.md: 封面页面是访问者在访问你的文档网站时首先看到的页面。可以包含项目或文档的标题、描述或副标题、作者或组织信息等信息。

|---------_navbar.yml:导航栏设置_

|---------_sidebar.md:博客列表

|---------.gitignore:git忽略文件

|---------.nojekyll:.一个空文件，通常放置在 GitHub Pages 网站的根目录下。它的主要作用是告诉 GitHub 不要为该网站运行 Jekyll 构建过程。

|---------blog-readme.md:网站的readme文件，可用于展示个人信息

|---------index.html:首页设置

|---------README.md:项目说明文件

## 博客创建示例
1. 在`template1`文件夹中创建一个`2022-01-01-first-blog.md`文件，内容如下：
```
# 第一篇博客

这是我的第一篇博客。
```
2. 在`_sidebar.md`文件中添加以下内容：
```
* [第一篇博客](2022-01-01-first-blog.md)
```
3. 在github上提交代码，等待github自动部署。
4. 在浏览器中输入`https://用户名.github.io`，即可访问博客。

## 其他信息设置
1. 在`information\about.md`文件中可添加网站的详细信息，包括网站标题、网站描述、网站关键词等。
2. 在`information\contact.md`文件中可添加联系方式，包括邮箱、QQ、微信等。
3. 在`information\links.md`文件中可添加友情链接。
4. 在`information\donate.md`文件中可添加捐赠信息。
5. 在`information\changelog.md`文件中可添加网站更新日志信息。

## 注意事项
1. 请确保在提交代码时，不要将敏感信息提交到github上，例如密码、密钥等。
2. 请确保在提交代码时，不要将网站内容泄露给他人，以免被他人利用。

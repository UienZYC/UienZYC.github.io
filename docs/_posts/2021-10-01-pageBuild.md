layout: post
title:  "Github Pages的建立过程"
date:   2021-10-01 22:49:12 +0800
categories: jekyll update

# Github Pages

_this notes track my acts for using github pages_

_xuling_

***

1. 安装ruby

   _Ruby 是一种开源的面向对象程序设计的服务器端脚本语言，在 20 世纪 90 年代中期由日本的松本行弘（まつもとゆきひろ/Yukihiro Matsumoto）设计并开发。_
   
   ```bash
   $ sudo apt-get install ruby-full
   ## 检查安装
   $ ruby --version
   ```
   
   ```
   sudo gem update --system #更新rubygem
   ```
   
   检查是否安装了bundler
   
   ` bundler --version`这会显示版本号
   
   或者`bundler -v`
   
2. 安装Jekyll (这个gem就是在使用ruby)

   根据https://jekyllrb.com/docs/installation/#requirements
   先要做三项检查：

   ```bash
   ruby -v
   gem -v
   gcc -v or g++ -v
   make -v
   ```

   然后找到https://jekyllrb.com/docs/installation/windows/

   为windows10 的wsl安装的方法

   进行如下操作：

   ```bash
   # 升级所有已安装软件
   sudo apt-get update -y && sudo apt-get upgrade -y
   # 然后进行一些安装ruby的操作，因为之前的操作里有，这里就不再做了。
   ```

   然后就可以安装jekyll了。

   ```bash
   $ sudo gem install jekyll bundler
   ```

   检查jekyll版本

   ```bash
   jekyll -v
   ```

3. 创建并检出 `gh-pages` 分支

   ```bash
   $ git checkout --orphan gh-pages
   ```

4. 建立一个本地git仓库

   ```bash
   $ git init
   ```

5. ```shell
   在githubpage那里把源切换为docs，然后这样做，如果没有（默认/root，就不需要这一步）
   $ mkdir docs
   # Creates a new folder called docs
   $ cd docs
   ```

6. 如果选择从 `gh-pages` 分支发布站点，则创建并检出 `gh-pages` 分支。

   ```shell
   $ git checkout --orphan gh-pages
   # 这是一个孤儿分支，它没有父对象，也就是说没有历史提交记录
   ```

7. 创建新 Jekyll 站点，请使用 `jekyll new` 命令：

   ```bash
   $ jekyll new --skip-bundle ./  #这个当前目录不可省略
   # 运行后显示：
   New jekyll site installed in #/*/docs.#
   Bundle install skipped.
   （当然，这里有我建立的路径）
   ```
   
8. 打开 Jekyll 创建的 Gemfile 文件

9. 将 "#" 添加到以 `gem "jekyll "` 开头的行首，以注释此行。

10. 编辑以 `# gem "github-pages"` 开头的行来添加 `github-pages` gem。 将此行更改为：

    ```shell
    gem "github-pages", "~> GITHUB-PAGES-VERSION", group: :jekyll_plugins
    ```

    将 *GITHUB-PAGES-VERSION* 替换为 `github-pages` gem 的最新支持版本。 您可以在这里找到这个版本：“[依赖项版本](https://pages.github.com/versions/)”。

    正确版本 Jekyll 将安装为 `github-pages` gem 的依赖项。

11. 保存并关闭 Gemfile。

12. 运行 `bundle install`

13. （可选）对 `_config.yml` 文件进行任何必要的编辑。 当仓库托管在子目录时相对路径需要此设置。 更多信息请参阅“[将子文件夹拆分到新仓库](https://docs.github.com/cn/github/getting-started-with-github/using-git/splitting-a-subfolder-out-into-a-new-repository)”。

    ```yml
    domain: my-site.github.io       # if you want to force HTTPS, specify the domain without the http at the start, e.g. example.com
    url: https://my-site.github.io  # the base hostname and protocol for your site, e.g. http://example.com
    baseurl: /REPOSITORY-NAME/      # place folder name if the site is served in a subfolder
    ```

14. 在本地测试站点。

    ```bash
    bundle exec jekyll serve
    ```

    在 web 浏览器中导航到 `http://localhost:4000`。

15. 添加并提交工作。

    ```shell
    git add .
    git commit -m 'Initial GitHub pages site with Jekyll'
    ```

16. 将您的 GitHub 仓库添加为远程仓库，将 *USER* 替换为拥有该仓库的帐户并将 *REPOSITORY* 替换为仓库名称。

	```shell
	$ git remote add origin 			https://github.com/USER/REPOSITORY.git
	# 当然，我这里用的是ssh地址
	```



## 更新 GitHub Pages gem

Jekyll 是一个活跃的开源项目，经常更新。 如果您计算机上的 `github-pages` gem 版本落后于 GitHub Pages 服务器上的 `github-pages` gem 版本，则您的站点在本地构建时的外观与在 GitHub 上发布时的外观可能不同。 为避免这种情况，请定期更新计算机上的 `github-pages` gem。

1. 打开 Git Bash。

2. 更新`github-pages`gem
   - 如果您安装了 Bundler，请运行 `bundle update github-pages`。
   - 如果未安装 Bundler，则运行 `gem update github-pages`.
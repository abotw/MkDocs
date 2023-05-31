# MkDocs

## MkDocs 是什么？

* Project documentation with Markdown.
* MkDocs is a **fast**, **simple** and **downright gorgeous** static site generator.

## 1. 如何安装？

```shell
pip3 install mkdocs
```

## 2. 如何卸载？

```shell
pip3 uninstall mkdocs
```

## 3. 如何使用？

### 3.0 Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h`, `mkdocs --help` - Print help message and exit.
* `mkdocs $COMMAND -h` - To view a list of options available on a given command

### 3.1 Create a new project

```shell
$ mkdocs new my-project # 创建一个名为 my-project 的文件夹，存储 mkdocs 文件
$ cd my-project
```

#### 3.1.1 Project layout

```
.
├── docs					# contain your documentation source files
│   ├── index.md	# The documentation homepage.
│   └── ...       # Other markdown pages, images and other files.
└── mkdocs.yml		# The configuration file.
```

⚠️ 注意：

* `docs` is the default value for the [docs_dir](https://www.mkdocs.org/user-guide/configuration/#docs_dir) configuration setting

### 3.2 Start the live-reloading docs server 实时渲染服务器

Start the server by running the `mkdocs serve` command.

```shell
$ mkdocs serve
INFO     -  Building documentation...
INFO     -  Cleaning site directory
INFO     -  Documentation built in 0.04 seconds
INFO     -  [19:11:17] Watching paths for changes: 'docs',
            'mkdocs.yml'
INFO     -  [19:11:17] Serving on http://127.0.0.1:8000/
# 默认端口：8000
```

### 3.3 Adding pages

### 3.4 Theming our documentation

```yaml
theme:
  name: material
```

```shell
$ pip3 install mkdocs-material
```



### 3.5 Changing the Favicon Icon

To use a different icon, create an `img` subdirectory in the `docs` directory and copy your custom `favicon.ico` file to that directory.

## 4. 配置文件

```yaml
site_name: MkLorum # 必填
site_url: https://example.com/
nav:
    - Home: index.md
    - About: about.md
theme: readthedocs
```

* [repo_url](https://www.mkdocs.org/user-guide/configuration/#repo_url)
* [edit_uri](https://www.mkdocs.org/user-guide/configuration/#edit_uri)
* [site_description](https://www.mkdocs.org/user-guide/configuration/#site_description)
* [copyright](https://www.mkdocs.org/user-guide/configuration/#copyright)
* [theme](https://www.mkdocs.org/user-guide/configuration/#theme)
* [extra_css](https://www.mkdocs.org/user-guide/configuration/#extra_css)
* [extra_javascript](https://www.mkdocs.org/user-guide/configuration/#extra_javascript)
* [markdown_extensions](https://www.mkdocs.org/user-guide/configuration/#markdown_extensions)

### 4.1 Linking to images and media

* [Linking to images and media](https://www.mkdocs.org/user-guide/writing-your-docs/#linking-to-images-and-media)

```shell
$ mkdir docs/img
docs/
    img/
        screenshot.png
```

```markdown
![Screenshot](img/screenshot.png)
```

## 5. Build the site

```shell
$ mkdcos build # 生成静态网页代码
INFO     -  Cleaning site directory
INFO     -  Building documentation to directory: /path/to/my-project/site # 生成 site/ 文件夹
INFO     -  Documentation built in 0.07 seconds
$ tree site -L 2
site
├── 404.html
├── about
│   └── index.html
├── css
│   ├── fonts
│   ├── theme.css
│   └── theme_extra.css
├── img
│   └── favicon.ico
├── index.html
├── js
│   ├── html5shiv.min.js
│   ├── jquery-3.6.0.min.js
│   ├── theme.js
│   └── theme_extra.js
├── search
│   ├── lunr.js
│   ├── main.js
│   ├── search_index.json
│   └── worker.js
├── search.html
├── sitemap.xml
└── sitemap.xml.gz

6 directories, 5 files
```

### 5.1 how to ignore documentation builds

```shell
echo "site/" >> .gitignore # Add a line containing site/ to your .gitignore file.
```

## References

* [References - MkDocs](https://squidfunk.github.io/mkdocs-material/reference/)

* [mkdocs 使用记录 - 鹤翔万里的笔记本](https://note.tonycrane.cc/cs/tools/mkdocs/)

* [Mkdocs Material使用记录 - shafish.cn](https://shafish.cn/blog/mkdocs/)

* [Getting Started with MkDocs](https://www.mkdocs.org/getting-started/)

* [Deploying your docs - MkDocs](https://www.mkdocs.org/user-guide/deploying-your-docs/)

* [MkDocs User Guide](https://www.mkdocs.org/user-guide/) - More complete documentation of all of MkDocs' features.
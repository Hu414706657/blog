---
title: Hexo搭建个人博客
date: 2021-04-22 10:17:47
tags:
---

### hexo官网地址：

* https://hexo.io/zh-cn/
---

### hexo文档地址：

* https://hexo.io/zh-cn/docs/
------

### 快速上手

- hexo目录结构：
    ```text
    .
    ├── _config.yml    # 网站的配置信息
    ├── package.json   # 应用程序的信息
    ├── public         # 公共文件夹，用于存放生成的站点文件
    ├── scaffolds      # 模版文件夹
    ├── source         # 资源文件夹，用来存放内容
    └── themes         # 主题文件夹

    ```

---

## 常用命令

* 后台运行命令：nohup hexo server -p 4000 &
* 停止服务：killall hexo
* 查看版本：hexo version 或者 hexo v
* 启动服务：hexo server 或者 hexo s
* 新建一篇文章，比如：hexo new "Hello Hexo"
* 清除缓存文件 (db.json) 和已生成的静态文件 (public)：hexo clean
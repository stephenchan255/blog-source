---
title: Blog config
date: 2020-09-04 12:14:13
---

# Install Hexo
```shell
  $ npm install hexo-cli -g
  $ hexo init blog      # init a hexo project called "blog" in current folder
  $ cd blog
  $ npm install         # "package-lock.json" created
  $ hexo s              # "db.json" created & project preview
```
<!-- more -->

# Install Next & Choosing-Scheme
- `$ npm install hexo-theme-next`
- `## my config (for NexT scheme)`

Hexo > Themes > "NexT" > [Doc](https://theme-next.js.org/) > [NexT-Installation](https://theme-next.js.org/docs/getting-started/#NexT-Installation) (download by npm) > [Choosing-Scheme](https://theme-next.js.org/docs/theme-settings/#Choosing-Scheme) (select "Gemini")

# Config
- Config file management: [Hexo | Alternate Theme Config](https://hexo.io/docs/configuration.html#Alternate-Theme-Config)
- Sidebar
	- `_config.yml` > `## my config (for sidebar)` [..](https://theme-next.js.org/docs/theme-settings/sidebar.html)
	- «Tags» / «Categories» Page & custom pages: [NexT | Costom Pages](https://theme-next.js.org/docs/theme-settings/custom-pages.html)
- Post 
	- custom style: `_config.yml` > `## my config (for custom style)`
	- [Categories & Tags](https://hexo.io/docs/front-matter#Categories-amp-Tags), 
- Misc Theme Settings: `_config.yml` > `## my config (for misc Theme Settings)`
- Third-party plugin: [Local Search](https://theme-next.js.org/docs/third-party-services/search-services.html#Local-Search), [Tag Plugins](https://theme-next.js.org/docs/tag-plugins/)

# Write new posts/pages
```shell
	$ hexo new [page] <title>
	$ hexo clean && hexo g
```

# Deployment
- github pages of this website: `_config.yml`: `## my config (for deployment)`
- github repo of the source file: [blog-source](https://github.com/stephenchan255/blog-source)

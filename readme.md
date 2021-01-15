# Hexo

## Deploy

[hexo clean && hexo deploy](https://hexo.io/docs/deployment)
[new blog](https://hexo.io/docs/writing)

```bash
$hexo new [layout] <title>
$hexo publish [layout] <title> // publish Draft
$hexo new photo "My Gallery"
```

## Travis status

![travis](https://travis-ci.org/CatzillaOrz/catzilla_githubio_repo.svg?branch=master)

## CDN / img_cdn

- wxpay
![wxpay2020-05-03](https://cdn.jsdelivr.net/gh/catzillaorz/imgcdn/vsc_img/wxpay2020-05-03.jpeg)

- alipay
![alipay2020-05-03](https://cdn.jsdelivr.net/gh/catzillaorz/imgcdn/vsc_img/alipay2020-05-03.jpeg)

- avatar
![avatar2020-05-03](https://cdn.jsdelivr.net/gh/catzillaorz/imgcdn/vsc_img/avatar2020-05-03.JPG)

## Bash

```bash

find ./ -type f -name '*01-29-*.md' -exec sed -i'' -e  "s/raw.githubusercontent.com\/CatzillaOrz\/imgcdn\/master\//cdn.jsdelivr.net\/gh\/catzillaorz\/imgcdn\//g" {} \;

rm -rf *.md-e
```

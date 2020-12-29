# hexo

## deploy

[hexo clean && hexo deploy](https://hexo.io/docs/deployment)
[new blog](https://hexo.io/docs/writing)

```bash
$hexo new [layout] <title>
$hexo publish [layout] <title> // publish Draft
$hexo new photo "My Gallery"
```

## travis status

![travis](https://travis-ci.org/CatzillaOrz/catzilla_githubio_repo.svg?branch=master)

## img_cdn

- wxpay
![wxpay2020-05-03](https://cdn.jsdelivr.net/gh/catzillaorz/imgcdn/vsc_img/wxpay2020-05-03.jpeg)

- alipay
![alipay2020-05-03](https://cdn.jsdelivr.net/gh/catzillaorz/imgcdn/vsc_img/alipay2020-05-03.jpeg)

- avatar
![avatar2020-05-03](https://cdn.jsdelivr.net/gh/catzillaorz/imgcdn/vsc_img/avatar2020-05-03.JPG)

```bash
find ./ -type f -name '*12-29-*.md' -exec sed -i'' -e  "s/raw.githubusercontent.com\/CatzillaOrz\/imgcdn\/master\//cdn.jsdelivr.net\/gh\/catzillaorz\/imgcdn\//g" {} \;
```

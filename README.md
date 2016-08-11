# YASCMF/ASSET

>   本仓库为 `YASCMF` 公共静态资源库。为了加快 `YASCMF` 运行速度，特意剥离出公共类库，你可以将此仓库部署到独立域名。

## 源码说明

文件操作类源码来自网络：http://www.cnblogs.com/eczhou/archive/2013/01/08/2851018.html 。更多细节请阅读 `pulish.php` 源码。

如有侵权请告知。

## 发布资源

原始静态资源位于 `resource` 目录下，你可以在此调整原始资源。

下载源码部署到服务器之后，绑定 `public` 为域名根目录，然后在终端中执行：

```bash
git clone https://github.com/yascmf/asset.git asset
cd asset
php publish.php
```

成功一般会回显：`all assets have been published!` 。

如果你想发布到新的临时目录，可携带 `public` 的参数，这样资源会被发布到 `_public` 下。

```bash
php publish.php _public
```

为了安全起见，静态资源站，请勿允许任何脚本语言执行权限。这里给出一份 `nginx` 虚拟主机配置文件：

```nginx
server {
listen 80;
server_name example.org;
access_log /home/wwwlogs/example.org_nginx.log combined;
index index.html index.htm;
include none.conf;
root /home/wwwroot/example.org/public;
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|ico)$ {
    expires 30d;
    }
location ~ .*\.(js|css)?$ {
    expires 7d;
    }
location ~* \.(eot|otf|ttf|woff|svg|woff2)$ {
    add_header Access-Control-Allow-Origin *;
    }
}
```

最后一条规则是针对网页字体（诸如 `font-awesome` ）跨域调用的，一般加上才会正常显示网页字体，否则浏览器端 `console` 会报错。

## CDN云存储

可使用七牛或又拍云，将静态资源发布到 `CDN` ，以加快静态资源访问速度，具体请参考七牛或又拍云官方开发文档。

## 相关配置

如果你没有部署此静态库，可以使用 `YASCMF` 官方线上静态资源库：
访问域名为： http://s2.ystatic.cn 或 https://s2.ystatic.cn 。

如果你已经部署此静态资源库到新的域名，请修改 `YASCMF` config 目录下 `site` 相关配置：

```php
        #静态资源CDN配置
        'cdn' => [
            //'on' or 'off'
            'status'  => 'on',
            'url'     => '//s2.ystatic.cn/',

            #匹配所有资源路径:
            //'pattern' => '/.*/i',
            #仅匹配 `lib/` 打头的资源路径:
            'pattern' => '/^lib\/.*/i',
        ],
```

由于启用了 `preg_match` 来对资源路径进行模式匹配，如果你想所有资源都从 `CDN` 域获取，可以将 `pattern` 值留空，或直接删掉该键值对，以跳过正则匹配加快字符串拼接速度。

你可以在 http://www.phpliveregex.com/ 网站在线完成正则匹配测试。

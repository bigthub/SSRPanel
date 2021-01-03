# SSRPanel Standard
Support but not limited to: Shadowsocks,ShadowsocksR,ShadowsocksRR,V2Ray

- [Demo](https://demo.ssrpanel.com)
- [Telegram](https://t.me/ssrpanel)
- [Issues](https://github.com/ssrpanel/SSRPanel/issues)
- [WIKI](https://github.com/ssrpanel/SSRPanel/wiki)

## Donate
**ETH** : 0x968f797f194fcec05ea571723199748b58de38ba


[VPS Recommend](https://github.com/ssrpanel/SSRPanel/wiki/VPS%E6%8E%A8%E8%8D%90&%E8%B4%AD%E4%B9%B0%E7%BB%8F%E9%AA%8C)

## Requirements
- PHP 7.1.3 +
- Mysql 5.5 +
- Memory 1G +
- Disk 10G +
- PHP Extensions: ZIP、XML、CURL、GD2、FileInfo、OpenSSL、Mbstring、PDO、Tokenizer、Ctype、JSON、BCMath
- Laravel 5.6

## Thanks
- [@shadowsocks](https://github.com/shadowsocks)
- [@V2Ray](https://www.v2ray.com)
- [@breakwa11](https://github.com/breakwa11)
- [@glzjin](https://github.com/esdeathlove)
- [@orvice](https://github.com/orvice)
- [@Akkariiin](https://github.com/shadowsocksrr)
- [@aiyahacke](https://github.com/aiyahacke)
- [@ColetteContreras](https://github.com/ColetteContreras)
- [@rc452860](https://github.com/rc452860)
- [@pch18](https://github.com/pch18)
- [@tonychanczm](https://github.com/tonychanczm)
- [@91yun](https://github.com/91yun)
- [@ToyoDAdoubi](https://github.com/ToyoDAdoubi)
- [@ipcheck](https://ipcheck.need.sh)
- [@cz88](http://www.cz88.net/index.shtml)
- [@ip.sb](https://www.ip.sb)
- [@ipip](https://www.ipip.net)


说明
SSRPanel 最后的开源版本。

安装
环境要求
PHP 7.1.3+ （必须）
MYSQL 5.5+ （推荐5.6）
RAM 1G+ (推荐2G)
DISK 10G+
Redis
PHP必须开启zip、xml、curl、gd2、fileinfo、openssl、mbstring、sg11解密等组件

安装完成后记得编辑.env中 APP_DEBUG 改为 false
编辑php.ini
找到php.ini
nano /usr/local/php/etc/php.ini

搜索disable_function
删除所有proc_开头的函数
NGINX 加入URL重写规则
location / {
    try_files $uri $uri/ /index.php$is_args$args;
}
定时任务
crontab加入如下命令
* * * * * php /www/wwwroot/ssrpanel/artisan schedule:run >> /dev/null 2>&1

注意运行权限，必须跟ssrpanel项目权限一致，否则出现各种莫名其妙的错误
例如用lnmp的话默认权限用户组是 www:www，则添加定时任务是这样的：
crontab -e -uwww
数据库
创建一个字符集为utf8mb4排序规则为utf8mb4_unicode_ci的数据库，然后直接访问网址按提示操作即可，或者自行导入sql文件夹下的db.sql

邮件配置
使用SMTP发信
编辑 .env 文件，修改 MAIL_ 开头的配置
使用队列处理邮件
邮件走队列处理，两种方式：

1.面板根目录下手动执行一次 sh queue.sh，用于观测是否成功发送邮件，可以通过kill手动杀死进程

2.守护发邮件进程

nano /etc/systemd/system/ssrpanel-email.service

复制粘贴以下内容

[Unit]
Description=sspanelj email queue Daemon Service

[Service]
User=root
Type=simple
# 根据自己的实际路径修改下一行
ExecStart=/home/wwwroot/ssrpanel/queue.sh

[Install]
WantedBy=multi-user.target
设置开机启动

systemctl daemon-reload
systemctl enable ssrpanel-email.service
systemctl start ssrpanel-email.service
发邮件失败处理
出现 Connection could not be established with host smtp.exmail.qq.com [Connection timed out #110] 这样的错误
因为smtp发邮件必须用到25,26,465,587这四个端口，故需要允许这四个端口通信
多语言版本
修改 .env 的 APP_LOCALE 值为 en
语言包位于 resources/lang 下，可自行更改，目前支持繁、简、日、韩
HTTPS
将 .env 文件里的 REDIRECT_HTTPS 值改为true，则全站强制走https
境内部署
部署于大陆服务器上时，请在composer.json目录下得config后面加入如下配置，用于组件下载加速

"repositories": {
    "packagist": {
        "type": "composer",
        "url": "https://mirrors.aliyun.com/composer/"
    }
},
后端
后端

自用修改版后端

自用修改版后端开机自启
nano /etc/systemd/system/vnet.service

复制粘贴以下内容

[Unit]
Description=vnet Daemon Service
After=network.target

[Service]
User=root
Type=simple
# 根据自己的实际路径修改下一行
ExecStart=/root/vnet -cfg /root/vnetconfig.json >vnet.log 2>&1

Restart=always

[Install]
WantedBy=multi-user.target
设置开机启动

systemctl daemon-reload

systemctl enable vnet.service

systemctl start vnet.service
## License

SSRPanel Standard is open-sourced software licensed under the MIT license.

title: certbot-auto申请ssl证书并自动续期
date: 2020-07-24 23:03:00
categories: 技术
tags: [ssl]
---
#### 1. 安装certbot
```javascript
wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
```
<!--more-->
#### 2. 申请证书
> 申请证书需要先把域名解析到服务器上

* 申请普通证书(以abc.com为例)
```javascript
sudo ./certbot-auto certonly --standalone --email my@qq.com -d abc.com -d www.abc.com
```
执行上面命令，按提示操作，域名可以填写多个，使用**-d**分隔
Certbot 会启动一个临时服务器来完成验证（会占用80端口或443端口，因此需要暂时关闭 Web 服务器），然后 Certbot 会把证书以文件的形式保存在 **/etc/letsencrypt/live/** 下面的域名目录下，包括完整的证书链文件和私钥文件。


* 申请泛解析证书(以abc.com为例)
>申请泛解析证书不需要关闭web服务

```javascript
sudo ./certbot-auto -d abc.com -d "*.abc.com" --manual --preferred-challenges dns certonly
```
这条命令申请了 abc.com 及 *.abc.com 这两个域名证书，并且手动指定了认证方式为 dns认证

接下来会要求你同意一些协议，输入一个邮箱（用来提醒你证书过期），然后会要求你添加一条 TXT 记录：

>Please deploy a DNS TXT record under the name _acme-challenge.abc.com with the following value: mrFoW9h-LcUIoxxxxxxxxxxxxxxxxxxxxxxx1M

>Before continuing, verify the record is deployed.

添加域名解析后，耐心等待几分钟，先在其它终端执行
```javascript
dig _acme-challenge.abc.com TXT
```

确保DNS记录已经生效，再按 Enter 继续
> 如果没有dig命令，需要安装**dnsutil**
```javascript
apt install dnsutils
```

之后会要求你再添加一条 TXT 记录
>这里注意，是添加一条新的名字相同、值不相同的 TXT 记录。不能把之前的那条记录删掉，也不能在原来的记录上修改。

添加完成后，依然先执行 dig 查询一下，如果没有问题，此时应该有两条记录如下

```javascript
dig _acme-challenge.abc.com TXT
; <<>> DiG 9.10.3-P4-Debian <<>> _acme-challenge.abc.com TXT
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 26466
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;_acme-challenge.abc.com.	IN	TXT

;; ANSWER SECTION:
_acme-challenge.abc.com. 7207	IN	TXT	"7aqiZi8sPemhXU81hGRgd69nMucE0GpdM66n6N8YQUY"
_acme-challenge.abc.com. 7207	IN	TXT	"EmFxAcgMuEw1NEEeaTHzfJF1TJIoiRDHwVPYWzJ3U2w"

;; Query time: 159 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Sat Jul 25 12:52:02 CST 2020
;; MSG SIZE  rcvd: 166

```

确定没问题后再次按 Enter 提交，经过验证后,证书就签发完成了,终端会输出证书存放的路径、过期时间等信息。
一般也是在/etc/letsencrypt/live的域名目录里

#### 3. 创建定时任务，自动续期
默认证书有效期是3个月，所以需要续期
**以下自动续期只适用于普通证书**

创建定时任务
```javascript
sudo crontab -e
```
我的certbot-auto的所在目录为/home；

在最后添加
```javascript
0 3 1 * * /home/certbot-auto renew --renew-hook "sudo nginx -s reload"
```
然后
```javascript
sudo crontab -l
```
查看一下是否存在刚才添加的定时命令。如果存在的话，那么每月1日的凌晨3点就会执行一次所有域名的续期操作

**泛域名证书续期不能使用以上命令，建议使用 https://sudo.plus/archives/159.html 的方法自动续期**
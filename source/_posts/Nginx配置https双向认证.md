title: Nginx配置https双向认证
date: 2020-11-02 08:13:00
categories: 技术
tags: [nginx,ssl]
---
### 生成 CA 私钥和证书
1. 生成一个 CA 私钥: 
```python
 openssl genrsa -out ca.key 4096
```
2. 生成一个 CA 的数字证书
    ```c
    openssl req -new -x509 -days 3650 -key ca.key -out ca.crt
    ```

### CA签发服务端证书
1. 生成服务端的私钥: server.key
    ```c
    openssl genrsa -out server.key 4096
    ```
2. 生成服务端数字证书请求: server.csr
    ```c
    openssl req -new -key server.key -out server.csr
    ```
    如果使用ip地址访问的，所以Common Name，输入ip即可。
    如果使用域名访问，那么这一步，必须是域名
3. 用 CA 私钥签发服务端的数字证书
    ```c
    openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 3650
    ```

### CA签发客户端证书
1. 生成客户端的私钥
    ```c
    openssl genrsa -out client.key 4096
    ```
2. 生成客户端数字证书请求
    ```c
    openssl req -new -key client.key -out client.csr
    ```
3. 用 CA 私钥签发客户端的数字证书
    ```c
    openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days 3650
    ```
	
### 配置nginx
    ```c
        server {
                listen 443;
                server_name localhost;
                ssl on;
                ssl_certificate server.crt;
                ssl_certificate_key server.key;
                ssl_client_certificate ca.crt;#双向认证
                ssl_verify_client on; #双向认证
                ssl_session_timeout 5m;
                ssl_protocols SSLv2 SSLv3 TLSv1 TLSv1.1 TLSv1.2; 
                ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
                ssl_prefer_server_ciphers on;
                root html;
                index index.html;
                location / {
                        try_files $uri $uri/ =404;
                }
        }
    ```
	
### 重载nginx

    ```c
        nginx -s reload
    ```
	
### 导出PFX格式证书
    ```c
    openssl pkcs12 -export -inkey client.key -in client.crt -out client.pfx
    ```

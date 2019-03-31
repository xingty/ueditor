# About

ueditor上传文件时默认是通过fromdata传到自己的服务器上，我稍微改了一下它的源码重新打包了一份。这个版本的ueditor默认上传到阿里云oss上。

# How To Use
把这份代码部署到你的nginx服务器上，目录结构为/ueditor,请注意不要放根目录，一定要放在ueditor这一级目录，否则无法正常工作。

```nginx
server {
    listen 80;
    root /var/www/ueditor;
    index index.html index.php;

    server_name 127.0.0.1;

    add_header 'Access-Control-Allow-Origin' "$http_origin";
    add_header 'Access-Control-Allow-Credentials' 'true';
    add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Token';
    add_header 'Access-Control-Allow-Methods' 'GET, POST,DELETE, OPTIONS';

    location / { 
			if ($request_method = 'OPTIONS') {
				add_header Access-Control-Allow-Origin $http_origin;
				add_header Access-Control-Allow-Methods GET,POST,PUT,DELETE,OPTIONS;
				add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Token';
				add_header 'Access-Control-Allow-Credentials' 'true';
				return 204;
			}

			try_files $uri $uri/ /index.html;
		}
}
```

```javascript
<script type="text/javascript">
	/**
	 * 使用oss上传可以通过这种方式修改oss获取签名的地址及指定默认上传路径
	 * 延迟3秒设置保证ueditor初始化完成
	 * */
	setTimeout(() => {
		window.UEDITOR_CONFIG.oss = {
			url: "http://localhost:8227/app/oss-info",//oss 签名路径
			path: "cms" //oss上传路径
		}
		/** 服务器统一请求接口路径,如果和api同源不需修改,否则需要改成api的路径 */
		window.UEDITOR_CONFIG.serverUrl = 'http://domain/ueditor/config'
	},3000);
</script>
```
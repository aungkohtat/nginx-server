
<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

- [nginx-server](#nginx-server)
   * [nginx installation](#nginx-installation)
   * [check directory ](#check-directory)
   * [nginx run localhost](#nginx-run-localhost)
   * [check nginx.config](#check-nginxconfig)
   * [nginx static context](#nginx-static-context)
      + [my new config](#my-new-config)
   * [mimes type](#mimes-type)
   * [location block](#location-block)
   * [redirects and rewrites](#redirects-and-rewrites)
   * [load balancer](#load-balancer)
   * [install server setup](#install-server-setup)

<!-- TOC end -->

<!-- TOC --><a name="nginx-server"></a>
# nginx-server
NGINX is open-source web server software used for reverse proxy, load balancing, and caching.
- NGINX is the fastest growing and most popular web server for a reason, so understanding it will be an invaluable skill to anyone working in the web industry, especially network admins & web developers.
NGINX is one of the top-grade HTTP Servers which powers most of the top Enterprise websites like Netflix, Dropbox, WordPress, GitHub, Discovery, and many others.
<!-- TOC --><a name="nginx-installation"></a>
## nginx installation

```bash
Aungs-MacBook-Pro:~ aungkohtet$ brew install nginx
Running `brew update --auto-update`...
^C
Aungs-MacBook-Pro:~ aungkohtet$ clear
Aungs-MacBook-Pro:~ aungkohtet$ brew install nginx
Running `brew update --auto-update`...
==> Auto-updated Homebrew!
Updated 2 taps (homebrew/core and homebrew/cask).
==> New Formulae
csvlens         k8sgpt          rathole         steamguard-cli  zipkin
doltgres        kiota           rattler-build   urlscan
helm-ls         ncmdump         ruby@3.2        vulkan-volk
==> New Casks
aqua                ia-presenter        markedit            taccy
emby                jyutping            prettyclean         wakatime

You have 7 outdated formulae installed.

==> Downloading https://ghcr.io/v2/homebrew/core/nginx/manifests/1.25.3
######################################################################### 100.0%
==> Fetching nginx
==> Downloading https://ghcr.io/v2/homebrew/core/nginx/blobs/sha256:22ebed3da9b8
######################################################################### 100.0%
==> Pouring nginx--1.25.3.sonoma.bottle.tar.gz
==> Caveats
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To start nginx now and restart at login:
  brew services start nginx
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/nginx/bin/nginx -g daemon\ off\;
==> Summary
🍺  /usr/local/Cellar/nginx/1.25.3: 26 files, 2.4MB
==> Running `brew cleanup nginx`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
Aungs-MacBook-Pro:~ aungkohtet$

```

<!-- TOC --><a name="check-directory"></a>
## check directory 
- **`cd /usr/local/etc/nginx`**

```bash
ungs-MacBook-Pro:~ aungkohtet$ cd /usr/local/etc/nginx
Aungs-MacBook-Pro:nginx aungkohtet$ ls
fastcgi.conf		mime.types		servers
fastcgi.conf.default	mime.types.default	uwsgi_params
fastcgi_params		nginx.conf		uwsgi_params.default
fastcgi_params.default	nginx.conf.default	win-utf
koi-utf			scgi_params
koi-win			scgi_params.default
```

<!-- TOC --><a name="nginx-run-localhost"></a>
## nginx run localhost
- `http://localhost:8080`


<!-- TOC --><a name="check-nginxconfig"></a>
## check nginx.config

```bash
cat nginx.conf

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       8080;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
    include servers/*;
}
```

<!-- TOC --><a name="nginx-static-context"></a>
## nginx static context

<!-- TOC --><a name="my-new-config"></a>
### my new config

```bash
http {
    server {
        listen 8080;
        root /Users/aungkohtet/Documents/akhlab/nginx-server/mysite;
    }
}
events {}
```

- `nginx -s reload`

```bash
Aungs-MacBook-Pro:nginx aungkohtet$ nginx -s reload
Aungs-MacBook-Pro:nginx aungkohtet$ Ok
```

<!-- TOC --><a name="mimes-type"></a>
## mimes type
- MIME types indicate the nature and format of a document. In NGINX, mime.types assigns these types to file extensions. For example:
```bash
http {
    include       mime.types;
    default_type  application/octet-stream;

    types {
        text/html                     html htm shtml;
        text/css                      css;
        application/javascript        js;
        image/jpeg                    jpeg jpg;
        image/png                     png;
        audio/mpeg                    mp3;
        // ... more mappings
    }
}

```


<!-- TOC --><a name="location-block"></a>
## location block
- Location blocks define configurations for specific URIs. For instance:

```bash
server {
    location / {
        root   /path/to/your/files;
        index  index.html index.htm;
    }

    location /images/ {
        alias /path/to/image/files/;
    }
}

```

<!-- TOC --><a name="redirects-and-rewrites"></a>
## redirects and rewrites
- Redirects send an HTTP status code to clients, indicating a resource's move. Rewrites modify URIs internally. Both are crucial for URL manipulation in NGINX.

- Redirect Example:

```bash
server {
    listen 80;
    server_name old-domain.com;
    return 301 http://new-domain.com$request_uri;
}

```

- Rewrite Example:

```bash
server {
    listen 80;
    server_name example.com;

    location /blog/ {
        rewrite ^/blog/(.*)$ /posts/$1 last;
    }
}

```

<!-- TOC --><a name="load-balancer"></a>
## load balancer

- NGINX load balancing distributes traffic across multiple servers, enhancing reliability and scalability. The upstream block defines servers, and NGINX balances requests among them.

<!-- TOC --><a name="install-server-setup"></a>
## install server setup
- `mkdir server`

- `npm init -y`

```bash
Aungs-MacBook-Pro:server aungkohtet$ npm init -y
Wrote to /Users/aungkohtet/Documents/akhlab/nginx-server/mysite/server/package.json:

{
  "name": "server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}


Aungs-MacBook-Pro:server aungkohtet$ 
```

- `npm install express`
```bash

 **package.json add line no.8 "start": "node index"**

 - `npm run start`
 ```bash
 Aungs-MacBook-Pro:server aungkohtet$ npm run start

> server@1.0.0 start
> node index

Listening no port 7777
 ```

 **run browser http://localhost:7777**

 ## dockerfile for express app

 - create dockerfile

 ```bash
 # Use an official Node.js runtime as a parent image
FROM node:16

<!-- TOC --><a name="set-the-working-directory-in-the-container"></a>
# Set the working directory in the container
WORKDIR /usr/src/app

<!-- TOC --><a name="copy-packagejson-and-package-lockjson-to-the-working-directory"></a>
# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

<!-- TOC --><a name="install-app-dependencies"></a>
# Install app dependencies
RUN npm install

<!-- TOC --><a name="bundle-app-source"></a>
# Bundle app source
COPY . .

<!-- TOC --><a name="expose-port-8080"></a>
# Expose port 8080
EXPOSE 8080

<!-- TOC --><a name="define-the-command-to-run-the-app"></a>
# Define the command to run the app
CMD ["npm", "run", "start" ]

 ```

 ```bash
 Aungs-MacBook-Pro:server aungkohtet$ docker build . -t myserver
[+] Building 185.6s (11/11) FINISHED                                                    docker:desktop-linux
 => [internal] load .dockerignore                                                                       0.0s
 => => transferring context: 2B                                                                         0.0s
 => [internal] load build definition from Dockerfile                                                    0.0s
 => => transferring dockerfile: 493B                                                                    0.0s
 => [internal] load metadata for docker.io/library/node:16                                              3.9s
 => [auth] library/node:pull token for registry-1.docker.io                                             0.0s
 => [1/5] FROM docker.io/library/node:16@sha256:f77a1aef2da8d83e45ec990f45df50f1a286c5fe8bbfb8c6e424  179.2s
 => => resolve docker.io/library/node:16@sha256:f77a1aef2da8d83e45ec990f45df50f1a286c5fe8bbfb8c6e4246c  0.0s
 => => sha256:f77a1aef2da8d83e45ec990f45df50f1a286c5fe8bbfb8c6e4246c6389705c0b 776B / 776B              0.0s
 => => sha256:ffd9397e94b74abcb54e514f1430e00f604328d1f895eadbd482f08cc02444e5 51.89MB / 51.89MB      160.3s
 => => sha256:c94b82f9827cab6e421b350965a9ef11b25b13ffbd1030536203d541f55dcbe2 2.00kB / 2.00kB          0.0s
 => => sha256:1ddc7e4055fdb6f6bf31063b593befda814294f9f904b6ddfc21ab1513bafa8e 7.23kB / 7.23kB          0.0s
 => => sha256:311da6c465ea1576925360eba391bcd32dece9be95960a0bc9ffcb25fe712017 50.50MB / 50.50MB       73.7s
 => => sha256:7e9bf114588c05b2df612b083b96582f3b8dbf51647aa6138a50d09d42df2454 17.58MB / 17.58MB       13.1s
 => => sha256:513d779256048c961239af5f500589330546b072775217272e19ffae1635e98e 191.90MB / 191.90MB    155.1s
 => => extracting sha256:311da6c465ea1576925360eba391bcd32dece9be95960a0bc9ffcb25fe712017               3.9s
 => => sha256:ae3b95bbaa61ce24cefdd89e7c74d6fbd7713b2bcae93af47063d06bd7e02172 4.20kB / 4.20kB         74.7s
 => => sha256:0e421f66aff42bb069dffc26af6d132194b22a1082b08c5ef7cd69c627783c04 34.79MB / 34.79MB      125.3s
 => => extracting sha256:7e9bf114588c05b2df612b083b96582f3b8dbf51647aa6138a50d09d42df2454               0.8s
 => => sha256:ca266fd6192108b67fb57b74753a8c4ca5d8bd458baae3d4df7ce9f42dedcc1d 2.27MB / 2.27MB        136.0s
 => => sha256:ee7d78be1eb92caf6ae84fc3af736b23eca018d5dedc967ae5bdee6d7082403b 450B / 450B            138.6s
 => => extracting sha256:ffd9397e94b74abcb54e514f1430e00f604328d1f895eadbd482f08cc02444e5               4.0s
 => => extracting sha256:513d779256048c961239af5f500589330546b072775217272e19ffae1635e98e              11.8s
 => => extracting sha256:ae3b95bbaa61ce24cefdd89e7c74d6fbd7713b2bcae93af47063d06bd7e02172               0.0s
 => => extracting sha256:0e421f66aff42bb069dffc26af6d132194b22a1082b08c5ef7cd69c627783c04               2.4s
 => => extracting sha256:ca266fd6192108b67fb57b74753a8c4ca5d8bd458baae3d4df7ce9f42dedcc1d               0.1s
 => => extracting sha256:ee7d78be1eb92caf6ae84fc3af736b23eca018d5dedc967ae5bdee6d7082403b               0.0s
 => [internal] load build context                                                                       0.1s
 => => transferring context: 2.16MB                                                                     0.1s
 => [2/5] WORKDIR /usr/src/app                                                                          0.6s
 => [3/5] COPY package*.json ./                                                                         0.0s
 => [4/5] RUN npm install                                                                               1.7s
 => [5/5] COPY . .                                                                                      0.1s
 => exporting to image                                                                                  0.1s
 => => exporting layers                                                                                 0.1s
 => => writing image sha256:f8be430929e6a20e619e860a3b608ea9efe26b17fc4189177c6c9362fe1a246c            0.0s
 => => naming to docker.io/library/myserver                                                             0.0s

What's Next?
  View a summary of image vulnerabilities and recommendations → docker scout quickview
 ```

 - `docker run -p 1111:7777 -d myserver`

 ```bash
 Aungs-MacBook-Pro:server aungkohtet$ docker run -p 1111:7777 -d myserver
85e581302371c38ace3b87d2b807d647cfff05e5c7f072dad2d7502256cb9220
Aungs-MacBook-Pro:server aungkohtet$ docker run -p 2222:7777 -d myserver
1403106b63f8a49e52feabddb6fd2702ee4624315e3212c2f9d49bc2448a57bd
Aungs-MacBook-Pro:server aungkohtet$ docker run -p 3333:7777 -d myserver
4c046f176b432da408dd421afbc678f08e6faba139b7adbb99be0e75cabfa507
Aungs-MacBook-Pro:server aungkohtet$ 
 ```

 **run browser http://localhost:1111, http://localhost:2222, http://localhost:3333, http://localhost:4444**

 ```bash
 Aungs-MacBook-Pro:server aungkohtet$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                              NAMES
8b875e7c4897   myserver   "docker-entrypoint.s…"   13 minutes ago   Up 13 minutes   8080/tcp, 0.0.0.0:4444->7777/tcp   bold_bardeen
4c046f176b43   myserver   "docker-entrypoint.s…"   14 minutes ago   Up 14 minutes   8080/tcp, 0.0.0.0:3333->7777/tcp   infallible_brahmagupta
1403106b63f8   myserver   "docker-entrypoint.s…"   15 minutes ago   Up 15 minutes   8080/tcp, 0.0.0.0:2222->7777/tcp   admiring_heyrovsky
85e581302371   myserver   "docker-entrypoint.s…"   15 minutes ago   Up 15 minutes   8080/tcp, 0.0.0.0:1111->7777/tcp   funny_solomon
Aungs-MacBook-Pro:server aungkohtet$ 
 ```





  


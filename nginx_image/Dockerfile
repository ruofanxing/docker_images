FROM centos:7

LABEL maintainer="rylai"

ENV NGINX_VERSION 1.17.9

ADD lua-nginx-module-0.10.15.tar.gz /
ADD LuaJIT-2.0.5.tar.gz /
ADD ngx_devel_kit-0.3.1.tar.gz /
ADD nginx-1.17.9.tar.gz /

RUN  CONFIG="\
		--prefix=/usr/share/nginx \
		--sbin-path=/usr/sbin/nginx \
		--modules-path=/usr/lib64/nginx/modules \
		--conf-path=/etc/nginx/nginx.conf \
		--error-log-path=/var/log/nginx/error.log \
		--http-log-path=/var/log/nginx/access.log \
		--pid-path=/var/run/nginx.pid \
		--lock-path=/var/run/nginx.lock \
		--http-client-body-temp-path=/var/lib/nginx/client_temp \
		--http-proxy-temp-path=/var/lib/nginx/proxy_temp \
		--http-fastcgi-temp-path=/var/lib/nginx/fastcgi_temp \
		--http-uwsgi-temp-path=/var/lib/nginx/uwsgi_temp \
		--http-scgi-temp-path=/var/lib/nginx/scgi_temp \
		--user=nginx \
		--group=nginx \
		--with-http_ssl_module \
		--with-http_realip_module \
		--with-http_addition_module \
		--with-http_sub_module \
		--with-http_dav_module \
		--with-http_flv_module \
		--with-http_mp4_module \
		--with-http_gunzip_module \
		--with-http_gzip_static_module \
		--with-http_random_index_module \
		--with-http_secure_link_module \
		--with-http_stub_status_module \
		--with-http_auth_request_module \
		--with-http_xslt_module=dynamic \
		--with-http_image_filter_module=dynamic \
		--with-http_geoip_module=dynamic \
		--with-threads \
		--with-stream \
		--with-stream_ssl_module \
		--with-stream_ssl_preread_module \
		--with-stream_realip_module \
		--with-stream_geoip_module=dynamic \
		--with-http_slice_module \
		--with-mail \
		--with-mail_ssl_module \
		--with-compat \
		--with-file-aio \
		--with-http_v2_module \
                --with-ld-opt="-Wl,-rpath,/usr/src/nginx-1.17.9/lua/luajit/lib" \
                --add-module=/usr/src/nginx-$NGINX_VERSION/lua-nginx-module-0.10.15 \
                --add-module=/usr/src/nginx-$NGINX_VERSION/ngx_devel_kit-0.3.1 \
	" \
	&& groupadd  -g 2020 nginx \
	&& useradd -u 2020 -g 2020 -s /sbin/nologin nginx  \
	&& yum install -y install gcc make perl openssl openssl-devel pcre-devel gd-devel \
           zlib-devel GeoIP-devel libxslt-devel perl-ExtUtils-Embed gperftools curl \
        && mkdir -p /var/lib/nginx/{client_temp,proxy_temp,fastcgi_temp,uwsgi_temp,scgi_temp} \
        && mkdir -p /usr/src /etc/nginx/conf.d /usr/share/nginx/html \
	&& mv /nginx-$NGINX_VERSION /usr/src/nginx-$NGINX_VERSION \
	&& cp /usr/src/nginx-$NGINX_VERSION/conf/* /etc/nginx/  \
        && cp /usr/src/nginx-$NGINX_VERSION/conf/server.default /etc/nginx/conf.d/default.conf \
        && mv /LuaJIT-2.0.5 /usr/src/nginx-$NGINX_VERSION/ \
        && mv /lua-nginx-module-0.10.15 /usr/src/nginx-$NGINX_VERSION/ \
        && mv /ngx_devel_kit-0.3.1 /usr/src/nginx-$NGINX_VERSION/ \
        && cd /usr/src/nginx-$NGINX_VERSION/LuaJIT-2.0.5 \
        && make PREFIX=/usr/src/nginx-$NGINX_VERSION/lua/luajit \
        && make install PREFIX=/usr/src/nginx-$NGINX_VERSION/lua/luajit \
        && export LUAJIT_LIB=/usr/src/nginx-$NGINX_VERSION/lua/luajit/lib \
        && export LUAJIT_INC=/usr/src/nginx-$NGINX_VERSION/lua/luajit/include/luajit-2.0 \
	&& cd /usr/src/nginx-$NGINX_VERSION \
	&& ./configure $CONFIG \
	&& make \
	&& make install \
        && ln -sf /dev/stdout /var/log/nginx/access.log \
        && ln -sf /dev/stderr /var/log/nginx/error.log
STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]

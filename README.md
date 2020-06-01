# Nginx Docker Image with suporte to Lua Scripts
Also the [decompress.lua](./decompress.lua) scritp to _decompress_ a gzipped request

## How To Use

**Dockerfile**

```Dockerfile
FROM luizalabs/nginx-lua:0.0.1

# Add your nginx conf
COPY ./nginx.conf /etc/nginx/nginx.conf

CMD ["nginx"]
```

**Nginx.conf**

```nginx
error_log /dev/stdout info;

events {
  worker_connections 1024;
}

http {
  server {
    server_name my-server;

    location / {
      proxy_pass http://my-server:8000;

      # to compress responses
      gzip on;
      gzip_types text/plain application/json;

      # to decompress requests to backend
      set $max_chunk_size 10240;
      set $max_body_size 524288;
      rewrite_by_lua_file /etc/lua/decompress.lua;
    }
  }
}

daemon off;
```

## nginx server menggunakan docker tanpa menggunakan dokcerfile
docker file is good, tapi kebanyakan docker image membuat server cepat penuh, dan penggunaanya harus melakukan build terlebih dahulu.

### PENGGUNAAN
- clone this repo
- run 
    ```docker-compose up -d --build```
- ✨Magic ✨
- untuk konfigurasi nginx, berada di direktori nginx pada root directory
- untuk merubah port silahkan ganti di port docker-compose.yml
- perubahan terjadi setelah container direstart, atau dibuild ulang

### SETUP SEBAGI API GATEWAY

```
# /etc/nginx/nginx.conf

upstream product_command {
    server 192.168.43.16:1111;
    # Add more backend servers as needed
}

upstream product_query {
    server 192.168.43.16:2222;
    # Add more backend servers as needed
}

server {
    listen 80;

    location /products {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;

        if ($request_method = GET) {
            proxy_pass http://product_query;
            break;
        }

        proxy_pass http://product_command;
        break;
    }

    # # jika membuat andpoint baru
}


```

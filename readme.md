### PENGGUNAAN
- clone this repo
- run 
    ```docker-compose up -d --build```
- ✨Magic ✨
- untuk konfigurasi nginx, berada di direktori nginx pada root directory
- untuk merubah port silahkan ganti di port docker-compose.yml
- perubahan terjadi setelah container direstart, atau dibuild ulang

### SETUP NGIN SEBAGAI API GATEWAY
pada contoh kali ini, misalnya kita mebangun fitur CRUD products kita bisa melakukan 
- GET /products - list product
- GET /products/:id - detail product
- POST /products - add a new product
- PUT /products/:id - update a product
- DELETE /products/:id - delete a product

berarti hanya perlu menjadi dua pembagian. Karena semua retrive data dilakukan di method GET.

```
# /etc/nginx/nginx.conf

# loadbalancer
upstream product_command {
    server 192.168.43.16:1111;
    # Add more backend servers as needed
}

# loadbalancer
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
    # # jika membuat baru
    # location /users {
    #     proxy_pass http://ur-backend-server;
    #     proxy_http_version 1.1;
    #     proxy_set_header Upgrade $http_upgrade;
    #     proxy_set_header Connection 'upgrade';
    #     proxy_set_header Host $host;
    #     proxy_cache_bypass $http_upgrade;
    #     break;
    # }
}

```

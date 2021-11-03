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
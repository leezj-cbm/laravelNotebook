# Setting Up Redis for Laravel

1. Installation of redis WSL : https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-on-windows/

2. Start redis server ```$ sudo redis-server start```

3. Change the .env file to CACHE_STORE = redis

4. Current PHP version is 8.2 . Download from here: https://pecl.php.net/package/redis/6.0.2/windows . ***Select: PHP Version 8.2 ,Thread safe x64 ***

5. Place the downloaded php_redis.dll file in xampp/php/ext

6. Open: xampp/php/php.ini using notepad . Add this among the extension group
    ```
    extension=redis
    ```

7. Run ```php artisan migrate:fresh --seed```

8. Don't forget to ``` php artisan passport:client --personal```

9. Should be good to go.
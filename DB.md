# DB

If you want to change DB connection go to .env file and change

DB_CONNECTION= xxx

By default it is connected to sqlite (for beginners)


### Change to MYSQL using XAMP

1. Install and configure XAMPP
2. Start mysql service
3. Click admin beside mysql to run phpMyAdmin
4. Create a new database
5. Create and set new account to access(name and password, privilages)
 6. Update the .env file to include server port, name , password, host
7. Migrate the file again , php artisan migrate:refresh --seed

You should be able to see values in myPHPAdmin
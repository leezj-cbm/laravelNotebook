# Passport in Laravel

Laravel official Documentation - https://laravel.com/docs/11.x/passport


## Important Summary

1. In the php.ini file, need to enable extensions. If not composer cannot install new api.

2. Use the composer command to install laravel passport

3. Install passport api

4. Print out the public and private keys using the artisan command. Two files will appear in storage. Put the private and public key in the .env file

5. Create the personal access client and password grant client. This is mandatory to issue tokens. A generic client will not do!!

6. Follow the tutorial steps. Main files involved are:-
    - App/Http/Models/User.php
    - App/Providers/AppServiceProvider
    - config/auth.php
    - config/passport.php - refer
    - .env
    - routes/api.php
    - app/Http/Controller ---> either make a new controller or modify existing one for register and login function


## Useful StackOverflow

### Creating Clients

Ensuring Proper Client Types

When you use php artisan passport:install, it typically creates the necessary clients with the correct types. However, if you manually create a client using php artisan passport:client, you need to specify the type of client it is.

<----IMPORTANT NOTE!!!! (stack overflow thread does not cover this) ----->

Personal access client ```php artisan passport:client --personal``` -  A client that is used to issue personal access tokens.

Password grant client  ```php artisan passport:client --password``` - A client that is used for password grant tokens.

<------------------------------------------------------------------------>

### Private and public keys

https://stackoverflow.com/questions/74054209/how-to-use-env-to-load-passport-oauth-keys-in-laravel

Follow these steps to solve the problem

If you haven't install Passport package in your project, run this command composer require laravel/passport

If you haven't migrated the Passport tables, run this command ```php artisan migrate```

The command below will create encryption keys needed to generate access keys. It will also generate personal access ```php artisan passport:install```

Make sure you remove LoadKeys from AuthServiceProvider to avoid it from loading keys from storage. Remove this line ```Passport::loadKeysFrom(__DIR__.'/../secrets/oauth');```

Publish Passport Configuration, so you will have passport config in the config folder. ```php artisan vendor:publish --tag=passport-config```

The passport config will include private_key, public_key, and personal_access_client configurations

If you don't have the keys generated, php artisan passport:keys will generate both private and public keys.

If you don't have the personal access client generated, ```php artisan passport:client``` will generate both PASSPORT_PERSONAL_ACCESS_CLIENT_ID and PASSPORT_PERSONAL_ACCESS_CLIENT_SECRET.

Your .env file should have these keys

PASSPORT_PERSONAL_ACCESS_CLIENT_ID="" PASSPORT_PERSONAL_ACCESS_CLIENT_SECRET=""

PASSPORT_PRIVATE_KEY ="-----BEGIN PRIVATE KEY----- -----END PRIVATE KEY-----"

PASSPORT_PUBLIC_KEY="-----BEGIN PUBLIC KEY----- -----END PUBLIC KEY-----"

Your PASSPORT_PRIVATE_KEY and PUBLIC_KEY can be found in the storage folder


## Useful tutorials

The tutorials are scattered and do not paint the full picture. See summary below!

1. First choice :- https://www.itsolutionstuff.com/post/laravel-11-rest-api-with-passport-authentication-tutorialexample.html

2. 2nd choice:-https://www.binaryboxtuts.com/php-tutorials/laravel-tutorials/laravel-11-api-authentication-using-laravel-passport/#google_vignette

3. 3rd choice - https://www.linkedin.com/pulse/laravel-passport-oauth-2-authentication-prakash-joshi-hcudf/

4. 4th choice - https://itcodstuff.com/blog/laravel-passport-rest-api-authentication-example/

5. 5th choice - https://dev.to/mahmudulhsn/laravel-passport-api-authentication-with-access-and-refresh-token-16d0


## General Read :Storing in DB

Storing Passport tokens in the database is the default and standard practice when using Laravel Passport. This approach is safe as long as you follow best practices for securing your application and database.

Here are some key considerations to ensure the safety of storing Passport tokens in the database:

1. Secure Database Connection

    Use SSL/TLS: Ensure that your database connection uses SSL/TLS to encrypt data in transit.
    Strong Authentication: Use strong credentials and consider using multi-factor authentication (MFA) for database access.

2. Encrypt Sensitive Data

    Encryption: While Passport stores tokens securely, consider encrypting sensitive fields in your database where necessary.

3. Regular Security Audits

    Vulnerability Scanning: Regularly scan your application and database for vulnerabilities.
    Update Dependencies: Keep your Laravel, Passport, and other dependencies up-to-date with security patches.

4. Proper Configuration

    Environment Variables: Store database credentials and other sensitive configurations in environment variables (.env file) instead of hardcoding them in your code.
    Environment Separation: Use separate databases and configurations for development, staging, and production environments.

5. Access Controls

    Database Permissions: Limit database permissions to only what is necessary for the application to function.
    Role-Based Access Control (RBAC): Implement RBAC within your application to ensure that users and services have the minimum necessary access.

6. Secure the API

    HTTPS: Always use HTTPS to encrypt data transmitted between the client and your API.
    Rate Limiting: Implement rate limiting to prevent abuse of your API endpoints.
    Implementation in Laravel Passport

    Laravel Passport automatically handles the storage of access tokens, refresh tokens, and client credentials in the database. The tokens are stored in the following tables:

    - oauth_access_tokens :  Stores access tokens for API access.
    - oauth_refresh_tokens :  Stores refresh tokens to obtain new access tokens.
    - oauth_clients : Stores client application information.
    - oauth_auth_codes : Stores authorization codes for the authorization code grant flow

    When a user or client is issued a token, Passport stores the token in the oauth_access_tokens table. This table includes fields such as id, user_id, client_id, name, scopes, revoked, and expires_at.

    Token Encryption
    Laravel Passport uses encryption to secure the tokens stored in the database. You can further secure the tokens by configuring Passport to use encrypted storage.

Conclusion
Storing Passport tokens in the database is a common and secure practice as long as you follow the outlined security measures. Ensuring that your database and application configurations adhere to best security practices will help maintain the integrity and confidentiality of the tokens.
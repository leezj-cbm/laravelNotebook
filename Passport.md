# Passport in Laravel

Laravel official Documentation - https://laravel.com/docs/11.x/passport

## Useful tutorials

1. First choice :- https://www.itsolutionstuff.com/post/laravel-11-rest-api-with-passport-authentication-tutorialexample.html

2. 2nd choice:-https://www.binaryboxtuts.com/php-tutorials/laravel-tutorials/laravel-11-api-authentication-using-laravel-passport/#google_vignette

3. 3rd choice - https://www.linkedin.com/pulse/laravel-passport-oauth-2-authentication-prakash-joshi-hcudf/

4. 4th choice - https://itcodstuff.com/blog/laravel-passport-rest-api-authentication-example/


## Storing in DB

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
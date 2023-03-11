# Environment Configuration
***

# How to create safety .env file in Laravel project?
***

# Contents of this file
***
- Introduction
- Example and Prevention
- Maintainers

# Introduction
***

The ```.env``` file in Laravel is a configuration file that contains sensitive information.
In your main Laravel folder you should have .env file which contains various settings, one row – one KEY=VALUE pair. 
And then, within your Laravel project code you can get those environment variables with function env(‘KEY’).
 By following these steps, you can help to protect your application's sensitive data and prevent vulnerabilities in your ```.env``` file


More info in the [Official Laravel Documentation](https://laravel.com/docs/master/configuration#environment-configuration)

# Example and Prevention
***

## Encryption
To encrypt an environment file, you may use the ```command:env:encrypt```:

```
php artisan env:encrypt
```

Running the command will encrypt your file and place the encrypted contents in an file. 
The decryption key is presented in the output of the command and should be stored in a secure password manager. 
If you would like to provide your own encryption key you may use the option when invoking the ```command:env:encrypt.env.env.encrypted--key```

```
php artisan env:encrypt --key=3UVsEgGVK36XN82KKeyLFMhvosbZN1aF
```

If your application has multiple environment files, such as and , you may specify the environment file that should be encrypted by providing
the environment name via the ```option:.env.env.staging--env```

```
php artisan env:encrypt --env=staging
```

## Decryption
To decrypt an environment file, you may use the command. This command requires a decryption key, 
which Laravel will retrieve from the environment variable: ```env:decrypt```  ```LARAVEL_ENV_ENCRYPTION_KEY```

```
php artisan env:decrypt
```

Or, the key may be provided directly to the command via the ```option:--key```

```
php artisan env:decrypt --key=3UVsEgGVK36XN82KKeyLFMhvosbZN1aF
```

### Not included in the version control system by default
Teamwork and working with git repositories is that .env file is NOT committed to the repository, it is included in .gitignore file.

add ```.gitignore``` file :

``` 
/*
!.env  
```

## Built-in environment variable protection
```
APP_KEY=
```

## Generate a new application key

```
php artisan key:generate
```

## Make sure your app is not in debug mode while in production
```
APP_DEBUG=false
```

## Path Traversal
Similar to unrestricted file uploads, you should use the PHP function to strip out directory information like so:```basename```

```
Route::get('/download', function(Request $request) {
    return response()->download(storage_path('content/').basename($request->input('filename')));
});
```
Read more : [OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Laravel_Cheat_Sheet.html#cookie-security-and-session-management)

## Permission 
Make sure that CHMOD for your .env file should be 400 or 440 so that it can not be accessed from outside the public folder.

in Linux:
```
chmod 440 .env
```

## Block access in ```.htaccess```
```.htaccess``` contains superpower, you can set env file permission but blocking that file via ```.htaccess```is very useful. add this code in the .htaccess file :

```
<FilesMatch "^\.env">
    Order allow,deny
    Deny from all
</FilesMatch>
```

also, protect dot files with this:

```
## Block access to dot file
location ~ /. {
    deny  all;
}
```

## Move the file outside of the project root directory ```.env```
By default, the file is located in the project root directory. You can move it outside the project root directory and update the variable in the file to reflect 
the new path. This way, even if someone gains access to your project files, they won’t have access to the file. ```.env APP_ENV.env.env ```

### Safety share ```.env``` secret 

Envault is a tool to share .env secrets. It lets you manage and sync your entire team’s local .env files, across all your projects, 
so you’re all kept up to date with the latest changes.

[Envault.dev](https://envault.dev/)


# Maintainers
***
- Brigitta Bujdosone Kovacs -(ISC)² Candidate- [kovacsbrigi.hu](https://kovacsbrigi.hu/) 

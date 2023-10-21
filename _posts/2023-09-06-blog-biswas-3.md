---
title: 'Basics of Laravel -- A PHP Framework'
date: 2023-09-08
permalink: /posts/2023/09/biswas/blog-laravel/
tags:
  - laravel
---
Laravel is an open-source PHP web framework that follows the `model-view-controller (MVC)` design pattern. It hosts two major tools: i) composer and ii) artisan. The `Composer` includes all the dependencies and libraries. It allows a user to create a project with respect to the mentioned framework (for example, those used in Laravel installation). Third party libraries can be installed easily with help of composer. All the dependencies are noted in composer.json file which is placed in the source folder. `Artisan` includes a set of commands which assists in building a web application. These commands are incorporated from Symphony framework, resulting in add-on features in Laravel 5.1+.

## Prerequisites
* Assuming, you have PHP and `VSCode` installed in your system.

## Setting up Laravel
### Setting up Composer
* Get and install composer from [https://getcomposer.org/download/](https://getcomposer.org/download/) by following the instructions.

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```
* This installer script will simply check some `php.ini` settings, warn you if they are set incorrectly, and then download the latest `composer.phar` in the current directory. The 4 lines above will, in order:
  * Download the installer to the current directory
  * Verify the installer SHA-384, which you can also cross-check here
  * Run the installer
  * Remove the installer
* Most likely, you want to put the composer.phar into a directory on your `PATH`, so you can simply call `composer` from any directory (Global install), using for example:
  * `mkdir /Users/ashis/_Applications`
  * `mv composer.phar _Applications`
  * `ln -s /Users/ashis/_Applications/composer.phar /Users/ashis/_Applications/composer`
  * `vi ~/.zshrc`
    * `export PATH="/Users/ashis/_Applications/:$PATH"`
    * `source ~/.zshrc`
  * Test if `composer` command works from the command line.

### Setting up Node.js and NPM
* `Node.js` has installer for all OS at this site [https://nodejs.org/en](https://nodejs.org/en). It also installs `npm`, a package manager for javascript.
  * Follow the step by running the installer and make a note on the install path. In my case it was installed at:

```bash
This package has installed:
	•	Node.js v18.17.1 to /usr/local/bin/node
	•	npm v9.6.7 to /usr/local/bin/npm
```
* Check the installation of node.js and npm by typing the following:

```bash
ashis@Ashiss-MacBook-Pro ~ % node -v
v18.17.1
ashis@Ashiss-MacBook-Pro ~ % npm -v
9.6.7
```
* Upgrade `npm` with `sudo npm install -g npm@10.0.0`
* Also run `npm install`

### Now, install and configure Laravel
* Install Laravel with `composer`:

```bash
ashis@Ashiss-MacBook-Pro ~ % composer global require laravel/installer
```
* Add the laravel executable in `PATH`:
  * in MacOS: `export PATH="$HOME/.composer/vendor/bin/:$PATH"`
  * in Ubuntu: `export PATH="$HOME/.config/composer/vendor/bin/:$PATH"`
* Check `laravel` installation with

```bash
ashis@Ashiss-MacBook-Pro ~ % laravel   
Laravel Installer 5.1.0

Usage:
  command [options] [arguments]

Options:
  -h, --help            Display help for the given command. When no command is given display help for the list command
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi|--no-ansi  Force (or disable --no-ansi) ANSI output
  -n, --no-interaction  Do not ask any interactive question
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  completion  Dump the shell completion script
  help        Display help for a command
  list        List commands
  new         Create a new Laravel application
```

## Create new laravel project
* `laravel new mlcompetition`
  * It may ask for 3 options: i) No starter kit, ii) Laravel breeze, iii) Laravel jetstream. Details can be found [here](https://laravel.com/docs/10.x/starter-kits). Please use either of `breeze` or `jetstream` template to integrate the `authentication/login/register` template/options. 

```bash
┌ Would you like to install a starter kit? ────────────────────┐
 │ Laravel Breeze                                               │
 └──────────────────────────────────────────────────────────────┘

 ┌ Which Breeze stack would you like to install? ───────────────┐
 │ Blade                                                        │
 └──────────────────────────────────────────────────────────────┘

 ┌ Would you like dark mode support? ───────────────────────────┐
 │ Yes                                                          │
 └──────────────────────────────────────────────────────────────┘

 ┌ Which testing framework do you prefer? ──────────────────────┐
 │ PHPUnit                                                      │
 └──────────────────────────────────────────────────────────────┘

 ┌ Would you like to initialize a Git repository? ──────────────┐
 │ Yes                                                          │
 └──────────────────────────────────────────────────────────────┘

┌ Which database will your application use? ───────────────────┐
 │ MySQL                                                        │
 └──────────────────────────────────────────────────────────────┘
```
* Open the project folder with `VSCode`.
  * `./composer.json` : contains the project's assets.
  * `./resources/views/welcome.blade.php` is the default welcome page. `blade` is a rendering template.
  * `./resources/views/dashboard.blade.php` is a view file that gets loaded when a user successfully logs in.
* Type `php artisan` to see all different options/commands. Get familiar with all the available commands. 
* Serve the project into built-in web server: `php artisan serve`

### Connecting a MySQL Database with the Laravel Project
* **Setting the Connection configurations**: Make sure you edit the 2 following files:
  * `.env` file: Check the `DB_` entries.
  * `config/database.php` file: check the `mysql` connection entries.
* **Create a view**:
  * `resources/views/dbconn.blade.php`

```php
<?php
if(DB::connection()->getPdo()){
	echo "Successful connection to the database named ".DB::connection()->getDatabaseName();
}else{
	echo "Unsuccessful connection to the database named ".DB::connection()->getDatabaseName();
}
?>
```
* Then, add a route in the `routes/web.php` file:

```php
Route::get('/dbconn', function(){
    return view('dbconn');
});
```
* Now, go check if `localhost:8000/dbconn` is successful or not.
### Front-end
* In the project directory run: `npm run build`. Here are the explanations for this command:
  * `npm`: One could say that it is a command native to the system, for calling `Node Package Manager` program. 
  * `run`: It is a command native to `npm`. Keep in mind this is an alias to the original command `run-script`.
  * `dev`/`build` and `prod`: They're user defined.
      * `dev`/`build`: Used for running the specific commands for serving the project, to any server, to live development. In the case of a web page, you'll see your web page in the browser, and any change you make to the HTML code, for example, will be reflected immediately in the page you see in your browser.
      * `prod`: Compiles all the necessary files for production. Final product. In the case of a web page, for example, the HTML, CSS, and JS files you'll handle to the client. The result of running this command, it is expected to be one single folder with all the afore mentioned content.
  * It creates the following files in `./public` directory:

```bash
ashis@Ashiss-MacBook-Pro mlcompetition % tree public 
public
├── build
│   ├── assets
│   │   ├── app-6e0eadfb.js
│   │   └── app-e3a33e56.css
│   └── manifest.json
├── favicon.ico
├── index.php
└── robots.txt

2 directories, 6 files
```

  * You will not be directly editing any of these files, instead you may edit `./resources/css/app.css` and `./resources/js/app.js` files.

### Databases with Migrations
* Migrations are like version control for your database, allowing your team to define and share the application's database schema definition. If you have ever had to tell a teammate to manually add a column to their local database schema after pulling in your changes from source control, you've faced the problem that Laravel's database migrations solve.
* The Laravel Schema facade provides database agnostic support for creating and manipulating tables across all of Laravel's supported database systems. Typically, migrations will use this facade to create and modify database tables and columns
* Before you do anything, edit the following entries in `.env` file at the project root directory:

```bash
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=xxxx
DB_USERNAME=xxxx
DB_PASSWORD=xxxx
```
* Also, go to `./config/database.php` and update the following as needed:

```php
'mysql' => [
            'driver' => 'mysql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'xxxxx'),
            'username' => env('DB_USERNAME', 'xxxxx'),
            'password' => env('DB_PASSWORD', 'xxxxx'),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ],
```
* **Migrate**
  * Do it first time: `php artisan migrate`
    * This will create few tables in the chosen database:
      * `users` -- all user info will be recorded here.
      * `password_reset_tokens` -- will store tokens if someone forgets their password
      * `password_access_tokens`, 
      * `failed_jobs`, 
      * `migrations`.
  * Right after migration, go hit the `Register` button in the website (e.g., `http://127.0.0.1:8000/`).
    * Provide information to register an account. The site will then log you into the account.
    * Once logged in, the route shown would be: `http://127.0.0.1:8000/dashboard`
    * You can check that `dashboard` view when routed would be inaccessible if logged out.

```bash
ashis@Ashiss-MacBook-Pro lara2site % php artisan migrate

   INFO  Preparing database.  

  Creating migration table ..................................................................................................... 16ms DONE

   INFO  Running migrations.  

  2014_10_12_000000_create_users_table ......................................................................................... 20ms DONE
  2014_10_12_100000_create_password_reset_tokens_table ......................................................................... 14ms DONE
  2019_08_19_000000_create_failed_jobs_table ................................................................................... 12ms DONE
  2019_12_14_000001_create_personal_access_tokens_table ........................................................................ 16ms DONE
```
### Working on the views
* Edit `resources/views/layouts/app.blade.php` to change the design.
* Add figures for the site in `public/`. Let's create `figs/` folder inside it which will contain all figures for the site. You may use the files in `figs/` considering `public/` as the site root directory.
  * The public site never has access outside the `public/` directory.


* **Generating migrations**
  * You may use the `make:migration` Artisan command to generate a database migration. The new migration will be placed in your `database/migrations` directory. Each migration filename contains a timestamp that allows Laravel to determine the order of the migrations: `php artisan make:migration create_flights_table`.

```bash
ashis@Ashiss-MacBook-Pro mlcompetition % php artisan make:migration create_flights_table

   INFO  Migration [database/migrations/2023_09_13_180750_create_flights_table.php] created successfully.
```
  * Content of that file would be something like below:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * The database connection that should be used by the migration.
     *
     * @var string
     */
    protected $connection = 'mysql';

    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('flights', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('airline');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('flights');
    }
};
```
  * Laravel will use the name of the migration to attempt to guess the name of the table and whether or not the migration will be creating a new table. If Laravel is able to determine the table name from the migration name, Laravel will pre-fill the generated migration file with the specified table. Otherwise, you may simply specify the table in the migration file manually.
* **Squashing migrations**
  * As you build your application, you may accumulate more and more migrations over time. This can lead to your database/migrations directory becoming bloated with potentially hundreds of migrations. If you would like, you may `squash` your migrations into a single `SQL` file. To get started, execute the `schema:dump` command:

```bash
php artisan schema:dump
 
# Dump the current database schema and prune all existing migrations...
php artisan schema:dump --prune
```
  * When you execute this command, Laravel will write a "schema" file to your application's database/schema directory. The schema file's name will correspond to the database connection. Now, when you attempt to migrate your database and no other migrations have been executed, Laravel will first execute the SQL statements in the schema file of the database connection you are using. After executing the schema file's SQL statements, Laravel will execute any remaining migrations that were not part of the schema dump.
  * Migration squashing is only available for the MySQL, PostgreSQL, and SQLite databases and utilizes the database's command-line client. Schema dumps may not be restored to in-memory SQLite databases.
* **Migration Structure**
  * A migration class contains two methods: `up` and `down`. 
    * The `up` method is used to add new tables, columns, or indexes to your database,
    * The `down` method should reverse the operations performed by the `up` method.
  * Within both of these methods, you may use the Laravel schema builder to expressively create and modify tables. 
* **Setting The Migration Connection**
  * If your migration will be interacting with a database connection other than your application's default database connection, you should set the $connection property of your migration in the migration files: `protected $connection = 'mysql';`
* **Running Migrations**
  * To run all of your outstanding migrations, execute the migrate Artisan command: `php artisan migrate`
  * If you would like to see which migrations have run thus far, you may use the `migrate:status` Artisan command: `php artisan migrate:status`
  * If you would like to see the SQL statements that will be executed by the migrations without actually running them, you may provide the `--pretend` flag to the migrate command: `php artisan migrate --pretend`
* **Rolling back migrations**
  * To roll back the latest migration operation, you may use the rollback Artisan command. This command rolls back the last "batch" of migrations, which may include multiple migration files: `php artisan migrate:rollback`
  * If you would like to see the SQL statements that will be executed by the migrations without actually running them, you may provide the `--pretend` flag to the migrate:rollback command: `php artisan migrate:rollback --pretend`
  * The `migrate:reset` command will roll back all of your application's migrations: `php artisan migrate:reset`
* **Rollback and Migrate using a single command**
  * The `migrate:refresh` command will roll back all of your migrations and then execute the `migrate` command. This command effectively re-creates your entire database:
  * `php artisan migrate:refresh`
* **Drop all tables and Migrate**
  * The `migrate:fresh` command will drop all tables from the database and then execute the migrate command: `php artisan migrate:fresh`
    * The `migrate:fresh` command will drop all database tables regardless of their prefix. This command should be used with caution when developing on a database that is shared with other applications.
## References (as of 9/6/2023)
* 
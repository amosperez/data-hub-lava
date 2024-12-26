## Setting Up Laravel with Docker
This guide outlines the steps to run a container, install a Laravel project within it, mount the project to the host directory, and set up Laravel Sail for containerized development.

### 1. Start a PHP containe 

Run a PHP container, mounting the application folder on your host machine to /var/www/html in the container:
```
docker run -it --rm \
  -v $(pwd)/application:/var/www/html \
  -w /var/www/html \
  php:8.2-cli bash
```
**Explanation:**
- `-v $(pwd)/application:/var/www/html`: Mounts the application folder on your host to the container.
- `-w /var/www/html`: Sets the container's working directory.
- `php:8.2-cli`: Uses the PHP CLI Docker image.

<br>
<br>

### 2. Install Composer

Inside the container, download and install Composer:
```
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```
**Explanation:**
- `curl`: Fetches the Composer installer script.
- `mv composer.phar /usr/local/bin/composer`: Moves the composer binary to a globally accessible location.

<br>
<br>

### 3. Install Required Tools and Extensions

Install necessary tools (zip, unzip, git) and PHP extensions for Laravel:
```
apt-get update && apt-get install -y \
    zip unzip git libzip-dev
```
**Explanation:**
- `apt-get update`: Updates package lists.
- `apt-get install`: Installs required tools and libraries.
- `zip, unzip`: Required for handling .zip files.
- `git`: Required for source code operations.
- `libzip-dev`: Required for the PHP zip extension.

<br>
<br>

### 4. Create a New Laravel Project

Install a new Laravel project using Composer:
```
composer create-project --prefer-dist laravel/laravel .
```
**Explanation:**
- `composer create-project`: Installs a new Laravel project.
- `--prefer-dist`: Downloads the pre-built package for faster setup.
- `.`: Installs the project in the current directory.

<br>
<br>

### 5. Install Laravel Sail

Add Laravel Sail to the project as a development dependency:
```
composer require laravel/sail --dev
```
**Explanation:**
- `composer require`: Adds the specified package to the project.
- `--dev`: Installs Sail as a development-only dependency.

<br>
<br>

### 6. Install Sail Configuration

Set up Sail for the project:
```
php artisan sail:install
```
**Explanation:**
- `php artisan sail:install`: Configures Laravel Sail, letting you choose services like MySQL, Redis, etc.

<br>
<br>

### 7. Define a Sail Alias

On your host machine, define an alias for temporary Sail to simplify its usage:
```
alias sail="./vendor/bin/sail"
```
**Explanation:**
- This command creates a temporary alias for the current terminal session. It allows you to run `sail` instead of `./vendor/bin/sail`.

<br>
<br>

### 8. Start Sail Services

Navigate to the `application` folder on your host and start the Sail containers in detached mode:
```
sail up -d
```
**Explanation:**
- `sail up -d`: Starts the Docker containers in detached mode (runs in the background).
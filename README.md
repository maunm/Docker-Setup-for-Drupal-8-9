# Docker Environment Setup - Drupal 9

The project structure is based on drupal/recommended-project.
More information here: https://www.drupal.org/docs/develop/using-composer/starting-a-site-using-drupal-composer-project-templates

## Environment information.
PHP: 7.4
Apache: 2.4
MySql: 5.7
Memcached: 3.1.3

## Local Docker Setup.
1. Copy the `.docker.env.default` file and name it as `.docker.env`.
2. You can leave the file as it is or do any update if you want.
3. Make sure to be in the project root and run in the terminal `./start`. This will download and start Docker containers.

## Drupal Installation

In the `/web` folder we have the Drupal project.

We also need to setup the database connection:

1. Go to the project root.
2. Go to `sites/`.
3. Copy the file `example.settings.local.php` and name it as `settings.local.php`  and paste it inside of `sites/default/`.
4. Update the DB connection information if it's needed, for this Docker setup will be:

```
$databases['default']['default'] = array (
  'database' => 'drupal',
  'username' => 'root',
  'password' => 'root',
  'prefix' => '',
  'host' => 'db',
  'port' => '3306',
  'namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
  'driver' => 'mysql',
);

```

8. You should be able to see the sites by going to:

http://localhost:8888/  Drupal Web Page
http://localhost:8889/  PHPMYADMIN

## Drush & Composer

To Run Drush
- Enter on PHP container: `docker exec -it drupal_web /bin/bash`
- Import config: `drush cim`
- Export config: `drush cex`
- Clear cache: `drush cr`


Run Composer commands inside PHP container as well.

- Enter on PHP container: `docker exec -it drupal_web /bin/bash`
- Inside of the container run Drush commands: `composer install`

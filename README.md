# Laraset

From the root directory of a fresh Laravel application run:

```shell
curl -s https://aldemeery.github.io/laraset/command.stub.php -o app/Console/Commands/AppInstallCommand.php --create-dirs && php artisan app:install
```

If you're using Sail:

```shell
curl -s https://aldemeery.github.io/laraset/command.stub.php -o app/Console/Commands/AppInstallCommand.php --create-dirs && sail php artisan app:install
```

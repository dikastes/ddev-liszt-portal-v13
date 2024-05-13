# TYPO3 Distribution - GitLab Project Template

Get going quickly with TYPO3 CMS and GitLab.

## Quickstart

Use the following ddev command to set up the template

`ddev typo3-init`

**NOTE**

> For the quickstart, you have **[ddev](https://ddev.readthedocs.io/en/stable/)** to
be installed on your host system. For further details, head over to the official **[documentation](https://ochorocho.gitlab.io/typo3-distribution-docs/)**.


## All commands - manual setup

Configure ddev, install packages and start the ddev instance:

```
ddev start
ddev composer install
ddev typo3 setup
```

Prepare and build frontend:

```
ddev npm install
ddev npm run build:production
```

Initialize data (page tree and link assets):

```
ddev typo3 extension:setup
ddev composer dumpautoload
```

Display all your project related information:

```
ddev describe
```

## Mail GUI

Start built-in [Mailpit](https://github.com/axllent/mailpit) to access emails sent by TYPO3

```
ddev launch -m
```

## Database GUI

To access the database via a web GUI, you can install e.g. [phpmyadmin](https://www.phpmyadmin.net/)
`ddev get ddev/ddev-phpmyadmin` or [adminer](https://www.adminer.org/de/) `ddev get ddev/ddev-adminer`.

## Use Vite.js [dev server for ddev](https://github.com/torenware/ddev-viteserve#getting-started)

```
ddev get torenware/ddev-viteserve
ddev restart
ddev vite-serve start
```

```
ddev vite-serve start && ddev typo3 cache:flush
ddev vite-serve stop && ddev typo3 cache:flush
```

## Files and folders

The folder `/packages` contains all your local extension/packages.
Require these packages simply by using `composer req vendor/package:@dev`

`assets` contains all scss, javascript, images and fonts which will be processed
by [Vite.js](https://vitejs.dev/) and stored in `/packages/liszt_web/Resources/Public/`.

## Npm Scripts / Vite.js

The frontend toolchain uses NPM and Vite.js with a few loaders to ...
  * Compile scss to css (`assets/Scss`)
  * Bundle javascript (`assets/JavaScript`)
  * Copy images (`assets/Image`) and fonts (`assets/Fonts`) to the Public folder of EXT:liszt_web

Watch for changes in js/scss files:
```
ddev npm run watch
```

Build JS, CSS for development use (not compressed/optimized):
```
ddev npm run build:development
```

Build JS, CSS for production use:
```
ddev npm run build:production
```

## QA / Analysis

Run PHPStan:
```
ddev exec ./vendor/bin/phpstan analyse -c .phpstan.neon --no-progress
```

PHP CS Fixer:
```
ddev exec ./vendor/bin/php-cs-fixer fix --dry-run --diff
```

## Composer Scripts - GitLab Runner

Run the `build_assets` Job:
```
composer run-script runner:assets
```

Run the `php_cs_fixer` Job:
```
composer run-script runner:fixer
```

Run the `package_install` Job:
```
composer run-script runner:install
```

Run the `phpstan` Job:
```
composer run-script runner:phpstan
```

## Deployer

`deploy.yaml` contains an example configuration for deployer
(PHP deployment tool). It's recommended to run [deployer](https://deployer.org/)
in GitLab CI.

Run deployer locally (only for testing):
```
./vendor/bin/dep deploy -vvv staging
```

## TYPO3 distribution documentation

https://ochorocho.gitlab.io/typo3-distribution-docs/

## Scheduler CronJob

To run a CronJob in ddev the plugin "ddev-cron" is required.
The add-on is installed during `ddev typo3-init`.
To install it manually run `ddev get ddev/ddev-cron`.
In case xdebug is enabled the scheduler CronJob is not executed.

Run the scheduler manually:

```bash
ddev typo3-scheduler # -f or --force to run it while xdebug is enabled
```

## External documentation

  * TYPO3 - https://docs.typo3.org/
  * DDEV - https://ddev.readthedocs.io/en/stable/
  * Vite.js - https://vitejs.dev/
  * Deployer - https://deployer.org/docs/7.x/basics

## License

GPL-2.0 or later



## Install new
- clone repository
- clone packages in /packages directory  /take care of the right branch (typo v13)
- run typo3 init script or insert /config/system/settings.php for typo3 installation
- restore database snapshot (ddev snapshot restore)
- npm install ( if manually insert settings.php)
- ddev restart
- ddev get torenware/ddev-viteserve
- ddev vite-serve start && ddev typo3 cache:flush


## MySQL Snapshot
ddev snapshot --name mysnapshot

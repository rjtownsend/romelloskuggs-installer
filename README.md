# ROMELLO SKUGGS INSTALLER

## A starter website for Drupal 8 Developers

Optional Drupal installer used for the [Romello Skuggs](https://github.com/rjtownsend/romelloskuggs) Drupal 8 Installation Profile. 
The project goal of Romello Skuggs is to provide a starter website for developers and site-builders similar to a starter theme 
for front-end developers. This installer provides the base ```composer.json``` file required to build any Drupal site. 

This installer was heavily borrowed from [Composer template for Drupal projects](https://github.com/drupal-composer/drupal-project). 

Usage:

```
composer create-project rjtownsend/romelloskuggs-installer:8.x-dev some-dir --no-interaction

# Navigate to http://website.local/core/install.php and install as usual
#
# Export site config to the config/sync directory
cd some-dir
drush cex -y
#
# Enabling settings.local.php is recommended during development, 
# Uncomment on web/sites/default/settings.php to enable
# Alternatively, use the command drupal site:mode dev
#
# Optional: create a bootstrap_sass subtheme using the subtheme installation script
# Full instructions for installing can be found here: https://www.drupal.org/project/bootstrap_sass
cd web/themes/contrib/bootstrap_sass
./scripts/create_subtheme.sh
#
# Subtheme will need to be enabled through the Drupal UI
```

## What does the Romello Skuggs Installer do?

When installing using the Romello Skuggs Installer ```composer.json``` file, the following tasks are taken care of: 

* Drupal will be installed in the `web`-directory.
* Autoloader is implemented to use the generated composer autoloader in `vendor/autoload.php`,
  instead of the one provided by Drupal (`web/vendor/autoload.php`).
* Modules (packages of type `drupal-module`) will be placed in `web/modules/contrib/`
* Theme (packages of type `drupal-theme`) will be placed in `web/themes/contrib/`
* Profiles (packages of type `drupal-profile`) will be placed in `web/profiles/contrib/`
* Custom modules (packages of type `drupal-custom-module`) will be placed in `web/modules/custom/`
* Custom themes (packages of type `drupal-custom-theme`) will be placed in `web/themes/custom/`
* Creates default writable versions of `settings.php` and `services.yml`.
* Creates settings.local.php; this needs to be uncommented in settings.php to enable use. 
* Creates `web/sites/default/files`-directory.
* Latest version of drush is installed locally for use at `vendor/bin/drush`.
* Latest version of DrupalConsole is installed locally for use at `vendor/bin/drupal`.
* Creates `config/sync` directory for Drupal config yml files
* Sets the `bootstrap_sass` subtheme installation script as executable
* Creates environment variables based on your .env file. See [.env.example](.env.example).

## How to Update Drupal Core

The ```composer.json``` will attempt to keep all of your Drupal Core files up-to-date; the 
project [drupal-composer/drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold) 
is used to ensure that your scaffold files are updated every time drupal/core is 
updated. If you customize any of the "scaffolding" files (commonly .htaccess), 
you may need to merge conflicts if any of your modified files are updated in a 
new release of Drupal core.

Follow the steps below to update your Drupal core.

1. Run `composer update drupal/core webflo/drupal-core-require-dev "symfony/*" --with-dependencies` to update Drupal Core and its dependencies.
1. Run `git diff` to determine if any of the scaffolding files have changed. 
   Review the files for any changes and restore any customizations to 
  `.htaccess` or `robots.txt`.

## Working with Git

The included ```.gitignore``` file excludes core, contrib modules, contrib themes, more. 
You will need to undate ```.gitignore``` if you wish to include those files. Note that
Composer does not recommend committing these files to git but also provides [workarounds](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md)
if you decide to anyways.

If you have not customized scaffold files (like index.php, updated.php, etc.) you could choose 
to not check them into your version control system (e.g. git). If that is the case for your project it might be
convenient to automatically run the [drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold) plugin after every install or update of your project. You can
achieve that by registering `@composer drupal:scaffold` as post-install and post-update command in your composer.json:

```json
"scripts": {
    "post-install-cmd": [
        "@composer drupal:scaffold",
        "..."
    ],
    "post-update-cmd": [
        "@composer drupal:scaffold",
        "..."
    ]
},
```

## How can I apply patches to downloaded modules?

The ```composer.json``` applies patches using the [composer-patches](https://github.com/cweagans/composer-patches) plugin.
To add a patch to drupal module foobar insert the patches section in the extra 
section of composer.json:

```json
"extra": {
    "patches": {
        "drupal/foobar": {
            "Patch description": "URL or local path to patch"
        }
    }
}
```


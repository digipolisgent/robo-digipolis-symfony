# Robo Digipolis Symfony

[![Latest Stable Version](https://poser.pugx.org/digipolisgent/robo-digipolis-symfony/v/stable)](https://packagist.org/packages/digipolisgent/robo-digipolis-symfony)
[![Latest Unstable Version](https://poser.pugx.org/digipolisgent/robo-digipolis-symfony/v/unstable)](https://packagist.org/packages/digipolisgent/robo-digipolis-symfony)
[![Total Downloads](https://poser.pugx.org/digipolisgent/robo-digipolis-symfony/downloads)](https://packagist.org/packages/digipolisgent/robo-digipolis-symfony)
[![License](https://poser.pugx.org/digipolisgent/robo-digipolis-symfony/license)](https://packagist.org/packages/digipolisgent/robo-digipolis-symfony)

[![Build Status](https://travis-ci.org/digipolisgent/robo-digipolis-symfony.svg?branch=develop)](https://travis-ci.org/digipolisgent/robo-digipolis-symfony)
[![Maintainability](https://api.codeclimate.com/v1/badges/dce687535f6482464538/maintainability)](https://codeclimate.com/github/digipolisgent/robo-digipolis-symfony/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/dce687535f6482464538/test_coverage)](https://codeclimate.com/github/digipolisgent/robo-digipolis-symfony/test_coverage)
[![PHP 7 ready](https://php7ready.timesplinter.ch/digipolisgent/robo-digipolis-symfony/develop/badge.svg)](https://travis-ci.org/digipolisgent/robo-digipolis-symfony)

Used by digipolis, serving as an example.

This package contains a RoboFileBase class that can be used in your own
RoboFile. All commands can be overwritten by overwriting the parent method.
This class also contains properties (`$fileBackupSubDirs` and `$console`) that
can be overwritten by your RoboFile.

## Versions
Use version 1.x for Symfony 2 or 3, use 2.x for Symfony 4.

## Properties

### `$filesBackupDirs`

An array of directory paths that will be added to the tar archive when creating
a backup of the files. These paths are relative to the `filesdir` path under the
`remote` key defined in your `properties.yml` file in your project root. If
there is no `properties.yml` file in your project root, the
`src/default.properties.yml` file in this package will be used. Defaults to
`['app', 'framework', 'logs']`.

### `$console`

The path to the symfony console executable, relative to your project root.
Defaults to `bin/console`. If the `bin/console` file is not found, it defaults
to `app/console`. If you want to modify this path, set it in your RoboFile's
constructor (don't forget to call the parent's constructor first).

## Example

```php
<?php

use DigipolisGent\Robo\Symfony\RoboFileBase;

class RoboFile extends RoboFileBase
{
    use \Robo\Task\Base\loadTasks;

    /**
     * {@inheritdoc}
     */
    public function __construct()
    {
        // Call our parent's constructor first.
        parent::__construct();
        $this->console = 'custom/path/to/console';
    }

    /**
     * @inheritdoc
     */
    public function digipolisDeploySymfony(
        array $arguments,
        $opts = [
            'app' => 'default',
            'worker' => null,
        ]
    ) {
        $collection = parent::digipolisDeploySymfony($arguments, $opts);
        $collection->taskExec('/usr/bin/custom-post-release-script.sh');
        return $collection;
    }
}

```

## Available commands

Following the example above, these commands will be available:

```bash
digipolis:backup-symfony           Create a backup of files (storage folder) and database.
digipolis:build-symfony            Build a Symfony site and package it.
digipolis:clean-dir                Partially clean directories.
digipolis:clear-op-cache           Command digipolis:database-backup.
digipolis:database-backup          Command digipolis:database-backup.
digipolis:database-restore         Command digipolis:database-restore.
digipolis:deploy-symfony           Build a Symfony site and push it to the servers.
digipolis:download-backup-symfony  Download a backup of files (storage folder) and database.
digipolis:init-symfony-remote      Install or update a Symfony remote site.
digipolis:install-symfony          Install the Symfony site in the current folder.
digipolis:package-project
digipolis:push-package             Command digipolis:push-package.
digipolis:restore-backup-symfony   Restore a backup of files (storage folder) and database.
digipolis:switch-previous          Switch the current release symlink to the previous release.
digipolis:sync-symfony             Sync the database and files between two Symfony sites.
digipolis:theme-clean
digipolis:theme-compile
digipolis:update-symfony           Executes database updates of the Symfony site in the current folder.
digipolis:upload-backup-symfony    Upload a backup of files (storage folder) and database to a server.
```

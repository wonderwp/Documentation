# WonderWp Documentation Component

## Working on the documentation locally

## Installing the documentation

Clone or fork the documentation repo from https://github.com/wonderwp/Documentation.

Once you've got the files locally, run : `composer install`.

Then you should be able to generate / serve the documentation locally with the two commands below. 

## Doc generation

This transforms your markup into HTML once.

`vendor/bin/daux generate --source=src --destination=doc --delete`

## Local doc preview

This serves a local preview you can access with your browser.

`vendor/bin/daux serve --source=src`


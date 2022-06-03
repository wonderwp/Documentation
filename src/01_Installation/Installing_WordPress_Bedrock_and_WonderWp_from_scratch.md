# Installing WordPress, Bedrock, and WonderWp from scratch (scripted)

If you'd like to start a new modern WordPress project rapidly, with 
- [Composer](https://getcomposer.org/) as a package manager, 
- [Bedrock](https://roots.io/bedrock/) as boilerplate,
- [WP-CLI](https://wp-cli.org/) as Command Line Tools,
- and [WonderWp](http://wonderwp.net/) as a framework,

Then you can follow the steps below : 

1. Create a new project:
   ```sh
   $ composer create-project roots/bedrock my-wonderwp-project && cd my-wonderwp-project 
   ```
   
2. Update environment variables in the `.env` file. Wrap values that may contain non-alphanumeric characters with quotes, or they may be incorrectly parsed.

- Database variables
    - `DB_NAME` - Database name
    - `DB_USER` - Database user
    - `DB_PASSWORD` - Database password
    - `DB_HOST` - Database host
    - Optionally, you can define `DATABASE_URL` for using a DSN instead of using the variables above (e.g. `mysql://user:password@127.0.0.1:3306/db_name`)
- `WP_ENV` - Set to environment (`development`, `staging`, `production`)
- `WP_HOME` - Full URL to WordPress home (https://example.com)
- `WP_SITEURL` - Full URL to WordPress including subdirectory (https://example.com/wp)
- `AUTH_KEY`, `SECURE_AUTH_KEY`, `LOGGED_IN_KEY`, `NONCE_KEY`, `AUTH_SALT`, `SECURE_AUTH_SALT`, `LOGGED_IN_SALT`, `NONCE_SALT`
    - Generate with [wp-cli-dotenv-command](https://github.com/aaemnnosttv/wp-cli-dotenv-command)
    - Generate with [our WordPress salts generator](https://roots.io/salts.html)

3. Install WP-CLI
   ```sh
   $ composer require wp-cli/wp-cli
   $ composer require wp-cli/wp-cli-bundle
   ```
   
4. Launch the WordPress install with your correct credentials via WP-CLI :
   ```sh
   $ vendor/bin/wp core install --url="yourlocalurl" --title="yoursitetitle" --admin_user="########" --admin_email="################"
   ```
       
5. Install WonderWp
   ```sh
   $ composer require wonderwp/wonderwp
   ```
   
6. Set the document root on your webserver to Bedrock's `web` folder: `/path/to/site/web/`
7. Access your WordPress site at `https://example.com/wp/wp-admin/` and its admin at `https://example.com/wp/wp-admin/`

With these 5 commands, your install should be ready to run with full modern development capabilities.

It's now time to [create your first plugin](../02_Creating_a_plugin/01_Getting_Started.md).

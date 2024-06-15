* goal
  * set up PostgreSQL / host Backstage data

## Prerequisites
* GNU-like build environment
  * Debian / Ubuntu
    * make
    * build-essential
  * MacOs
    * xcode-select --install
      * `xcode-select --version` check that it's installed
* Basic knowledge of `apt-get`, `psql` and `yarn`
* if host of backstage app != host of PostgreSQL -> ports available
  * 5432
  * 5433

## Steps
* [Install PostgreSQL](https://www.postgresql.org/download/) in your host
  * MacOs
    * `brew install postgresql` & `brew services start postgresql`
  * Others
    * TODO:
* Connect to PostgreSQL
  * Linux - `sudo -u postgres psql`
  * MacOs - `psql postgres`
  * Others
    * TODO:
* Create a super with password
  * `CREATE USER newadmin WITH SUPERUSER CREATEDB CREATEROLE LOGIN PASSWORD 'newpassword';`
    * `\du`  -- check it has been created -- 
* At backstage path (Check in '../examples/my-backstage-app/') `yarn --cwd packages/backend add pg`
  * Configure backstage `pg` client
* Configure Backstage 'pg' client
  * ways to check the 'POSTGRES_HOST' & 'POSTGRES_PORT'
    * connect to PostgreSQL & `SHOW config_file;`
      * get the path of the configfile
    * `cat PathOfTheConfigFilePreviouslyGet`
  * `export POSTGRES_HOST=localhost`, `export POSTGRES_PORT=5432`, `export POSTGRES_USER=newadmin`, `export POSTGRES_USER=newadmin`
* `yarn dev`

## Notes
* Check '/examples/my-backstage-app'

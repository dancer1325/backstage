## DDBB
* [Knex](https://knexjs.org)
  * := library
    * uses
      * backstage BE & built-in plugins
    * allows
      * separated logical DDBB / plugin
    * supports multitude of DDBB
* Supported by Backstage
  * SQLite
    * uses
      * in-memory mock/test DDBB
  * PostgreSQL
    * recommended to use in production

## Cache
* Uses
  * Backstage BE & built-in plugins
    * via [Keyv](https://github.com/jaredwray/keyv)
* Supported by Backstage
  * memory
    * uses
      * local testing
      ```
       backend:
         cache:
           store: memory
      ```
  * memcache
      ```
      backend:
        cache:
          store: memcache
          connection: user:pass@cache.example.com:11211
      ```
  * Redis
      ```
      backend:
        cache:
          store: redis
          connection: redis://user:pass@cache.example.com:6379
          useRedisSets: true
      ```


## Containerization
* TODO: What example above?

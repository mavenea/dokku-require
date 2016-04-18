(this isn't polished, but should work as describe - tested on dokku 4.*)

# Dokku Require

A plugin which allow you to specify plugins and volumes
required by your app.

## Requirements

* dokku 0.4.0+


## Installation

```
# on 0.4.x
dokku plugin:install https://github.com/crisward/dokku-require.git require
```

## Usage

In the root of your app, you need to add an app.json file [as specificed in this heroku spec](https://devcenter.heroku.com/articles/app-json-schema#schema-reference)

With the addition of a dokku section (see below). 

```json
{
  "name": "my website",
  "description": "My website",
  "keywords": [
    "productivity",
    "HTML5"
  ],
  "website": "https://mywebsite.com/",
  "dokku":{
    "plugins":[
      "mariadb","redis"
    ],
    "volumes":[
      {"host":"/var/lib/dokku/data/storage","app":"/storage"}
    ]
  }
}
```

The `"dokku":{"plugins":[]}` array can contain a list of plugin names, which 
by default will be created with the app name and linked to your app.
This will only work the official plugins which have `create` and `link` methods.
ie.

* [CouchDB (beta)](https://github.com/dokku/dokku-couchdb)                     
* [Elasticsearch (beta)](https://github.com/dokku/dokku-elasticsearch-plugin)  
* [MariaDB (beta)](https://github.com/dokku/dokku-mariadb-plugin)              
* [Memcached (beta)](https://github.com/dokku/dokku-memcached-plugin)          
* [Mongo (beta)](https://github.com/dokku/dokku-mongo-plugin)                  
* [MySQL (beta)](https://github.com/dokku/dokku-mysql-plugin)                  
* [Nats (beta)](https://github.com/dokku/dokku-nats)                           
* [Postgres (beta)](https://github.com/dokku/dokku-postgres-plugin)            
* [RabbitMQ (beta)](https://github.com/dokku/dokku-rabbitmq-plugin)            
* [Redis (beta)](https://github.com/dokku/dokku-redis-plugin)                  
* [RethinkDB (beta)](https://github.com/dokku/dokku-rethinkdb-plugin)   

If you need more control over the creation process or you'd like to use
a non-official plugins the below syntax can be used. Each command in the
array will be run in sequence.

```json
  "dokku":{
    "plugins":[
      {
        "name":"mariadb",
        "commands":["mariadb:create mydb","mariadb:link mydb $APP"]
      }
    ]
  }
```

`$APP` will be replaced with your app name,

## Todo.

* ~~Check host folder exists before creating volume, then create~~
* ~~Check if plugin is installed, then show error if it isn't~~
* Create demo github repo that can be used with dokku clone and dokku require to build an app (wordpress, sentry?)
* general code tidy, peer review etc

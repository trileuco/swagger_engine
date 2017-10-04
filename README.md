# SwaggerEngine

Include [swagger-ui](https://github.com/swagger-api/swagger-ui) as rails engine.

## Swagger specifications

https://github.com/swagger-api/swagger-spec/blob/master/versions/2.0.md

## Install

Add to Gemfile

```gem 'swagger_engine'```

Add to your config/routes.rb

```mount SwaggerEngine::Engine, at: "/api-docs"```

### Protect your route

#### Devise

```
authenticate :user do
  mount SwaggerEngine::Engine, at: "/api-docs"
end
```

or

```
authenticate :user, lambda { |u| u.admin? } do
  mount SwaggerEngine::Engine, at: "/api-docs"
end
```

#### Basic http auth

Set username and password in `config/initializers/swagger_engine.rb`:

```
SwaggerEngine.configure do |config|
  config.admin_username = ENV['ADMIN_USERNAME']
  config.admin_password = ENV['ADMIN_PASSWORD']
end
```

## Configure

### Json files

Set the path of your json files in a initializer:

```
#config/initializers/swagger_engine.rb

SwaggerEngine.configure do |config|
  config.json_files = {
    v1: "lib/swagger/swagger_v1.json",
    v2: "lib/swagger/swagger_v2.json"
  }
end
```
`lib/swagger/` is a good place to place them..

If you want create several different api specifications on the same application, you can mount the engine several times and
configure the json files separated in entries on the hash using as hash key the root_path where the engine is mounted.

```
mount SwaggerEngine::Engine, at: "/admin_api/api-docs", as: "admin_api"
mount SwaggerEngine::Engine, at: "/public_api/api-docs", as: "public_api"
```

```
#config/initializers/swagger_engine.rb

SwaggerEngine.configure do |config|
  config.json_files = {
    '/admin_api/api-docs/' => {
      v1: 'lib/swagger/admin_api/v1.json',
      v2: 'lib/swagger/admin_api/v2.json'
    },
    '/public_api/api-docs/' => {
      v1: 'lib/swagger/public_api/v1.json',
      v2: 'lib/swagger/public_api/v2.json'
    }
  }
end
```

### Edit your json files

Use [Swagger editor](https://github.com/swagger-api/swagger-editor).

## License

This project rocks and uses MIT-LICENSE.

## Developed in collaboration with

[Rawfish](http://rawfishindustries.com)

[![Rawfish Logo](http://rawfishindustries.com/wp-content/uploads/2015/03/logo_rawfish_WEB.jpg)](http://rawfishindustries.com)

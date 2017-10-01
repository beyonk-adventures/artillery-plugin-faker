# Artillery Faker

Makes [faker.js](https://github.com/Marak/faker.js) available for [Artillery](https://artillery.io/) loadtest configurations.

## Install

```sh
$ npm install artillery-plugin-faker
```

## Usage

Add the plugin to your loadtest configuration:

```yaml
config:
  plugins:
    faker:
      locale: 'en'
      defaultSize: 250
```

There are two options available: `locale` (default: `en`) sets the locale of *faker.js* and `defaultSize` (default: `250`) set the default size of an array in the variables section of your config (see below).

This plugin can be used in two different ways:

### Variables Section

```yaml
config:
  plugins:
    faker: {}
  variables:
    title: '$faker.name.title'
    firstName: [ '$faker.name.firstName' ]
    lastName: [ '$faker.name.lastName', 100 ]
  scenarios:
    - flow:
      - get:
          url: "/search?q={{ firstName }}"
```

1. Using a string prefixed with `$faker.` will result in a random value of the indicated *faker.js* function.
2. Using an array including a string to a valid *faker.js* function prefixed with `$faker.` will generate an array of random values with the size of the `defaultSize` value.
3. Using an integer as the second array item will generate an array of random values of this size.

> For a complete list of available *faker.js* functions, have a look at the [faker.js documentation](https://github.com/Marak/faker.js#api-methods). Please note that no parameters can be passed to the *faker.js* functions at this time.


### Using the *faker.js* functions directly

Use the `$faker` function with a valid path to a *faker.js* function to invoke this function directly:

```yaml
config:
  plugins:
    faker: {}
scenarios:
  - flow:
    - get:
        url: "/search?q={{ $faker('name.firstName') }}"
```

Use the `$fake` function to make use of *faker.js* [templating feature](https://github.com/Marak/faker.js#fakerfake):

```yaml
config:
  plugins:
    faker: {}
scenarios:
  - flow:
    - get:
        url: "/search?q={{ $fake('{{name.firstName}} {{name.lastName}}, {{name.suffix}} ') }}"
```

> Have a look at the `example.yml` file for a fully working example.



# friendly-lite-server - Self Hosted Captcha Server, compatible with Friendly Captcha

[**Friendly Captcha**](https://friendlycaptcha.com) offers a privacy aware captcha service. This repository provides a lite server version of the service with some basic features for puzzle and verification on your own machine. The Friendly Captcha service normally works by serving challenges with the difficulty based on the likelihood of being a real human user, as well as being highly available. This implementation is much simpler but will work for small hobby projects.

This distribution is licensed under a non-commercial source available license, which means you can run this server yourself for non-commercial or internal projects. 

If you need more advanced security features and reliability or want to support our work, we highly recommend to [**subscribe**](https://friendlycaptcha.com/signup)  to the Friendly Captcha service and/or [**sponsor** us on GitHub](https://github.com/sponsors/FriendlyCaptcha).

## Requirements

* PHP 7.4 or higher
* sodium support
* apcu support

## Installation

You need a web server running PHP 7.4 or later.

1. Install the public folder to the your document root.
2. Copy and adapt Env.template.php to Env.php in classes folder.
3. Change the friendly captcha widgets endpoint to user your server
4. In your backend configuration, use the your own server endpoint and

## Endpoints

Instead of `https://api.friendlycaptcha.com/api/v1/siteverify` use `https://yourserver/siteverify.php`.
Instead of `https://(eu-)api.friendlycaptcha.eu/api/v1/puzzle"` use `https://yourserver/puzzle.php`. 

## Running as Docker Container

You can also run the server as a docker container. Use the following command to build the container:

```bash
docker build -t friendly-lite-server .
```

Then run the container with the following command:

```bash
docker run -d -p 80:80 -e "SECRET=FILL-YOUR-SECRET-HERE" -e "API_KEY=FILL-YOUR-API-KEY-HERE" friendly-lite-server
```

Alternatively, use the following `docker-compose` commands:

```bash
docker-compose build
docker-compose up -d
```

When using Docker, the following environment variables are available:

* `SECRET`: Your secret 
* `API_KEY`: Your api key
* `LOG_FILE`: Default is `php://stdout`.
* `SCALING_TTL_SECONDS`: Default is `1800`
* `EXPIRY_TIMES_5_MINUTES`: Default is `12`

## What works

* Check of signature
* Check of puzzles
* Check of timestamps
* Replay checks
* Basic difficulty scaling

## Executing tests

To run the included [PHPUnit](https://phpunit.de/index.html) tests, make sure you have [composer](https://getcomposer.org/) installed. Then run:

```bash
composer install
./vendor/bin/phpunit
```

If you want to execute the [PHPUnit code coverage reporting](https://docs.phpunit.de/en/9.6/textui.html#the-command-line-test-runner), make sure you have [XDebug](https://xdebug.org/docs/install) installed and activated in your PHP CLI ini. You could for example install it via `pecl install xdebug` and then make sure your `/etc/php/x.xx/cli/php.ini` contains a line like `zend_extension=xdebug.so`. Then, to for example create an HTML coverage report, run:

```bash
XDEBUG_MODE=coverage ./vendor/bin/phpunit --coverage-html coverage
```

Your report will be saved to the `coverage` folder in this example.

## License 

This software is [**fair-code**](http://faircode.io) distributed under [**Apache 2.0 with Commons Attribution Clause**](https://github.com/FriendlyCaptcha/friendly-lite-server/blob/main/LICENSE) license.

## Troubleshooting 

### The widget shows an error when loading the puzzle 

* Check your server log for errors 
* Open /puzzle.php manually and see if there are any errors displayed
* Is LibSodium available?

### Captcha is not validated, even generated by the widget itself

* This should not happen, maybe file an issue including the server logs

### Invalid Captchas are accepted (for example form submitted before captcha was solved) 

* Friendly Captcha recommends to accept Captcha solutions on server errors, so also here check for server errors
* Is APCU and LibSodium available?


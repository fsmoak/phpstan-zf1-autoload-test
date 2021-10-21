# PHPStan with ZendFramework 1 Fallback Autoloader
PHPStan Version: ```0.12.99```  

This demonstrates an issue when using an autoloader (like zend framework 1 fallback) which will
include files that do not exist: `@include "<non-existant-path>";`

```PHPStan\Reflection\BetterReflection\SourceLocator\FileReadTrapStreamWrapper::stream_open```
will capture this attempt of opening a file and register it as a success.

https://www.php.net/manual/de/streamwrapper.stream-open.php  
This is because ```streamWrapper::stream_open``` ```$path``` "Specifies the URL that was passed to the original function." which is non-existent.

Only ```streamWrapper::stream_open``` ```$opened_path``` "If the path is opened successfully" will contain only valid paths and ```null``` if not.

Discussion: https://github.com/phpstan/phpstan/discussions/5074

Pull Request: https://github.com/phpstan/phpstan-src/pull/722

## Install
```bash
docker run -it --rm -v $PWD:/var/www -w /var/www composer install
```

## Run
```bash
docker run -it --rm -v $PWD:/var/www -w /var/www php:7.4-cli vendor/bin/phpstan
```

### Output:
```
❯ docker run -it --rm -v $PWD:/var/www -w /var/www php:7.4-cli vendor/bin/phpstan
Note: Using configuration file /var/www/phpstan.neon.
 1/1 [▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓] 100%
 
 ------ ----------------------------------------------
 Line   Test.php
 ------ ----------------------------------------------
 Could not read file: Application/boolean.php
 ------ ----------------------------------------------
 
                                                                                                                         
 [ERROR] Found 1 error
```

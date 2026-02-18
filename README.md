# Monolog

[![Latest Stable Version](https://poser.pugx.org/monolog/monolog/v/stable)](https://packagist.org/packages/monolog/monolog)
[![Total Downloads](https://poser.pugx.org/monolog/monolog/downloads)](https://packagist.org/packages/monolog/monolog)
[![License](https://poser.pugx.org/monolog/monolog/license)](LICENSE)

Monolog is a comprehensive logging library for PHP that implements the [PSR-3](https://www.php-fig.org/psr/psr-3/) logging interface. It sends your logs to files, sockets, inboxes, databases, and various web services.

## Features

- **PSR-3 Compliant**: Implements the PSR-3 logging interface for broad compatibility
- **Multiple Handlers**: Support for diverse output destinations:
  - Files and Rotating Files
  - Email and Messaging services
  - Databases (MongoDB, CouchDB, DynamoDB)
  - Search services (Elasticsearch, Elastica)
  - Message queues (AMQP)
  - Cloud services (AWS, Google Cloud Logging)
  - Monitoring services (Rollbar, Telegram, Slack, etc.)
- **Flexible Formatting**: Multiple formatters for different output formats (JSON, Logstash, Syslog, HTML, etc.)
- **Processing Pipeline**: Chain processors to add contextual information to log records
- **Error Handling**: Built-in error handler that converts PHP errors to logs
- **Signal Handling**: Advanced signal handling for graceful application termination
- **Utility Functions**: Helper functions for common logging tasks

## Requirements

- **PHP >= 8.1**: Requires PHP 8.1 or higher
- **PSR Log**: Requires `psr/log` ^2.0 or ^3.0

## Installation

Install Monolog using Composer:

```bash
composer require monolog/monolog
```

## Quick Start

### Basic Usage

```php
<?php
use Monolog\Logger;
use Monolog\Handler\StreamHandler;

// Create a logger
$log = new Logger('my_logger');

// Create a handler
$log->pushHandler(new StreamHandler('app.log', Logger::DEBUG));

// Log messages
$log->info('Informational message');
$log->error('Error message', ['key' => 'value']);
$log->debug('Debug message with extra data', [
    'user_id' => 123,
    'action' => 'login'
]);
```

### Using Different Handlers

Monolog supports multiple output destinations simultaneously:

```php
<?php
use Monolog\Logger;
use Monolog\Handler\StreamHandler;
use Monolog\Handler\RotatingFileHandler;

$log = new Logger('my_logger');

// Log to file with rotation
$log->pushHandler(new RotatingFileHandler('app.log', 10, Logger::DEBUG));

// Log errors to separate file
$log->pushHandler(new RotatingFileHandler('errors.log', 5, Logger::ERROR));

// Also log to stdout
$log->pushHandler(new StreamHandler('php://stdout', Logger::INFO));
```

### Adding Formatters

Customize log output format with formatters:

```php
<?php
use Monolog\Logger;
use Monolog\Handler\StreamHandler;
use Monolog\Formatter\LineFormatter;

$handler = new StreamHandler('app.log');
$formatter = new LineFormatter(
    "[%datetime%] %channel%.%level_name%: %message% %context% %extra%\n"
);
$handler->setFormatter($formatter);

$log = new Logger('my_logger');
$log->pushHandler($handler);
```

### Using Processors

Add contextual data to every log record:

```php
<?php
use Monolog\Logger;
use Monolog\Handler\StreamHandler;
use Monolog\Processor\UidProcessor;
use Monolog\Processor\MemoryPeakUsageProcessor;

$log = new Logger('my_logger');
$log->pushHandler(new StreamHandler('app.log'));

// Add a unique ID to all logs
$log->pushProcessor(new UidProcessor());

// Add memory usage information
$log->pushProcessor(new MemoryPeakUsageProcessor());
```

## Documentation

For comprehensive documentation and advanced usage, see:

- [Usage Guide](doc/01-usage.md) - Detailed documentation on how to use Monolog
- [Handlers, Formatters & Processors](doc/02-handlers-formatters-processors.md) - Reference for all components
- [Utilities](doc/03-utilities.md) - Helper functions and utilities
- [Extending Monolog](doc/04-extending.md) - Creating custom handlers, formatters, and processors
- [Message Structure](doc/message-structure.md) - Understanding log record structure
- [Sockets](doc/sockets.md) - Socket-based communication

## Logging Levels

Monolog uses the PSR-3 logging levels:

- **DEBUG** (100): Detailed debug information
- **INFO** (200): Informational messages
- **NOTICE** (250): Normal but significant events
- **WARNING** (300): Warning messages
- **ERROR** (400): Error conditions
- **CRITICAL** (500): Critical conditions
- **ALERT** (550): Action must be taken immediately
- **EMERGENCY** (600): System is unusable

## Common Use Cases

### Log to File with Rotation

```php
$log->pushHandler(new RotatingFileHandler('app.log', 30, Logger::DEBUG));
```

### Send Errors via Email

```php
use Monolog\Handler\NativeMailerHandler;

$handler = new NativeMailerHandler(
    'errors@example.com',
    'Application Error Report',
    'from@example.com'
);
$handler->setLevel(Logger::ERROR);
$log->pushHandler($handler);
```

### Send to Elasticsearch

```php
use Monolog\Handler\ElasticsearchHandler;

$handler = new ElasticsearchHandler($elasticClient);
$log->pushHandler($handler);
```

### Error Handling

Convert all PHP errors to logs:

```php
use Monolog\ErrorHandler;

$log = new Logger('my_app');
$log->pushHandler(new StreamHandler('app.log'));

ErrorHandler::register($log);
```

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to contribute to Monolog.

To run tests:

```bash
vendor/bin/phpunit
```

## License

Monolog is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Credits

Monolog was created by Jordi Boggiano. See the [AUTHORS](CHANGELOG.md) file for a list of all contributors.

## Support

- **GitHub Issues**: Report bugs and request features at https://github.com/Seldaek/monolog/issues
- **GitHub Discussions**: General questions and discussions at https://github.com/Seldaek/monolog/discussions
- **Documentation**: Full documentation available in the [doc](doc/) directory

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for a detailed list of changes in each version.

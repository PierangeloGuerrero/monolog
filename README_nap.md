# Monolog

[![Latest Stable Version](https://poser.pugx.org/monolog/monolog/v/stable)](https://packagist.org/packages/monolog/monolog)
[![Total Downloads](https://poser.pugx.org/monolog/monolog/downloads)](https://packagist.org/packages/monolog/monolog)
[![License](https://poser.pugx.org/monolog/monolog/license)](LICENSE)

Monolog è 'na libreria cumpleta 'e logging pe PHP ca ve mette a disposizione l'interfaccia [PSR-3](https://www.php-fig.org/psr/psr-3/). Manna 'e logs vvostri a file, socket, email, database e varie pile web.

## Caracteristiche

- **Conforme a PSR-3**: Attuà l'interfaccia 'e logging PSR-3 pe una compatibilità larga
- **Multiplice Handler**: Supporta diverse destinazioni 'e output:
  - File e File Ruotanti
  - Servizi 'e Email e Messaggistica
  - Database (MongoDB, CouchDB, DynamoDB)
  - Servizi 'e Ricerca (Elasticsearch, Elastica)
  - Code 'e Messaggio (AMQP)
  - Servizi Cloud (AWS, Google Cloud Logging)
  - Servizi 'e Monitoraggio (Rollbar, Telegram, Slack, etc.)
- **Formattazione Flessibile**: Multiplici formattatori pe diversi formati 'e output (JSON, Logstash, Syslog, HTML, etc.)
- **Pipeline 'e Processamento**: Catenà i processatori pe aggie' informazioni contestuali ai record 'e log
- **Gestione 'e Errori**: Gestione 'e errori integrata ca converte li errori PHP a log
- **Gestione 'e Signali**: Gestione avanzata 'e signali pe na chiusura accurata dell'applicazione
- **Funzioni Utilità**: Funzioni d'aiuto pe i compiti 'e logging comuni

## Requisiti

- **PHP >= 8.1**: Richiede PHP 8.1 o più recente
- **PSR Log**: Richiede `psr/log` ^2.0 o ^3.0

## Installazione

Installate Monolog usando Composer:

```bash
composer require monolog/monolog
```

## Cominciate Abbasso

### Uso Basico

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

### Usanno Handler Diversi

Monolog supporta multiple destinazioni 'e output contemporaneamente:

```php
<?php
use Monolog\Logger;
use Monolog\Handler\StreamHandler;
use Monolog\Handler\RotatingFileHandler;

$log = new Logger('my_logger');

// Scriv a file cu rotazione
$log->pushHandler(new RotatingFileHandler('app.log', 10, Logger::DEBUG));

// Scriv li errori a file separato
$log->pushHandler(new RotatingFileHandler('errors.log', 5, Logger::ERROR));

// Anche scriv a stdout
$log->pushHandler(new StreamHandler('php://stdout', Logger::INFO));
```

### Aggiungite Formattatori

Personalizzate 'o formato 'e output 'e log cu formattatori:

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

### Usanno Processatori

Aggiungite dati contestuali a ogni record 'e log:

```php
<?php
use Monolog\Logger;
use Monolog\Handler\StreamHandler;
use Monolog\Processor\UidProcessor;
use Monolog\Processor\MemoryPeakUsageProcessor;

$log = new Logger('my_logger');
$log->pushHandler(new StreamHandler('app.log'));

// Aggiungite n'ID unico a tutti li log
$log->pushProcessor(new UidProcessor());

// Aggiungite informazioni sull'utilizzo 'e memoria
$log->pushProcessor(new MemoryPeakUsageProcessor());
```

## Documentazione

Pe documentazione completa e uso avanzato, date nu guardo a:

- [Guida d'Uso](doc/01-usage.md) - Documentazione dettagliata sull'uso 'e Monolog
- [Handler, Formattatori & Processatori](doc/02-handlers-formatters-processors.md) - Riferimento pe tutti li componenti
- [Utilità](doc/03-utilities.md) - Funzioni d'aiuto e utilità
- [Estensione 'e Monolog](doc/04-extending.md) - Creazione 'e handler, formattatori e processatori personalizzati
- [Struttura 'e Messaggio](doc/message-structure.md) - Comprensione della struttura 'e record 'e log
- [Socket](doc/sockets.md) - Comunicazione basata su socket

## Livelli 'e Logging

Monolog usa li livelli 'e logging PSR-3:

- **DEBUG** (100): Informazioni 'e debug dettagliate
- **INFO** (200): Messaggi informativi
- **NOTICE** (250): Eventi normali ma significativi
- **WARNING** (300): Messaggi 'e avvertimento
- **ERROR** (400): Condizioni 'e errore
- **CRITICAL** (500): Condizioni critiche
- **ALERT** (550): N'azione deve essere presa immediatamente
- **EMERGENCY** (600): 'O sistema è inutilizzabile

## Casi d'Uso Comuni

### Scriv a File cu Rotazione

```php
$log->pushHandler(new RotatingFileHandler('app.log', 30, Logger::DEBUG));
```

### Mandà Errori via Email

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

### Mandà a Elasticsearch

```php
use Monolog\Handler\ElasticsearchHandler;

$handler = new ElasticsearchHandler($elasticClient);
$log->pushHandler($handler);
```

### Gestione 'e Errori

Convertite tutti li errori PHP a log:

```php
use Monolog\ErrorHandler;

$log = new Logger('my_app');
$log->pushHandler(new StreamHandler('app.log'));

ErrorHandler::register($log);
```

## Contribute

Li contributi songo benvenuti! Date nu guardo a [CONTRIBUTING.md](CONTRIBUTING.md) pe i dettagli sull'assaje pi' contribuire a Monolog.

Pe faie li test:

```bash
vendor/bin/phpunit
```

## Licenza

Monolog è licensiato sotta 'a Licenza MIT. Date nu guardo a 'o file [LICENSE](LICENSE) pe i dettagli.

## Crediti

Monolog è stato creato da Jordi Boggiano. Date nu guardo a 'o file [AUTHORS](CHANGELOG.md) pe 'a lista 'e tutti li contributori.

## Supporto

- **GitHub Issues**: Segnalate i bug e richiedete feature a https://github.com/Seldaek/monolog/issues
- **GitHub Discussions**: Domande generali e discussioni a https://github.com/Seldaek/monolog/discussions
- **Documentation**: Documentazione completa disponibile nella cartella [doc](doc/)

## Changelog

Date nu guardo a [CHANGELOG.md](CHANGELOG.md) pe 'a lista dettagliata 'e cambiamenti in ogni versione.

<div align="center">
    <img alt="Schranz Search Logo with a Seal on it with a magnifying glass" src="https://avatars.githubusercontent.com/u/120221538?s=400&v=5" width="200" height="200">
</div>

<h1 align="center">Schranz Search SEAL <br /> Yii Integration</h1>

<br />
<br />

Integration of the Schranz Search — Search Engine Abstraction Layer (SEAL) into Yii.

> **Note**:
> This is part of the `schranz-search/schranz-search` project create issues in the [main repository](https://github.com/schranz-search/schranz-search).

> **Note**:
> This project is heavily under development and any feedback is greatly appreciated.

## Installation

Use [composer](https://getcomposer.org/) for install the package:

```bash
composer require schranz-search/yii-module
```

Also install one of the listed adapters.

### List of adapters

The following adapters are available:

 - [MemoryAdapter](../../packages/seal-memory-adapter): `schranz-search/seal-memory-adapter`
 - [ElasticsearchAdapter](../../packages/seal-elasticsearch-adapter): `schranz-search/seal-elasticsearch-adapter`
 - [OpensearchAdapter](../../packages/seal-opensearch-adapter): `schranz-search/seal-opensearch-adapter`
 - [MeilisearchAdapter](../../packages/seal-meilisearch-adapter): `schranz-search/seal-meilisearch-adapter`
 - [AlgoliaAdapter](../../packages/seal-algolia-adapter): `schranz-search/seal-algolia-adapter`
 - [SolrAdapter](../../packages/seal-solr-adapter): `schranz-search/seal-solr-adapter`
 - [RediSearchAdapter](../../packages/seal-redisearch-adapter): `schranz-search/seal-redisearch-adapter`
 - [TypesenseAdapter](../../packages/seal-typesense-adapter): `schranz-search/seal-typesense-adapter`
 - ... more coming soon

Additional Wrapper adapters:

 - [ReadWriteAdapter](../../packages/seal-read-write-adapter)
 - [MultiAdapter](../../packages/seal-multi-adapter)

Creating your own adapter? Add the [`seal-php-adapter`](https://github.com/topics/seal-php-adapter) Topic to your Github Repository.

## Usage

The following code shows how to configure the package:

```php
<?php

// config/common/params.php

return [
    // ...
    'schranz-search/yii-module' => [
        'schemas' => [
            'app' => [
                'dir' => 'config/schemas',
            ],
        ],
        'engines' => [
            'default' => [
                'adapter' => 'meilisearch://127.0.0.1:7700',
            ],
        ],
        'index_name_prefix' => '',
        'reindex_providers' => [
            \App\Search\BlogReindexProvider::class,
        ],
    ],
];
```

A more complex configuration can be here found:

```php
<?php

// config/common/params.php

return [
    // ...
    'schranz-search/yii-module' => [
        'schemas' => [
            'app' => [
                'dir' => 'config/schemas/app',
                'engine' => 'meilisearch',
            ],
            'other' => [
                'dir' => 'config/schemas/other',
                'engine' => 'algolia',
            ],
        ],
        'engines' => [
            'algolia' => [
                'adapter' => 'algolia://' . $_ENV['ALGOLIA_APPLICATION_ID'] . ':' . $_ENV['ALGOLIA_ADMIN_API_KEY'],
            ],
            'elasticsearch' => [
                'adapter' => 'elasticsearch://127.0.0.1:9200',
            ],
            'meilisearch' => [
                'adapter' => 'meilisearch://127.0.0.1:7700',
            ],
            'memory' => [
                'adapter' => 'memory://',
            ],
            'opensearch' => [
                'adapter' => 'opensearch://127.0.0.1:9200',
            ],
            'redisearch' => [
                'adapter' => 'redis://supersecure@127.0.0.1:6379',
            ],
            'solr' => [
                'adapter' => 'solr://127.0.0.1:8983',
            ],
            'typesense' => [
                'adapter' => 'typesense://S3CR3T@127.0.0.1:8108',
            ],
            
            // ...
            'multi' => [
                'adapter' => 'multi://elasticsearch?adapters[]=opensearch',
            ],
            'read-write' => [
                'adapter' => 'read-write://elasticsearch?write=multi',
            ],
        ],
    ],
];
```

The default engine is available as `Engine`:

```php
class Some {
    public function __construct(
        private readonly \Schranz\Search\SEAL\EngineInterface $engine,
    ) {
    }
}
```

Multiple engines can be accessed via the `EngineRegistry`:

```php
class Some {
    private Engine $engine;

    public function __construct(
        private readonly \Schranz\Search\SEAL\EngineRegistry $engineRegistry,
    ) {
        $this->engine = $this->engineRegistry->getEngine('algolia');
    }
}
```

How to create a `Schema` file and use your `Engine` can be found [SEAL Documentation](../../README.md#usage).

### Commands

The package provides the following commands:

**Create configured indexes**

```bash
./yii schranz:search:index-create
```

**Drop configured indexes**

```bash
./yii schranz:search:index-drop
```

## Authors

- [Alexander Schranz](https://github.com/alexander-schranz/)
- [The Community Contributors](https://github.com/schranz-search/schranz-search/graphs/contributors)

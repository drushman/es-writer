ES-Writer [![build status](https://code.go1.com.au/fn/es-writer/badges/master/build.svg)](https://code.go1.com.au/fn/es-writer/commits/master)
====

## Problems

It's every easy to have conflict when we have multiple services writing data into same Elastic Search server.

To avoid this problems, the service shoudl publish message to a certain instead of writing to ES direclty. So that we can have ES-Writer, a single actor that connect with Elastic Search.

By this convention, the services doesn't need to know credentials of Elastic Search server.

It's also easy to create cluster Elastic Search servers without magic.

## Usage

### Queueing

To write data to ES server, your service publishes messages to `GO1` RabbitMQ with:

- **Exchange:** `$exchange`
- **Key:** `$routingKey`
- **Payload:** `OBJECT`
    - Examples:
        - fixtures/indices/indices-create.json
        - fixtures/indices/indices-drop.json
        - fixtures/portal/portal-index.json
        - fixtures/portal/portal-update.json
        - fixtures/portal/portal-update-by-query.json
        - fixtures/portal/portal-delete.json
        - fixtures/portal/portal-delete-by-query.json

Ref https://www.elastic.co/guide/en/elasticsearch/reference/5.2/docs-bulk.html

### Consuming

    /path/to/es-writer
        -url             RABBITMQ_URL<STRING=amqp://go1:go1@127.0.0.1:5672/>
        -kind            RABBITMQ_KIND<STRING=topic>
        -exchange        RABBITMQ_EXCHANGE<STRING=events>
        -routing-key     RABBITMQ_ROUTING_KEY<STRING=es.writer.go1>
        -prefetch-count  RABBITMQ_PREFETCH_COUNT<INT=50>
        -es-url          ELASTIC_SEARCH_URL<STRING=http://127.0.0.1:9200/?sniff=false>

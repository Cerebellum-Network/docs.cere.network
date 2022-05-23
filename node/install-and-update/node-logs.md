# Node logs

**In this guide you will learn how to:**

* How to check application logs
* How to change the log level
* How to collect logs

## Check application logs

You can check logs by running the following command (more `logs` command parameters can be found [here](https://docs.docker.com/compose/reference/logs/)):

```
docker-compose logs -f --tail=100 add_validation_node_custom
```

## Change log level

By default, LOG\_LEVEL `debug` is enabled. To change it, please, update correspond variable in the [Network .env file](node-logs.md#check-application-logs). The following LOG\_LEVELS are supporting:

1. error
2. warn
3. info
4. debug
5. trace

## Collect logs

In order to collect logs you can use the following command:

*   Collect application logs: \\

    `docker-compose logs add_validation_node_custom > app_logs.txt`
*   Collect system logs: \\

    `dmesg > system_logs.txt`

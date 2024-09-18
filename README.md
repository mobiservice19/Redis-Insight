# Redis Insight
_Основано на [redisinsight](https://github.com/RedisInsight/RedisInsight)_

### HELM CHART FOR USE TO KUBERNETES

### Sources
* https://github.com/RedisInsight/RedisInsight
* https://hub.docker.com/r/redis/redisinsight
* https://github.com/mobiservice19/Redis-Insight

## Install HELM Chart RedisInsight

#### Add repo RedisInsight
```helm
helm repo add redisinsight https://raw.githubusercontent.com/mobiservice19/Redis-Insight/main/charts
```

#### HELM REPO UPDATE
```helm
helm repo update
```
#### HELM SEARCH
```helm
helm search repo | grep redisinsight
```

#### Pull default values from HELM Chart redisinsight
```helm
helm show values redisinsight/redisinsight > values.yaml
```

#### Install HELM Chart redisinsight
```helm
helm upgrade redisinsight redisinsight/redisinsight \
    --install \
    --namespace redisinsight \
    --create-namespace \
    --values values.yaml \
    --wait \
    --atomic
```


### Add Secret for BasicAuth
```bash
kubectl -n redisinsight create secret generic redisinsight-basic-auth-secret \
    --from-literal=auth='<your-basic-auth>'
```

### Add Secret for connection to Redis (use custom secret to extraEnv)
```bash
kubectl -n redisinsight create secret generic redisinsight-secret \
    --from-literal=RI_APP_PORT='<your-RI_APP_PORT>' \
    --from-literal=RI_APP_HOST='<your-RI_APP_HOST>' \
    --from-literal=RI_SERVER_TLS_KEY='<your-RI_SERVER_TLS_KEY>' \
    --from-literal=RI_SERVER_TLS_CERT='<your-RI_SERVER_TLS_CERT>' \
    --from-literal=RI_ENCRYPTION_KEY='<your-RI_ENCRYPTION_KEY>' \
    --from-literal=RI_LOG_LEVEL='<your-RI_LOG_LEVEL>' \
    --from-literal=RI_FILES_LOGGER='<your-RI_FILES_LOGGER>' \
    --from-literal=RI_STDOUT_LOGGER='<your-RI_STDOUT_LOGGER>' \
    --from-literal=RI_PROXY_PATH='<your-RI_PROXY_PATH>'
```

## Configuration Redis Insight Environment

The following environment variables can be set to configure Redis Insight:
You can configure Redis Insight with the following environment variables.

| Variable | Purpose | Default | Additional info |
| ------ | ------ | ------ | ------  |
| RI_APP_PORT | The port that Redis Insight listens on | 5540 | See Express Documentation⁠ |
| RI_APP_HOST | The host that Redis Insight connects to | 0.0.0.0 | See Express Documentation⁠ |
| RI_SERVER_TLS_KEY | Private key for HTTPS | n/a | Private key in PEM format⁠. Can be a path to a file or a string in PEM format.
| RI_SERVER_TLS_CERT | Certificate for supplied private key | n/a | Public certificate in PEM format⁠. Can be a path to a file or a string in PEM format. |
| RI_ENCRYPTION_KEY | Key to encrypt data with | n/a | Redis Insight stores sensitive information (database passwords, Workbench history, etc.) locally (using sqlite3⁠). This variable allows you to store sensitive information encrypted using the specified encryption key. Note: The same encryption key should be provided for subsequent docker run commands with the same volume attached to decrypt the information. |
| RI_LOG_LEVEL | Configures the log level of the application. | info | Supported logging levels are prioritized from highest to lowest: error, warn, info, http, verbose, debug, silly |
| RI_FILES_LOGGER | Log to file | true |By default, you can find log files in the following folders: /data/logs |
| RI_STDOUT_LOGGER| Log to STDOUT | true | |
| RI_PROXY_PATH | Configures a subpath for a proxy | n/a | |

# Gestion accès

https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials.html

https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials_environment.html

```
$ export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
   # The access key for your AWS account.
$ export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   # The secret access key for your AWS account.
$ export AWS_SESSION_TOKEN=AQoDYXdzEJr...<remainder of security token>
   # The session key for your AWS account. This is needed only when you are using temporary credentials.
   # The AWS_SECURITY_TOKEN environment variable can also be used, but is only supported for backward compatibility purposes.
   # AWS_SESSION_TOKEN is supported by multiple AWS SDKs other than PHP.
```

Lancer composer:
```
docker run --rm --interactive --tty -v ${PWD}:/app \
-u $(id -u):$(id -g) -e COMPOSER_HOME=/app/composer \
-w /app prooph/composer:7.1 init

docker run --rm --interactive --tty -v ${PWD}:/app \
-u $(id -u):$(id -g) -e COMPOSER_HOME=/app/composer \
-w /app prooph/composer:7.1 install
```

Créer les `AWS_ACCESS_KEY_ID` et `AWS_SECRET_ACCESS_KEY` qu'on passera
```
docker run --rm --interactive --tty -v ${PWD}:/app \
-u $(id -u):$(id -g) -e COMPOSER_HOME=/app/composer \
--env-file rootkey.csv \
 -w /app --entrypoint=php prooph/composer:7.1 \
 bin/app.php
 # na pas fonctionne
```
Cela ne fonctionne pa non-plus:
```
docker run --rm --interactive --tty -v ${PWD}:/app \
-u $(id -u):$(id -g) -e COMPOSER_HOME=/app/composer \
-e AWS_ACCESS_KEY_ID=XXXXXXXX \
-e AWS_SECRET_ACCESS_KEY=XXXXXXXX \
-w /app --entrypoint=php prooph/composer:7.1 \
 bin/app.php
```

Ce qui fonctionne est de créer un dossier `.aws` et ajouter `putenv('HOME=/app');`
dans le code, par la suite:
```
docker run --rm --interactive --tty -v ${PWD}:/app \
-u $(id -u):$(id -g) -e COMPOSER_HOME=/app/composer \
-w /app --entrypoint=php prooph/composer:7.1 \
 bin/app.php
```
ou bien faire:
```
docker run --rm --interactive --tty -v ${PWD}:/app \
-u $(id -u):$(id -g) \
-e HOME=/app -e COMPOSER_HOME=/app/composer  \
-w /app --entrypoint=php prooph/composer:7.1  bin/app.php
```

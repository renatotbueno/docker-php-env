# Docker PHP Environment
A complete PHP environment using:

- Nginx
- PHP-FPM
- Mysql
- Redis
- Solr

### Instalation

1. Fork the repository.
2. Access the path of directory.
3. Run **docker-compose up --build**

### Structure

```
├── docker-php-env
    │   ├── nginx
    │   │   ├── Dockefile
    │   │   └── myaoo.local.conf
    │   │
    │   ├── php
    │   │   ├── .ssh
    │   │   │   ├── id_rsa
    │   │   │   └── id_rsa.pub
    │   │   │
    │   │   ├── custom_php.ini
    │   │   └── Dockerfile
    │   │ 
    │   └── redis
    │       ├── docker-entrypoint.sh
    │       └── Dockerfile      
    │   └── solr
    │       ├── configsets
    │       ├── lib
    │       │   └── mysql-connector-java.jar
    │       ├── mycore
    │       │   ├── conf
    │       │   │   ├── data-config.xml
    │       │   │   └── managed-schema
    │       │   └── core.properties
```

### Services

1. **Nginx** - http://ip_docker:8881
2. **PHP-FPM** - http://ip_docker:9000
3. **Redis** - http://ip_docker:8881
4. **Solr** - http://ip_docker:8983

##### - PHP
The path *.ssh* is used to copy yours *ssh keys* to docker environment, that way you can use composer private packages from your github or bitbucket account using the same *ssh key* whithout add new *ssh key* on your github or bitbucket account.

##### - Nginx
The file *Dockerfile* contains the *vhost* configuration file.
The file *myapp.local.conf* is a use example.

##### - Solr
Into the **sorl** path you will find a *mycore* path that contains all solr configuration.
The most important files are:
- **core.properties** contains the database informations
- **conf > data-config** contains the SQL to import data from your database to your solr core
- **conf > managed-schema** contains all your core fields ( your will find a commented section <!-- *my fields* -> )

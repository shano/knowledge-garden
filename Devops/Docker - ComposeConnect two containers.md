## How to connnect two containers

 If they're sitting under the same network, simply add the container name under external_links:
```

    version: '2'
    
    networks:
        default:
            external:
                name: dev
    
    services:
        php:
            build:
                context: docker/dev/
                dockerfile: Dockerfile
            container_name: tiff-downloader
            image: tiff-downloader
            environment:
                VIRTUAL_HOST:   "tiff-downloader.local"
                PHP_IDE_CONFIG: "serverName=tiff-downloader.php"
                SSH_AUTH_SOCK:  "/ssh-agent"
                CERT_NAME:      "dev"
            external_links:
                - memcached
                - mysql
                - rabbit
                - downloader
            volumes:
                - /etc/localtime:/etc/localtime:ro
                - ./:/var/www/html
                - ./docker/dev/supervisord.conf:/etc/supervisor/conf.d/supervisord.conf
```

Then you access it using something like http://downloader, not http://downloader.local

#tech/docker
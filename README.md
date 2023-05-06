## Build images

### Octane Roadrunner

```bash
docker build -t octane-roadrunner . -f docker/octane-roadrunner/Dockerfile
```

### Octane Swoole

```bash
docker build -t octane-swoole . -f docker/octane-swoole/Dockerfile
```

### Nginx PHP-FPM

```bash
docker build -t nginx-php-fpm . -f docker/nginx-php-fpm/Dockerfile
```

## Run image

### Octane Roadrunner

```bash
docker run -it --rm -p 8080:8080 octane-roadrunner
```

### Octane Swoole

```bash
docker run -it --rm -p 8080:8080 octane-swoole
```

### Nginx
```bash
docker run -it --rm -p 8080:80 nginx-php-fpm
```

## Push images to artifact registry

### Octane Roadrunner

```bash
docker tag octane-roadrunner eu.gcr.io/nuxt-319423/octane-roadrunner
docker push eu.gcr.io/nuxt-319423/octane-roadrunner
```

### Octane Swoole

```bash
docker tag octane-swoole eu.gcr.io/nuxt-319423/octane-swoole
docker push eu.gcr.io/nuxt-319423/octane-swoole
```

### Nginx PHP-FPM

```bash
docker tag nginx-php-fpm eu.gcr.io/nuxt-319423/nginx-php-fpm
docker push eu.gcr.io/nuxt-319423/nginx-php-fpm
```

# nb-dev-docker
牛逼的PHP7开发Docker

### Build
```{r, engine='bash', count_lines}
docker-compose build
```

### Start up
```{r, engine='bash', count_lines}
docker-compose up
docker-compose start //background run
```


### Stop
```{r, engine='bash', count_lines}
docker-compose stop
```

### Go inside
```{r, engine='bash', count_lines}
docker exec -t -i nbdevdocker_front_1 /bin/bash
```

### Web Document root
```{r, engine='bash', count_lines}
/var/www/public
```

### Run
```{r, engine='bash', count_lines}
http://localhost
```

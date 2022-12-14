Docker compose file of  version 
```
version: "3.9"
services:
```
  # DataBase 
  db:
```  
    platform: linux/amd64 # Only required in case Apple M1 / ARM
    command: --default-authentication-plugin=mysql_native_password
```

# Image of mysql 
```    
    image: "mysql:8.0.27"
```
    ports:
      - "3307:3306"
# environment variable      
    environment:
      - MYSQL_DATABASE=abc
      - MYSQL_ROOT_PASSWORD=abc
# Store Data into volumes
```
    volumes:
      - database-data:/var/lib/mysql # default path
```

  api:
    build: .
    ports:
      - "8080:80"
    env_file: .env
    depends_on:
      - db
    restart: on-failure # require because mysql might take couple of seconds to be ready to accept connectsions
    volumes:
      - .:/code

volumes:
  database-data: null # named volumes can be managed easier using docker-compose  
  
  
  
  ```
  version: "3.9"
services:
  db:
    # container_name: therapy-link-mysql
    platform: linux/amd64 # Only required in case Apple M1 / ARM
    command: --default-authentication-plugin=mysql_native_password
    image: "mysql:8.0.27"
    ports:
      - "3306:3306"
    environment:
      - MYSQL_DATABASE=abc
      - MYSQL_ROOT_PASSWORD=abc
    volumes:
      - database-data:/var/lib/mysql # persist data even if container shuts

  api:
    build: .
    ports:
      - "8080:80"
    env_file: .env
    depends_on:
      - db
    restart: on-failure # require because mysql might take couple of seconds to be ready to accept connectsions
    volumes:
      - .:/code

volumes:
  database-data: null # named volumes can be managed easier using docker-compose  
```
  

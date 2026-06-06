# Laboratorium 13 + 13D
---

# Laboratorium 13

## Autor
Kalina Salamończyk 101658

---

## Sieci

Aplikacja wykorzystuje dwie sieci:
- `backend` — mysql, php, phpmyadmin
- `frontend` — nginx

Nginx jest podłączony do obu sieci (backend i frontend), ponieważ musi komunikować się z PHP-FPM (backend) oraz być dostępny z zewnątrz (frontend).

### Uzasadnienie sieci dla phpMyAdmin

phpMyAdmin został podłączony do sieci `backend`, ponieważ musi mieć bezpośredni dostęp do kontenera MySQL. Nie wymaga obecności w sieci `frontend`, gdyż dostęp użytkownika odbywa się przez wystawiony port 6001 na hoście.

---


## Użyte polecenia i wyniki

### 1. Uruchomienie aplikacji

```bash
docker compose up -d
```

Wynik:
```
PS C:\Users\kalin\lab13> docker compose up -d
[+] up 52/52
 ✔ Image phpmyadmin:5.2    Pulled                                 50.0s
 ✔ Image php:8.2-fpm       Pulled                                 46.0s
 ✔ Image nginx:1.25        Pulled                                 28.3s
 ✔ Network lab13_backend   Created                                0.1s
 ✔ Network lab13_frontend  Created                                0.1s
 ✔ Volume lab13_mysql_data Created                                0.0s
 ✔ Container php           Created                                0.7s
 ✔ Container mysql         Created                                0.7s
 ✔ Container phpmyadmin    Created                                0.4s
 ✔ Container nginx         Created                                0.5s
```

---

### 2. Sprawdzenie statusu kontenerów

```bash
docker compose ps
```

Wynik:
```
PS C:\Users\kalin\lab13> docker compose ps
NAME         IMAGE            COMMAND                  SERVICE      CREATED         STATUS         PORTS
mysql        mysql:8.0        "docker-entrypoint.s…"   mysql        2 minutes ago   Up 2 minutes   3306/tcp, 33060/tcp
nginx        nginx:1.25       "/docker-entrypoint.…"   nginx        2 minutes ago   Up 2 minutes   0.0.0.0:4001->80/tcp, [::]:4001->80/tcp
php          php:8.2-fpm      "docker-php-entrypoi…"   php          2 minutes ago   Up 2 minutes   9000/tcp
phpmyadmin   phpmyadmin:5.2   "/docker-entrypoint.…"   phpmyadmin   2 minutes ago   Up 2 minutes   0.0.0.0:6001->80/tcp, [::]:6001->80/tcp

```

---

### 3. Inspekcja sieci backend

```bash
docker network inspect lab13_backend
```

Wynik:
```
PS C:\Users\kalin\lab13> docker network inspect lab13_backend
[
    {
        "Name": "lab13_backend",
        "Id": "296b882dd7777deca256e8892c70f99c08b17b401be628ee1baa432eac2b6741",
        "Created": "2026-06-06T18:40:01.85739717Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv4": true,
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.21.0.0/16",
                    "Gateway": "172.21.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Options": {
            "com.docker.network.enable_ipv4": "true",
            "com.docker.network.enable_ipv6": "false"
        },
        "Labels": {
            "com.docker.compose.config-hash": "303a5c6a665d112f4cfee901befcc7bb1f43b97b72bbbccd3bbe862f5d0e35b7",
            "com.docker.compose.network": "backend",
            "com.docker.compose.project": "lab13",
            "com.docker.compose.version": "5.0.2"
        },
        "Containers": {
            "6ea8873f81126b1b81c8fd6e3f2dff96eaae59a2e56741afaaae80b8ef952bf8": {
                "Name": "phpmyadmin",
                "EndpointID": "1f41150047c799307345d712911c7563b1cccbdefd9cfd75eb545e9563394cf7",
                "MacAddress": "c2:0e:3a:55:14:02",
                "IPv4Address": "172.21.0.4/16",
                "IPv6Address": ""
            },
            "731ce63b524a581291e87cf98e56cec1ed64ab3a95d92651e24e858d9389965a": {
                "Name": "php",
                "EndpointID": "a9405b86077bd550704742627474e0c3dfefba820e45785ec870123cc685918e",
                "MacAddress": "7e:ac:14:ee:b2:3d",
                "IPv4Address": "172.21.0.3/16",
                "IPv6Address": ""
            },
            "d8e832bbe9aa6debd1186042a68db8eee8881e039b842249d5598a0c5c201142": {
                "Name": "mysql",
                "EndpointID": "f88c25850e77b1f01022f8867dc8383ec1e9235da72bd300ac2da80106843bbb",
                "MacAddress": "ce:83:4f:8a:86:70",
                "IPv4Address": "172.21.0.2/16",
                "IPv6Address": ""
            },
            "f1e38f87e38f1ce24bd70d8e87310b329492cb65fa80700f9374fc14d7820cf8": {
                "Name": "nginx",
                "EndpointID": "20dc98b2c8b9d15952bca85304e48cabf2d70f68155b163b31b843f96b4037ce",
                "MacAddress": "0e:98:0f:9b:e7:a9",
                "IPv4Address": "172.21.0.5/16",
                "IPv6Address": ""
            }
        },
        "Status": {
            "IPAM": {
                "Subnets": {
                    "172.21.0.0/16": {
                        "IPsInUse": 7,
                        "DynamicIPsAvailable": 65529
                    }
                }
            }
        }
    }
]
```

---

### 4. Inspekcja sieci frontend

```bash
docker network inspect lab13_frontend
```

Wynik:
```
PS C:\Users\kalin\lab13> docker network inspect lab13_frontend
[
    {
        "Name": "lab13_frontend",
        "Id": "238d5e12576ee42984b8a55ab16a9db78052f20a4ee3650f9590ab6f0fbe1821",
        "Created": "2026-06-06T18:40:02.019945219Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv4": true,
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.22.0.0/16",
                    "Gateway": "172.22.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Options": {
            "com.docker.network.enable_ipv4": "true",
            "com.docker.network.enable_ipv6": "false"
        },
        "Labels": {
            "com.docker.compose.config-hash": "c99dfed360a6944db43f183a5bd3f4016b0c10584adeeaa2e535a0bc34386287",
            "com.docker.compose.network": "frontend",
            "com.docker.compose.project": "lab13",
            "com.docker.compose.version": "5.0.2"
        },
        "Containers": {
            "f1e38f87e38f1ce24bd70d8e87310b329492cb65fa80700f9374fc14d7820cf8": {
                "Name": "nginx",
                "EndpointID": "a9f06738466f158bb697c209de9f55759d8cd753fc16e1c64b15cd98cd32d269",
                "MacAddress": "62:3d:fc:0c:b5:9d",
                "IPv4Address": "172.22.0.2/16",
                "IPv6Address": ""
            }
        },
        "Status": {
            "IPAM": {
                "Subnets": {
                    "172.22.0.0/16": {
                        "IPsInUse": 4,
                        "DynamicIPsAvailable": 65532
                    }
                }
            }
        }
    }
]

```

---

## Weryfikacja działania

### Strona PHP (Nginx na porcie 4001)

Adres: http://localhost:4001

<img width="1918" height="933" alt="image" src="https://github.com/user-attachments/assets/e57b4362-ee30-47ce-8822-e5d0ad97aacf" />


---

### Panel phpMyAdmin (port 6001)

Adres: http://localhost:6001

Dane logowania:
- użytkownik: `root`
- hasło: `rootpassword`

<img width="1908" height="694" alt="image" src="https://github.com/user-attachments/assets/39ac9a11-da48-4853-8e53-2e5667e8f87f" />


---

### Testowa baza danych

W panelu phpMyAdmin założono testową bazę danych `testowa_baza`.

<img width="234" height="43" alt="image" src="https://github.com/user-attachments/assets/5534364c-0b48-430f-9b01-88cb7b3d4457" />


---

### 5. Zatrzymanie aplikacji

```bash
docker compose down
```

Wynik:
```
PS C:\Users\kalin\lab13> docker compose down
[+] down 6/6
 ✔ Container phpmyadmin   Removed                            1.6s
 ✔ Container nginx        Removed                            1.0s
 ✔ Container php          Removed                            0.6s
 ✔ Container mysql        Removed                            1.5s
 ✔ Network lab13_frontend Removed                            0.3s
 ✔ Network lab13_backend  Removed    
```

---

## Plik docker-compose.yml

```yaml
services:

  mysql:
    image: mysql:8.0
    container_name: mysql
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: testdb
      MYSQL_USER: user
      MYSQL_PASSWORD: userpassword
    volumes:
      - mysql_data:/var/lib/mysql
    networks:
      - backend

  php:
    image: php:8.2-fpm
    container_name: php
    restart: unless-stopped
    volumes:
      - ./php:/var/www/html
    networks:
      - backend

  nginx:
    image: nginx:1.25
    container_name: nginx
    restart: unless-stopped
    ports:
      - "4001:80"
    volumes:
      - ./php:/var/www/html
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - php
    networks:
      - frontend
      - backend

  phpmyadmin:
    image: phpmyadmin:5.2
    container_name: phpmyadmin
    restart: unless-stopped
    ports:
      - "6001:80"
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
    depends_on:
      - mysql
    networks:
      - backend

volumes:
  mysql_data:

networks:
  frontend:
  backend:
```


---

# Laboratorium 13D 

## Opis modyfikacji

Dane wrażliwe (hasła MySQL) zostały przeniesione ze zmiennych środowiskowych do mechanizmu Docker Compose Secrets. Sekrety są montowane w kontenerze pod ścieżką `/run/secrets/<nazwa_sekretu>`.

Zamiast:
```yaml
MYSQL_ROOT_PASSWORD: rootpassword
```

Użyto:
```yaml
MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
```

## Użyte polecenia i wyniki

### 1. Uruchomienie aplikacji z secrets

```bash
docker compose up -d
```

Wynik:
```
PS C:\Users\kalin\lab13> docker compose up -d
[+] up 6/6
 ✔ Network lab13_backend  Created                            0.1s
 ✔ Network lab13_frontend Created                            0.1s
 ✔ Container mysql        Created                            0.2s
 ✔ Container php          Created                            0.2s
 ✔ Container nginx        Created                            0.2s
 ✔ Container phpmyadmin   Created

```

---

### 2. Weryfikacja montowania sekretów

```bash
docker container inspect mysql
```

Wynik (sekcja Mounts):
```
"Mounts": [
                {
                    "Type": "bind",
                    "Source": "C:\\Users\\kalin\\lab13\\db_root_password.txt",
                    "Target": "/run/secrets/db_root_password",
                    "ReadOnly": true,
                    "BindOptions": {}
                },
                {
                    "Type": "bind",
                    "Source": "C:\\Users\\kalin\\lab13\\db_password.txt",
                    "Target": "/run/secrets/db_password",
                    "ReadOnly": true,
                    "BindOptions": {}
                }
            ],
```

---

### 3. Weryfikacja działania po modyfikacji

Strona PHP: http://localhost:4001 — działa poprawnie.

<img width="1913" height="948" alt="image" src="https://github.com/user-attachments/assets/0c1d56d0-6d0d-4c39-9be0-68fe23344b15" />


phpMyAdmin: http://localhost:6001 — logowanie działa, baza dostępna.

<img width="1907" height="723" alt="image" src="https://github.com/user-attachments/assets/cd66e87b-79b8-4955-a78d-bc23caec64d1" />


## Zmodyfikowany docker-compose.yml

```yaml
services:

  mysql:
    image: mysql:8.0
    container_name: mysql
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
      MYSQL_DATABASE: testdb
      MYSQL_USER: user
      MYSQL_PASSWORD_FILE: /run/secrets/db_password
    volumes:
      - mysql_data:/var/lib/mysql
    secrets:
      - db_root_password
      - db_password
    networks:
      - backend

  php:
    image: php:8.2-fpm
    container_name: php
    restart: unless-stopped
    volumes:
      - ./php:/var/www/html
    networks:
      - backend

  nginx:
    image: nginx:1.25
    container_name: nginx
    restart: unless-stopped
    ports:
      - "4001:80"
    volumes:
      - ./php:/var/www/html
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - php
    networks:
      - frontend
      - backend

  phpmyadmin:
    image: phpmyadmin:5.2
    container_name: phpmyadmin
    restart: unless-stopped
    ports:
      - "6001:80"
    environment:
      PMA_HOST: mysql
      PMA_PORT: 3306
    depends_on:
      - mysql
    networks:
      - backend

secrets:
  db_root_password:
    file: ./db_root_password.txt
  db_password:
    file: ./db_password.txt

volumes:
  mysql_data:

networks:
  frontend:
  backend:

```

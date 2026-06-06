# Laboratorium 13 — Docker Compose: Stack LEMP + phpMyAdmin

## Autor
Imię Nazwisko, nr indeksu

---

## Opis projektu

Aplikacja wielokontenerowa uruchomiona za pomocą Docker Compose, składająca się z czterech mikrousług:
- **nginx:1.25** — serwer WWW, dostępny na porcie 4001
- **php:8.2-fpm** — interpreter PHP
- **mysql:8.0** — baza danych
- **phpmyadmin:5.2** — panel administracyjny bazy, dostępny na porcie 6001

---

## Topologia sieci

Aplikacja wykorzystuje dwie sieci:
- `backend` — mysql, php, phpmyadmin
- `frontend` — nginx

Nginx jest podłączony do obu sieci (backend i frontend), ponieważ musi komunikować się z PHP-FPM (backend) oraz być dostępny z zewnątrz (frontend).

### Uzasadnienie sieci dla phpMyAdmin

phpMyAdmin został podłączony do sieci `backend`, ponieważ musi mieć bezpośredni dostęp do kontenera MySQL. Nie wymaga obecności w sieci `frontend`, gdyż dostęp użytkownika odbywa się przez wystawiony port 6001 na hoście — izolacja od warstwy frontendowej jest tu zasadna z punktu widzenia bezpieczeństwa.

---

## Struktura projektu

```
lab13/
├── docker-compose.yml
├── nginx/
│   └── default.conf
├── php/
│   └── index.php
└── README.md
```

---

## Użyte polecenia i wyniki

### 1. Uruchomienie aplikacji

```bash
docker compose up -d
```

Wynik:
```
[WKLEJ TUTAJ cały output z terminala]
```

---

### 2. Sprawdzenie statusu kontenerów

```bash
docker compose ps
```

Wynik:
```
[WKLEJ TUTAJ output]
```

---

### 3. Inspekcja sieci backend

```bash
docker network inspect lab13_backend
```

Wynik:
```
[WKLEJ TUTAJ output — interesuje nas sekcja "Containers"]
```

---

### 4. Inspekcja sieci frontend

```bash
docker network inspect lab13_frontend
```

Wynik:
```
[WKLEJ TUTAJ output]
```

---

## Weryfikacja działania

### Strona PHP (Nginx na porcie 4001)

Adres: http://localhost:4001

[WKLEJ TUTAJ screenshot strony phpinfo()]

---

### Panel phpMyAdmin (port 6001)

Adres: http://localhost:6001

Dane logowania:
- użytkownik: `root`
- hasło: `rootpassword`

[WKLEJ TUTAJ screenshot panelu phpMyAdmin po zalogowaniu]

---

### Testowa baza danych

W panelu phpMyAdmin założono testową bazę danych `testowa_baza`.

[WKLEJ TUTAJ screenshot założonej bazy]

---

### 5. Zatrzymanie aplikacji

```bash
docker compose down
```

Wynik:
```
[WKLEJ TUTAJ output]
```

---

## Plik docker-compose.yml

```yaml
[WKLEJ TUTAJ całą zawartość swojego docker-compose.yml]
```

# ДЗ otus-pgsql-hw-lesson-11


## Пункт 1.
### Развернуть виртуальную машину любым удобным способом. поставить на неё PostgreSQL 15 любым способом

ВМ с 2 ядрами и 4 Гб ОЗУ и SSD 10GB создана

    bash-4.2$ systemctl status postgresql-15
    ● postgresql-15.service - PostgreSQL 15 database server
       Loaded: loaded (/usr/lib/systemd/system/postgresql-15.service; disabled; vendor preset: disabled)
       Active: active (running) since Wed 2024-01-17 03:12:49 EST; 4 days ago
         Docs: https://www.postgresql.org/docs/15/static/
      Process: 92558 ExecStartPre=/usr/pgsql-15/bin/postgresql-15-check-db-dir ${PGDATA} (code=exited, status=0/SUCCESS)
     Main PID: 92563 (postmaster)
       CGroup: /system.slice/postgresql-15.service
               ├─92563 /usr/pgsql-15/bin/postmaster -D /var/lib/pgsql/15/data/
               ├─92565 postgres: logger
               ├─92566 postgres: checkpointer
               ├─92567 postgres: background writer
               ├─92569 postgres: walwriter
               ├─92570 postgres: autovacuum launcher
               ├─92571 postgres: logical replication launcher
               ├─97054 postgres: postgres postgres [local] idle
               └─97393 postgres: postgres postgres [local] idle
    bash-4.2$

Тест производительности до оптимизации

    pgbench -c8 -P 6 -T 60 -U postgres postgres
    
    bash-4.2$ pgbench -c8 -P 6 -T 60 -U postgres postgres
    Password:
    pgbench (15.5)
    starting vacuum...end.
    progress: 6.0 s, 2334.5 tps, lat 3.400 ms stddev 1.533, 0 failed
    progress: 12.0 s, 2409.4 tps, lat 3.310 ms stddev 1.275, 0 failed
    progress: 18.0 s, 2413.7 tps, lat 3.304 ms stddev 1.270, 0 failed
    progress: 24.0 s, 2413.0 tps, lat 3.304 ms stddev 1.251, 0 failed
    progress: 30.0 s, 2376.8 tps, lat 3.355 ms stddev 1.326, 0 failed
    progress: 36.0 s, 2397.2 tps, lat 3.327 ms stddev 1.250, 0 failed
    progress: 42.0 s, 2368.0 tps, lat 3.368 ms stddev 1.578, 0 failed
    progress: 48.0 s, 2370.8 tps, lat 3.362 ms stddev 1.402, 0 failed
    progress: 54.0 s, 2397.9 tps, lat 3.327 ms stddev 1.285, 0 failed
    progress: 60.0 s, 2350.2 tps, lat 3.393 ms stddev 1.290, 0 failed
    transaction type: <builtin: TPC-B (sort of)>
    scaling factor: 1
    query mode: simple
    number of clients: 8
    number of threads: 1
    maximum number of tries: 1
    duration: 60 s
    number of transactions actually processed: 142996
    number of failed transactions: 0 (0.000%)
    latency average = 3.345 ms
    latency stddev = 1.351 ms
    initial connection time = 26.469 ms
    tps = 2383.799684 (without initial connection time)
    bash-4.2$



## Пункт 2. 
### Настроить кластер PostgreSQL 15 на максимальную производительность не обращая внимание на возможные проблемы с надежностью в случае аварийной перезагрузки виртуальной машины

Использовал сайт :

https://pgconfigurator.cybertec.at/

![data source](https://github.com/olegrovenskiy/otus-pgsql-hw-lesson-11/blob/main/dz11.png)

и применил конфигурацию


## Пункт 3.
### Нагрузить кластер через утилиту через утилиту pgbench (https://postgrespro.ru/docs/postgrespro/14/pgbench)

        bash-4.2$ pgbench -c8 -P 6 -T 60 -U postgres postgres
        Password:
        pgbench (15.5)
        starting vacuum...end.
        progress: 6.0 s, 2304.4 tps, lat 3.444 ms stddev 1.360, 0 failed
        progress: 12.0 s, 2298.2 tps, lat 3.472 ms stddev 1.575, 0 failed
        progress: 18.0 s, 2407.7 tps, lat 3.313 ms stddev 1.216, 0 failed
        progress: 24.0 s, 2343.9 tps, lat 3.402 ms stddev 1.274, 0 failed
        progress: 30.0 s, 2314.0 tps, lat 3.446 ms stddev 1.294, 0 failed
        progress: 36.0 s, 2384.4 tps, lat 3.345 ms stddev 1.266, 0 failed
        progress: 42.0 s, 2362.6 tps, lat 3.376 ms stddev 1.274, 0 failed
        progress: 48.0 s, 2374.5 tps, lat 3.359 ms stddev 1.244, 0 failed
        progress: 54.0 s, 2361.3 tps, lat 3.378 ms stddev 1.244, 0 failed
        progress: 60.0 s, 2400.3 tps, lat 3.323 ms stddev 1.211, 0 failed
        transaction type: <builtin: TPC-B (sort of)>
        scaling factor: 1
        query mode: simple
        number of clients: 8
        number of threads: 1
        maximum number of tries: 1
        duration: 60 s
        number of transactions actually processed: 141316
        number of failed transactions: 0 (0.000%)
        latency average = 3.385 ms
        latency stddev = 1.300 ms
        initial connection time = 25.854 ms
        tps = 2355.760316 (without initial connection time)
        bash-4.2$


## Пункт 4.
### Написать какого значения tps удалось достичь, показать какие параметры в какие значения устанавливали и почему

Ранее параметры были ыставлены по рекомендациям на первых занятиях, и как видно применение параметров с рекомендованного сайта не улучшило tps, 
думаю что данные параметры ВМ не позволяют уже дополнительно что либо оптимизировать.

Для эксперимента увеличил число CPU с 2 до 4.

        bash-4.2$ pgbench -c8 -P 6 -T 60 -U postgres postgres
        Password:
        pgbench (15.5)
        starting vacuum...end.
        progress: 6.0 s, 3229.5 tps, lat 2.459 ms stddev 1.199, 0 failed
        progress: 12.0 s, 3256.0 tps, lat 2.450 ms stddev 1.207, 0 failed
        progress: 18.0 s, 3296.2 tps, lat 2.420 ms stddev 1.177, 0 failed
        progress: 24.0 s, 3319.0 tps, lat 2.404 ms stddev 1.165, 0 failed
        progress: 30.0 s, 3234.7 tps, lat 2.467 ms stddev 1.222, 0 failed
        progress: 36.0 s, 3268.5 tps, lat 2.441 ms stddev 1.166, 0 failed
        progress: 42.0 s, 3275.7 tps, lat 2.435 ms stddev 1.186, 0 failed
        progress: 48.0 s, 3256.6 tps, lat 2.450 ms stddev 1.187, 0 failed
        progress: 54.0 s, 3300.9 tps, lat 2.417 ms stddev 1.178, 0 failed
        progress: 60.0 s, 3231.8 tps, lat 2.469 ms stddev 1.203, 0 failed
        transaction type: <builtin: TPC-B (sort of)>
        scaling factor: 1
        query mode: simple
        number of clients: 8
        number of threads: 1
        maximum number of tries: 1
        duration: 60 s
        number of transactions actually processed: 196020
        number of failed transactions: 0 (0.000%)
        latency average = 2.441 ms
        latency stddev = 1.189 ms
        initial connection time = 26.179 ms
        tps = 3268.098680 (without initial connection time)
        bash-4.2$

Данное изменение дало заметный прирост по TPS = 3268/




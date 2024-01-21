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






## Пункт 3.
### Нагрузить кластер через утилиту через утилиту pgbench (https://postgrespro.ru/docs/postgrespro/14/pgbench)



написать какого значения tps удалось достичь, показать какие параметры в какие значения устанавливали и почему

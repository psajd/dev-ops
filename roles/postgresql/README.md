Настройка `master`:
* Включается репликация через изменение параметров в postgresql.conf (например, `wal_level = replica`)

Настройка `slave`:
* На реплике создается копия данных мастера с помощью команды `pg_basebackup`.
*	Устанавливается файл `standby.signal`, который сообщает PostgreSQL, что это реплика

Сетап:
```bash
vagrant up
ansible-playbook -i replication-hosts.ini playbooks/setup_postgres.yml
```

Для теста создадим тестовую таблицу на `master`, запишем в нее данные:

```sql
CREATE TABLE test (a INT);
INSERT INTO test (id) VALUES (1);
```

Теперь может прочитать из этой таблицы на `replicaN`:

```sql
SELECT * FROM test;
```
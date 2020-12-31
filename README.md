# my-first-python
my-first-python


    which psql
    createdb
    psql -h localhost

    pip install psycopg2-binary


И используйте этот пакет для подключения DATABASE_URLв своем коде.
```python
    import os
    import psycopg2

    DATABASE_URL = os.environ['DATABASE_URL']

    conn = psycopg2.connect(DATABASE_URL, sslmode='require')
  ```




**Подключение с Django**


Установите `dj-database-urlпакет`, используя `pip`.

    pip install dj-database-url

Обязательно добавьте `psycopg2-binary` и `dj-database-url` в свой requirements.txt файл.

**Затем добавьте в конец** `settings.py:*`

```python
    import dj_database_url
    DATABASES['default'] = dj_database_url.config(conn_max_age=600, ssl_require=True)
```

### Фоновые задачи в Python с RQ

    pip install rq

#### Создать работника

Теперь, когда у нас есть все необходимое для создания рабочего процесса, давайте создадим его.

Создайте файл с именем `worker.py`. Этот модуль будет прослушивать задачи в очереди и обрабатывать их по мере их поступления.

```python
import os

import redis
from rq import Worker, Queue, Connection

listen = ['high', 'default', 'low']

redis_url = os.getenv('REDISTOGO_URL', 'redis://localhost:6379')

conn = redis.from_url(redis_url)

if __name__ == '__main__':
    with Connection(conn):
        worker = Worker(map(Queue, listen))
        worker.work()
```


Теперь вы можете запустить новый рабочий процесс:

    python worker.py


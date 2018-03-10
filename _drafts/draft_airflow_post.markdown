# My Airflow journey

Not long ago I knew nothing about task schedulers. I lie, I knew that they
existed, I had heard of Airflow and Azkaban, but never really felt I needed
them. I was wrong.

Someone new at work - who had introduced me to i3 - was a big fan of Luigi, and made me start wondering if I should give it some time.

My objective was to be able to programatically define an ETL process, schedule
it, and run it getting logs that told me how it ran and that helped me track if
there had been any issues. Also that allowed me to decide if I wanted to re-run
part of it or branch out different alternatives if necessary. As a stretch I
wanted to see if I could monitor parallel execution of some processes.

I read a bit about the alternatives , talked to a few friends about their thoughts regarding the most popular options, and ended up deciding to give Airflow a chance. This is my learning journey, I want to make it as truthful as possible, and maybe help new learners not get stuck where I got stuck.

I started by going to the [official Airflow project
page](https://airflow.readthedocs.io/en/latest/index.html). I installed it
using the instructions and followed the tutorial and was able to get to the
browser UI. Success!! (not). Everything seemed simple enough but I wasn't really understanding what I was doing. Even worse, I didn't feel any closer to achieving my objective.

As I felt stuck I looked for an alternative tutorial. From a first look [this
example](https://gtoonstra.github.io/etl-with-airflow/etlexample.html) felt
exactly like what I needed. And on top of that it used Docker to keep
everything clean. That would also be useful if I ever was to implement this at
work where the environment is Windows.

In summary, the idea of the ETL tutorial was to create a Docker-compose network
with a Postgres service and a webserver. It sounded quite neat, and exactly
what I wanted, but as you might have guessed it turned out to be not that great. 
The first time I ran it, it crashed right away because the Postgres port 5432,
was already in use on my computer. I got some help and learnt that in the
docker compose file when declaring the ports for Postgres if instead of the
`ports: 5432:5432` I had just `ports: "5432"` Docker would not expose any ports to the exterior. In this case, that wasn't needed so it was a good solution.

Second attempt: the webserver ran, so I could go to the browser and on
`localhost:8080` I could see the Airflow UI. It felt a bit better, still I got
a few errors. The errors where all related to not finding a
variable: `sql_path`, in the execution of two Python scripts.

The problem can be solved by running a script that can be found on the `DAGs`
tab of the UI: click on the `off` to get it `on`, wait a few seconds and reload
the page. With that the problem is fixed.

However, when the container is stopped the configuration doesn't persist, which
means that every time you run the process you need to re-run the init script.

To avoid that, I had to modify the docker-compose yaml configuration file. The
neecessary additions were as shown below, first the original file then the one
with the fixes:

**docker-compose-LocalExecutor.yml**

```
version: '2'
services:
  postgres:
      image: postgres:9.6
      environment:
          - POSTGRES_USER=airflow
          - POSTGRES_PASSWORD=airflow
          - POSTGRES_DB=airflow
      ports:
          - "5432"
      volumes:
          - ./examples/do-this-first/database_user.sql:/docker-entrypoint-initdb.d/database_user.sql
          - ./examples/do-this-first/dwh_tables.sql:/docker-entrypoint-initdb.d/dwh_tables.sql
          - ./examples/do-this-first/populate_tables.sql:/docker-entrypoint-initdb.d/populate_tables.sql
  webserver:
      image: puckel/docker-airflow:1.8.0
      restart: always
      depends_on:
          - postgres
      environment:
          - LOAD_EX=n
          - EXECUTOR=Local
      volumes:
          - ./examples/etl-example/dags:/usr/local/airflow/dags
          - ./examples/etl-example/sql:/usr/local/airflow/sql
      ports:
          - 8080:8080
      command: webserver
```


```
version: '2'
services:
  postgres:
      image: postgres:9.6
      environment:
          - POSTGRES_USER=airflow
          - POSTGRES_PASSWORD=airflow
          - POSTGRES_DB=airflow
          - PGDATA=/var/lib/postgresql/data/pgdata
      ports:
          - "5432"
      volumes:
          - ./examples/do-this-first/database_user.sql:/docker-entrypoint-initdb.d/database_user.sql
          - ./examples/do-this-first/dwh_tables.sql:/docker-entrypoint-initdb.d/dwh_tables.sql
          - ./examples/do-this-first/populate_tables.sql:/docker-entrypoint-initdb.d/populate_tables.sql
          - data:/var/lib/postgresql/data/pgdata
  webserver:
      image: puckel/docker-airflow:1.8.0
      restart: always
      depends_on:
          - postgres
      environment:
          - LOAD_EX=n
          - EXECUTOR=Local
      volumes:
          - ./examples/etl-example/dags:/usr/local/airflow/dags
          - ./examples/etl-example/sql:/usr/local/airflow/sql
          - ./airflow.cfg:/usr/local/airflow/airflow.cfg
      ports:
          - 8080:8080
      command: webserver
volumes:
  data: {}
```

Basically what changes is that we add a data volume and a means to persist the
`airflow.cfg` configuration file.



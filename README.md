* `airflow` DAGs are stored in a single centrally located folder, defined by the `airflow.cfg` file in the 
`~/airflow/` HOME folder and the `AIRFLOW_HOME` Linux environment variable (in some combination?).
* When `airflow` is initially installed, this is configured to be `~/airflow/dags/`.
* (In `datablocks`, I will want to write a run hook in the package `__init__.py` that moves the folder reference to 
the currently installed folder.)
* `airflow list_dags` is a command that lists known DAGs. It initially contains many tutorial DAGs that are included 
with the package on install.
* `airflow list_tasks {{ filename }}` lists tasks in the chosen DAG.
* Adding the `--tree` operator lists tasks in the shape of a tree. In this output, the indentation reflects the 
dependence of endpoint tasks on tasks that come before them.
* By default, `airflow` uses a sequential execution engine. It can be configured to use...well, there are a lot of 
options. To get parallel execution, you will need to configure for extras.
* To test-run a DAG, `airflow test {{ dag_id }} {{ task_id}} date`, where date is a datetime that you want to run 
the task on. e.g. `airflow test tutorial print_date 2015-06-01`. Can also be e.g. `2017-1-23T10:34`. 
* FYI, there is no way to delete a DAG directly. Instead, you have to use a custom script. See [here](https://stackoverflow.com/questions/40651783/airflow-how-to-delete-a-dag).
* Due to [this bug](https://issues.apache.org/jira/browse/AIRFLOW-1165), the management server is broken right now 
when `airflow` is deployed in localhost (default) mode.
  * Deploy it with `airflow webserver --debug`. 
  *`&`  (backgrounding) is helpful.
  * Note that when you `KeyboardInterrupt` this, the socket connection is *not* closed in the OS (explanation [here](https://stackoverflow.com/questions/4465959/python-errno-98-address-already-in-use)).
  To free the port, you need to kill the leftover process. Easiest way is by doing `sudo fuser -k 8080/tcp`.
  * This bug is fixed in master, RC `1.9.0`. But I am using the latest release, `1.8.0`...
  * The workaround is to generate and self-sign an SSL certificate private/public key pair, and then associate that 
  with `airflow` webserver via [these instructions](http://airflow.readthedocs.io/en/latest/security.html#ssl). cf. [here](https://issues.apache.org/jira/browse/AIRFLOW-1165). 
  * However, this results in a different error: `SSL Wrong Version Number`. The site ([link](http://localhost:8080/)) 
  still doesn't work.
* `airflow backfill {{ dag_id}} -s {{ start date }} [ -e {{end date}} ]` runs the entire DAG on a range of simulated 
dates. The end date is optional.
* `airflow` uses `sqlite` as its backend by default, with a `SequentialExecutor` running scheduled jobs. With more 
configuration, you can get something that runs in parallel.

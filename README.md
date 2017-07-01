* `airflow` DAGs are stored in a single centrally located folder, defined by the `airflow.cfg` file in the 
`~/airflow/` HOME folder.
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

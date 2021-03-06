# Airflow prometheus exporter

Exposes dag and task based metrics from Airflow to a Prometheus compatible endpoint.

## Compatibility

**Note: Airflow 1.10.14 with Python 3.8 users**

You should install `importlib-metadata` package in order for plugin to be loaded. See [#85](https://github.com/epoch8/airflow-exporter/issues/85) for details.

**Note: this version is compatible with Airflow 1.10.3+ only, see [#46](https://github.com/epoch8/airflow-exporter/issues/46) for details**

For compatibility with previous versions of Airflow use older version: [v0.5.4](https://github.com/epoch8/airflow-exporter/releases/tag/v0.5.4)

* Airflow: airflow1.10.3+
* Python: python2, python3
* DataBase: postgresql, mysql

## Install

```sh
pip install airflow-exporter
```

That's it. You're done.

## Exporting extra labels to Prometheus

It is possible to add extra labels to DAG-related metrics by providing `labels` dict to DAG `params`.

### Example

```python
dag = DAG(
    'dummy_dag',
    schedule_interval=timedelta(hours=5),
    default_args=default_args,
    catchup=False,
    params={
        'labels': {
            'env': 'test'
        }
    }
)
```

Label `env` with value `test` will be added to all metrics related to `dummy_dag`:

`airflow_dag_status{dag_id="dummy_dag",env="test",owner="owner",status="running"} 12.0`

## Metrics

Metrics will be available at 

```
http://<your_airflow_host_and_port>/admin/metrics/
```

### `airflow_task_status`

Labels:

* `dag_id`
* `task_id`
* `owner`
* `status`
* `hostname`

Value: number of tasks in specific status.

### `airflow_dag_status`

Labels:

* `dag_id`
* `owner`
* `status`

Value: number of dags in specific status.

### `airflow_dag_run_duration`

Labels:

* `dag_id`: unique identifier for a given DAG

Value: duration in seconds of the longest DAG Run for given DAG. This metric 
is not available for DAGs that have already completed.

## License

Distributed under the BSD license. See [LICENSE](LICENSE) for more
information.

# composer-dev-example
[source code](https://github.com/GoogleCloudPlatform/composer-local-dev)  
[document](https://cloud.google.com/composer/docs/composer-2/run-local-airflow-environments)

認証情報の構成
```
$ gcloud auth application-default login
$ gcloud auth login
```
ライブラリのInstall
```
$ cd composer-local-dev
$ pip3 install .
```
ローカル Composer環境の作成
```
$ composer-dev create \
  --from-image-version IMAGE_VERSION \
  --dags-path LOCAL_DAGS_PATH \
  LOCAL_ENVIRONMENT_NAME
```
例
```
$ composer-dev create \
  --from-image-version composer-2.0.32-airflow-2.2.5 \
  example-local-environment
```
--dag-pathを指定しないとcomposerディレクトリが作成されその下にdags, data, airflow.dbなどが作成される。  

git cloneしたフォルダの下ではなく出ても環境は作成できる。
```
/Users/sample/composer-dev-example $ composer-dev create \                         
  --from-image-version composer-2.0.32-airflow-2.2.5 \
  --dags-path dags \
  example-local-environment
```

作成した環境の確認
```
$ composer-dev list

  Environment Name          │ Version*                      │ State        
╶───────────────────────────┼───────────────────────────────┼─────────────╴
  example-local-environment │ composer-2.0.32-airflow-2.2.5 │ Not started  
```
作成した環境の詳細情報を表示
```
$ composer-dev describe

Composer example-local-environment environment is in state: Not started.

Image version: composer-2.0.32-airflow-2.2.5
Dags directory: /Users/sample/composer-dev-example/composer-local-dev/composer/example-local-environment/dags.
The environment is using credentials from gcloud located at /Users/sample/.config/gcloud.
```
Airflow 環境をスタート（localhost:8080でAirflow UIにアクセス）
```
$ composer-dev start example-local-environment
Starting example-local-environment composer environment...
...
Started example-local-environment environment.

1. You can put your DAGs in /Users/sample/composer-dev-example/composer-local-dev/composer/example-local-environment/dags
2. Access Airflow at http://localhost:8080
```

Dagを変更して反映させるために再起動
```
$ composer-dev restart example-local-environment
...
2022-12-24T06:43:38.716401176Z DAG_PROCESSOR_MANAGER_LOG:[2022-12-24 06:43:38,715] {manager.py:518} INFO - Process each file at most once every 30 seconds
2022-12-24T06:43:38.716415718Z [2022-12-24 06:43:38,715] {scheduler_job.py:1235} INFO - Marked 1 SchedulerJob instances as failed
2022-12-24T06:43:38.716975468Z DAG_PROCESSOR_MANAGER_LOG:[2022-12-24 06:43:38,716] {manager.py:519} INFO - Checking for new files in /home/airflow/airflow/dags every 10 seconds
2022-12-24T06:43:38.719036343Z DAG_PROCESSOR_MANAGER_LOG:[2022-12-24 06:43:38,717] {manager.py:704} INFO - Searching for files in /home/airflow/airflow/dags

Started example-local-environment environment.

1. You can put your DAGs in /Users/sample/composer-dev-example/composer-dev-example/dags
2. Access Airflow at http://localhost:8080
```

ログの確認
```
$ composer-dev logs example-local-environment
...
run_id='scheduled__2022-01-01T00:00:00+00:00', try_number=1) to executor with priority 1 and queue default
2022-12-24T06:45:02.809633465Z [2022-12-24 06:45:02,809] {base_executor.py:85} INFO - Adding to queue: ['airflow', 'tasks', 'run', 'demo', 'airflow', 
'scheduled__2022-01-01T00:00:00+00:00', '--local', '--subdir', 'DAGS_FOLDER/sample.py']
2022-12-24T06:45:02.815494757Z [2022-12-24 06:45:02,814] {sequential_executor.py:59} INFO - Executing command: ['airflow', 'tasks', 'run', 'demo', 'hello', 
'scheduled__2022-01-02T00:00:00+00:00', '--local', '--subdir', 'DAGS_FOLDER/sample.py']
```

Airflow 環境の停止
```
$ composer-dev stop example-local-environment
Stopped composer local environment.
```
Airflow 環境の削除..動かない
```
$ composer-dev remove example-local-environment
Usage: composer-dev [OPTIONS] COMMAND [ARGS]...
Try 'composer-dev --help' for help.
╭─ Error ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ No such command 'remove'.                                                                                                      │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```
Docker イメージの削除..動かない
```
$ docker rmi $(docker images --filter=reference='*/cloud-airflow-releaser/*/*' -q)
Error response from daemon: conflict: unable to delete 428a4887bf60 (must be forced) - image is being used by stopped container 4ccc178d3498
```
Docker イメージの削除..-fオプションで強制削除
```
$ docker rmi -f $(docker images --filter=reference='*/cloud-airflow-releaser/*/*' -q)
Error response from daemon: conflict: unable to delete 428a4887bf60 (must be forced) - image is being used by stopped container 4ccc178d3498
```

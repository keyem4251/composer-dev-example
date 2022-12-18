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

# Static Jinja Plus Docker Port

Докер-порт проекта [StaticJinjaPlus](https://github.com/MrDave/StaticJinjaPlus).<br>
Все образы доступны на [DockerHub](https://hub.docker.com/repository/docker/lypavel/static-jinja-plus/general).

## Запуск контейнера

1. [Установите Docker](https://docs.docker.com/engine/install/), если необходимо.
2. Скачайте интересующий вас образ из DockerHub:
    ```sh
    docker pull lypavel/static-jinja-plus:${TAG}
    ```
    `${TAG}` - тег образа
3. Запустите контейнер
    ```sh
    docker run --rm -v ${TEMPLATES_PATH}:/opt/StaticJinjaPlus/templates -v ${BUILD_PATH}:/opt/StaticJinjaPlus/build lypavel/static-jinja-plus:${TAG}
    ```
    `${TEMPLATES_PATH}` - путь к каталогу с вашими html-шаблонами<br>
    `${BUILD_PATH}` - путь к каталогу, где будут созданы отрендеренные HTML-страницы<br>
    `${TAG}` - тег образа

    Например:
    ```sh
    docker run --rm -v ./templates:/opt/StaticJinjaPlus/templates -v ./build:/opt/StaticJinjaPlus/build lypavel/static-jinja-plus:latest
    ```

## Создание образа на основе DockerFile

1. Скачайте код и перейдите в корень каталога.
2. Для всех версий, кроме `develop` и `develop-slim` необходимо узнать хэш-сумму
    ```sh
    curl -sL https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/${SJP_TAG}.tar.gz | sha256sum
    ```
    `${JSP_TAG}` - тег в репозитории [StaticJinjaPlus](https://github.com/MrDave/StaticJinjaPlus/tags).
3. Определитесь, какой Dockerfile вам нужен.
    * `0.1.X` или `latest` на основе `ubuntu`:
        ```
        ./0.1/ubuntu/Dockerfile
        ```
    * `develop` на основе `ubuntu`:
        ```
        ./develop/ubuntu/Dockerfile
        ```
    * `0.1.X` или `latest` на основе `python-slim`:
        ```
        ./0.1/python-slim/Dockerfile
        ```
    * `develop` на основе `python-slim`:
        ```
        ./develop/python-slim/Dockerfile
        ```
4. Создайте образ
    ```sh
    docker build -t ${docker_username}/${image_name}:${TAG} -f ${DOCKERFILE} . --build-arg SJP_TAG=${SJP_TAG} --build-arg SJP_HASH=${SJP_HASH}
    ```
    `${docker_username}` - имя пользователя Docker<br>
    `${image_name}` - имя образа<br>
    `${TAG}` - тег образа<br>
    `${DOCKERFILE}` - путь к Dockerfile<br>
    `${SJP_TAG}` - тег из репозитория [StaticJinjaPlus](https://github.com/MrDave/StaticJinjaPlus/tags)<br>
    `${SJP_HASH}` - хэш сумма<br>

### Примеры команд для создания разных образов
* На основе `ubuntu`
    * `develop`
    ```sh
    docker build -t lypavel/static-jinja-plus:develop -f develop/ubuntu/Dockerfile .
    ```
    * `latest`
    ```sh
    docker build -t lypavel/static-jinja-plus:latest -f 0.1/ubuntu/Dockerfile . --build-arg SJP_TAG=0.1.1 --build-arg SJP_HASH=30d9424df1eddb73912b0e2ad5375fa2c876c8e30906bec91952dfb75dcf220b
    ```
    * `0.1.1`
    ```sh
    docker build -t lypavel/static-jinja-plus:0.1.1 -f 0.1/ubuntu/Dockerfile . --build-arg SJP_TAG=0.1.1 --build-arg SJP_HASH=30d9424df1eddb73912b0e2ad5375fa2c876c8e30906bec91952dfb75dcf220b
    ```
    * `0.1.0`
    ```sh
    docker build -t lypavel/static-jinja-plus:0.1.0 -f 0.1/ubuntu/Dockerfile . --build-arg SJP_TAG=0.1.0 --build-arg SJP_HASH=3555bcfd670e134e8360ad934cb5bad1bbe2a7dad24ba7cafa0a3bb8b23c6444
    ```
* На основе `python-slim`
    * `develop-slim`
    ```sh
    docker build -t lypavel/static-jinja-plus:develop-slim -f develop/python-slim/Dockerfile .
    ```
    * `slim`
    ```sh
    docker build -t lypavel/static-jinja-plus:slim -f 0.1/python-slim/Dockerfile . --build-arg SJP_TAG=0.1.1 --build-arg SJP_HASH=30d9424df1eddb73912b0e2ad5375fa2c876c8e30906bec91952dfb75dcf220b
    ```
    * `0.1.1-slim`
    ```sh
    docker build -t lypavel/static-jinja-plus:0.1.1-slim -f 0.1/python-slim/Dockerfile . --build-arg SJP_TAG=0.1.1 --build-arg SJP_HASH=30d9424df1eddb73912b0e2ad5375fa2c876c8e30906bec91952dfb75dcf220b
    ```
    * `0.1.0-slim`
    ```sh
    docker build -t lypavel/static-jinja-plus:0.1.0-slim -f 0.1/python-slim/Dockerfile . --build-arg SJP_TAG=0.1.0 --build-arg SJP_HASH=3555bcfd670e134e8360ad934cb5bad1bbe2a7dad24ba7cafa0a3bb8b23c6444
    ```

***

Код написан в образовательных целях на онлайн-курсе для веб-разработчиков [Devman](dvmn.org).
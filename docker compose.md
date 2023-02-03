Docker Image - собственно, шаблон готового к запуску приложения со всем нужным для работы окружением.
контейнер \- это уже выполняющийся, запущенный образ, готовый к дальнейшему использованию.

Докер хаб \- реестр образов, где можно брать и загружать свои образы.

Разберем поля docker ps -a (или container ls -a)

CONTAINER ID - идентификатор контейнера, он уникален.

IMAGE - название образа, на основе которого запущен контейнер;

COMMAND - выполняемая при запуске контейнера команда;

CREATED - Когда был создан контейнер.

STATUS - статус контейнера, активен он или выключен.

PORTS - порты, через которые сервисы извне взаимодействует с контейнером и тем, что внутри него;

NAMES - имя контейнера.

Dockerfile - это обычный текстовый файл, содержащий набор инструкций для создания образа. Этот файл всегда должен называться Dockerfile и не иметь расширения.

FROM ubuntu

RUN apt update -y
RUN apt install fortune -y
RUN ln -s /usr/games/fortune /usr/bin/fortune

ENTRYPOINT \["fortune"\]

docker-compose - утилита для запуска мультиконтейнерных (т.е. состоящих из нескольких докер-контейнеров) приложений.
docker-compose.yml - файл, в котором хранится конфигурация(инструкция) запускаемого проекта docker-compose:

version: '3.1'
services:
db:
image: mariadb
restart: always
environment:
MYSQL\_ROOT\_PASSWORD: example
adminer:
image: adminer
restart: always
ports:
\- 8080:8080

В нём могут задаваться такие директивы:

version - это версия файла, в зависимости от которой используются разные ограничения, например, одна и та же команда может работать в зависимости от версий;

services - описание сервисов, которые мы будем использовать;

db, adminer - названия сервисов (но не контейнеров);

image - образ, из которого мы будем запускать контейнер;

restart - нужно ли перезапускать контейнер;

environment - здесь можно переопределить переменные окружения (опционально);

ports - какие порты со стороны контейнера и хоста использовать.

### Жизненный цикл Docker контейнера

Контейнеры Docker эфемерны, то есть они удаляются при удалении контейнера.

Их статус зависит от статуса приложений, которые они размещают.

Различные состояния включают в себя:

Создать контейнер  `$ docker create –name <container-name> <image-name>`

Запустить контейнер  `$ docker run -it -d –name <container-name> <image-name>`

Пауза контейнера `$ docker pause <container-id/name>`

Снять с паузы  `$ docker unpause <container-id/name>`

Стартануть контейнер  `$ docker start <container-id/name>`

Остановить контейнер  `$ docker stop <container-id/name>`

Перезапустить контейнер  `$ docker restart <container-id/name>`

Убить контейнер `$docker kill <container-id/name>`

Удалить / очистить контейнер  `$docker rm <container-id/name>`

### Разница между Docker run и Docker start

Run: создать новый контейнер из образа и выполнить его.

Вы можете создать N клонов одного образа.

> Команда: Docker run IMAGE\_ID, а не Docker run CONTAINER\_ID

Start: запуск контейнера, остановленного ранее.

Например, если вы остановили базу данных с помощью команды docker stop CONTAINER\_ID, вы можете перезапустить тот же контейнер с помощью команды docker start CONTAINER\_ID, и данные и настройки будут такими же.

### Команды Docker-compose

Конечно, его основная задача в том, чтобы управлять контейнерами и он содержит различные команды для управления всем жизненным циклом контейнеров.

Некоторые из команд включают в себя запуск, остановку, сборку представления и т.д.

Чтобы просмотреть эти команды и их действия, запустите их на своем терминале:

```
$ docker-compose --help
Define and run multi-container applications with Docker.

Usage:
  docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE             Specify an alternate compose file (default: docker-compose.yml)
  -p, --project-name NAME     Specify an alternate project name (default: directory name)
  --verbose                   Show more output
  --no-ansi                   Do not print ANSI control characters
  -v, --version               Print version and exit
  -H, --host HOST             Daemon socket to connect to

  --tls                       Use TLS; implied by --tlsverify
  --tlscacert CA_PATH         Trust certs signed only by this CA
  --tlscert CLIENT_CERT_PATH  Path to TLS certificate file
  --tlskey TLS_KEY_PATH       Path to TLS key file
  --tlsverify                 Use TLS and verify the remote
  --skip-hostname-check       Don't check the daemon's hostname against the name specified
                              in the client certificate (for example if your docker host
                              is an IP address)
  --project-directory PATH    Specify an alternate working directory
                              (default: the path of the Compose file)

Commands:
  build              Build or rebuild services
  bundle             Generate a Docker bundle from the Compose file
  config             Validate and view the Compose file
  create             Create services
  down               Stop and remove containers, networks, images, and volumes
  events             Receive real time events from containers
  exec               Execute a command in a running container
  help               Get help on a command
  images             List images
  kill               Kill containers
  logs               View output from containers
  pause              Pause services
  port               Print the public port for a port binding
  ps                 List containers
  pull               Pull service images
  push               Push service images
  restart            Restart services
  rm                 Remove stopped containers
  run                Run a one-off command
  scale              Set number of containers for a service
  start              Start services
  stop               Stop services
  top                Display the running processes
  unpause            Unpause services
  up                 Create and start containers
  version            Show the Docker-Compose version information
```

### Docker-compose run, up, exec – какая разница?

Что ж, когда я начал использовать docker, я заметил, что в руководствах часто использовались docker-compose up, docker-compose run и docker-compose exec, в основном без объяснений.

Я знаю, что вы можете просто прочитать разницу в документах командной справки, но иногда полезно полностью оценить разницу, копая немного глубже.

## Docker Compose Up

Эта команда используется, когда вы хотите запустить или вызвать все службы в вашем файле docker-compose.yml.

Файл Docker-compose.yml определяет ваши сервисы, их свойства, переменные и зависимости.

```
$ docker-compose up
```

Для просмотра запущенных контейнеров:

```
$ docker-compose ps
```

### Docker Compose Run

Эта команда раскрутит новый контейнер.

Он используется в основном тогда, когда вы хотите, чтобы новый контейнер и просто контейнер не работал, и являлся одноразовым процессом, который позволяет избежать конфликтов с другими сервисами в вашем docker-compose.yml, которые могут быть запущены.

```
$ docker-compose run zuri python manage.py migrate
```
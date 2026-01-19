# jenkins_simple_pipeline

## how to install Jenkins selfhosted
https://stepik.org/lesson/1987367/step/1?unit=2015278

Установка Jenkins локально
В этом уроке мы установим Jenkins на ваш компьютер и запустим его в Docker-контейнере. Это самый простой и безопасный способ начать работу с Jenkins.

Шаг 1: Установка Docker
Первым делом нам нужно установить Docker. Это платформа для запуска приложений в контейнерах.

Для Linux (Ubuntu/Debian/CentOS)
Самый простой способ — использовать официальный скрипт установки от Docker:

# Скачиваем и запускаем скрипт установки
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Добавляем текущего пользователя в группу docker
# (чтобы не использовать sudo каждый раз)
sudo usermod -aG docker $USER

# Перелогиньтесь или выполните команду для применения изменений
newgrp docker

# Проверяем, что Docker установлен
docker --version

                  
Вы должны увидеть версию Docker, например: Docker version 24.0.7

Для macOS
Скачайте и установите Docker Desktop с официального сайта:

Перейдите на https://www.docker.com/products/docker-desktop
Скачайте версию для macOS (Apple Silicon или Intel)
Установите приложение и запустите его
Дождитесь, пока Docker полностью запустится (иконка в трее перестанет мигать)
Проверьте установку в терминале:

docker --version

                  
Для Windows
Установите Docker Desktop для Windows:

Перейдите на https://www.docker.com/products/docker-desktop
Скачайте установщик для Windows
Запустите установщик и следуйте инструкциям
После установки перезагрузите компьютер
Запустите Docker Desktop
Откройте PowerShell или командную строку и проверьте:

docker --version

                  
Шаг 2: Запуск Jenkins в Docker
Теперь, когда Docker установлен, запустим Jenkins одной командой!

Создаём том для данных Jenkins
Сначала создадим Docker volume для хранения данных Jenkins (конфигурация, плагины, задания). Это нужно, чтобы данные не потерялись при перезапуске контейнера:

docker volume create jenkins_home

                  
Запускаем Jenkins
Выполните следующую команду:

docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts

                  
Для Windows (PowerShell):

docker run -d `
  --name jenkins `
  -p 8080:8080 `
  -p 50000:50000 `
  -v jenkins_home:/var/jenkins_home `
  jenkins/jenkins:lts

                  
Разберём, что означает эта команда:

-d — запускаем контейнер в фоновом режиме
--name jenkins — даём контейнеру понятное имя
-p 8080:8080 — пробрасываем порт 8080 (веб-интерфейс Jenkins)
-p 50000:50000 — порт для подключения агентов
-v jenkins_home:/var/jenkins_home — монтируем том для данных
-v /var/run/docker.sock:/var/run/docker.sock — даём доступ к Docker (для Linux/Mac)
jenkins/jenkins:lts — используем официальный образ Jenkins LTS (Long Term Support)
Шаг 3: Получение начального пароля
Jenkins создаёт случайный пароль администратора при первом запуске. Нам нужно его получить.

Подождите примерно 30-60 секунд, пока Jenkins полностью запустится, затем выполните:

docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword

                  
Вы увидите строку вроде:

f8a4e2b9c1d3a5e7f9b2c4d6e8f0a1b3

                  
Скопируйте этот пароль — он понадобится на следующем шаге!

Шаг 4: Первый запуск Jenkins
Откройте браузер и перейдите по адресу:

http://localhost:8080

                  
Вы увидите страницу "Unlock Jenkins":

Вставьте скопированный пароль в поле "Administrator password"
Нажмите "Continue"
Шаг 5: Установка плагинов
Jenkins предложит установить плагины. Вы увидите два варианта:

Install suggested plugins — рекомендуемые плагины (выбирайте это)
Select plugins to install — ручной выбор плагинов
Нажмите "Install suggested plugins". Jenkins автоматически установит самые популярные плагины:

Git — интеграция с Git-репозиториями
Pipeline — для создания CI/CD пайплайнов
GitHub — интеграция с GitHub
Docker — работа с Docker-контейнерами
И многие другие полезные инструменты
Установка займёт 2-5 минут. Подождите, пока все плагины установятся.

Шаг 6: Создание первого пользователя
После установки плагинов Jenkins попросит создать учётную запись администратора:

Username — ваш логин (например: admin)
Password — надёжный пароль
Confirm password — повторите пароль
Full name — ваше имя
Email — ваш email
Нажмите "Save and Continue".

Шаг 7: Настройка URL
Jenkins покажет URL, по которому он будет доступен:

http://localhost:8080/

                  
Оставьте как есть и нажмите "Save and Finish".

Шаг 8: Готово!
Поздравляем! Jenkins установлен и готов к работе. Нажмите "Start using Jenkins" — вы попадёте на главную страницу.

Полезные команды Docker
Вот команды, которые вам пригодятся при работе с Jenkins в Docker:

# Остановить Jenkins
docker stop jenkins

# Запустить Jenkins снова
docker start jenkins

# Перезапустить Jenkins
docker restart jenkins

# Посмотреть логи Jenkins
docker logs jenkins

# Посмотреть логи в реальном времени
docker logs -f jenkins

# Удалить контейнер Jenkins (данные сохранятся в volume)
docker rm -f jenkins

# Удалить контейнер И все данные
docker rm -f jenkins
docker volume rm jenkins_home

                  
Что делать, если Jenkins не запустился?
Если что-то пошло не так, проверьте логи:

docker logs jenkins

                  
Типичные проблемы:

Порт 8080 уже занят
Измените порт в команде запуска:

docker run -d --name jenkins -p 9090:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts

                  
Теперь Jenkins будет доступен на http://localhost:9090

Docker не запущен
Убедитесь, что Docker Desktop запущен (для macOS/Windows) или Docker daemon работает (для Linux):

docker ps

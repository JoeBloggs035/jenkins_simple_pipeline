# Часть 1: Запуск пайплайнов в Jenkins
Шаг 1: Создание Pipeline Job
Сначала нам нужно создать задание (Job) в Jenkins, которое будет выполнять ваш пайплайн.

Откройте Jenkins в браузере: http://localhost:8080
Войдите с вашими учётными данными
На главной странице нажмите "New Item" (или "Создать Item")
В открывшемся окне:

Введите имя проекта, например: my-first-pipeline
Выберите "Pipeline"
Нажмите "OK"
Шаг 2: Настройка Pipeline
Теперь настроим наш пайплайн:

Прокрутите страницу вниз до раздела "Pipeline"

В поле "Definition" выберите:

"Pipeline script" — если хотите вставить код прямо здесь
"Pipeline script from SCM" — если код в Git-репозитории
Вариант A: Вставляем код напрямую
Если выбрали "Pipeline script":

В большом текстовом поле вставьте ваш Jenkinsfile. Например:

pipeline {
    agent any
    
    stages {
        stage('Hello') {
            steps {
                echo 'Привет! Это мой первый пайплайн!'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Собираем проект...'
                sh 'echo "Build completed"'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Запускаем тесты...'
                sh 'echo "All tests passed"'
            }
        }
    }
}

                  
Нажмите "Save" внизу страницы
Вариант B: Берём код из Git
Если выбрали "Pipeline script from SCM":

В поле "SCM" выберите "Git"

Заполните данные репозитория:

Repository URL: адрес вашего Git-репозитория (например: https://github.com/username/repo.git)
Credentials: оставьте "none" для публичных репозиториев
Branch: укажите ветку, например */main или */master
В поле "Script Path" укажите путь к Jenkinsfile:

По умолчанию: Jenkinsfile
Или свой путь: .jenkins/Jenkinsfile
Нажмите "Save"

Шаг 3: Первый запуск
Теперь самое интересное — запускаем пайплайн:

После сохранения вы попадёте на страницу проекта
Нажмите "Build Now" (или "Собрать сейчас") в левом меню
Через секунду в разделе "Build History" появится #1
Нажмите на #1, чтобы открыть детали сборки
Шаг 4: Просмотр результатов
Есть несколько способов посмотреть, что происходит:

Способ 1: Console Output (Лог выполнения)
На странице сборки нажмите "Console Output"
Вы увидите полный лог выполнения пайплайна
Здесь будут все команды и их результаты
Способ 2: Pipeline Visualization (Визуализация)
Вернитесь на страницу проекта
Вы увидите графическое представление стадий
Зелёные блоки — успешные стадии
Красные — с ошибками
Нажмите на любую стадию, чтобы увидеть её лог
Полезные команды для Jenkins
# Перезапустить Jenkins (если что-то сломалось)
docker restart jenkins

# Посмотреть логи Jenkins
docker logs -f jenkins

# Войти в контейнер Jenkins
docker exec -it jenkins bash

# Проверить версию Jenkins
docker exec jenkins cat /var/jenkins_home/jenkins.version


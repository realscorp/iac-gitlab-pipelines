# Что это?
Общий репозиторий пайплайнов для всего инфраструктурного кода.  
Пайплайны должны быть написаны так, чтобы обеспечивалась модульность: отдельные файлы под каждый инструмент (Terraform, Ansible, Packer, Docker etc), и отдельные файлы под каждое окружение. Так разработчик инфраструктурного кода сможет самостоятельно подключить в свой проект необходимый и достаточный набор инструментов и окружений.
# Описание структуры
- **init.yml** - один большой список джобов для всех типов кода: список стадий, инициализация переменных, подготовка ssh в докер-раннере и языко-специфичные джобы
    - **OS flavors** - выводит список флейворов в облачном проекте для конкретного окружения
    - **OS images** - выводит список образов в облачном проекте для конкретного окружения
    - **TF validate** - инициализирует Terraform-провайдеры и проверяет код встроенным линтером на валидность
    - **TF plan** - выполняет и показывает пользователю Terraform plan, с сохранением его в кэш для следующей джобы
    - **TF apply** - применение сгенерированного на предыдущем шаге плана
    - **TF destroy** - уничтожение всей описанной в коде инфраструктуры (только для окружения PLAYGROUND)
    - **TF unlock** - "ручное" удаление блокировки на стейт-файле в случае, если apply джоба была прервана до автоматического снятия блокировки
    - **TF show** - показать содержимое стейт-файла (terraform show)
    - **ANS validate** - инициализирует Ansible (скачивает коллекции, роли) и проверяет код линтером на валидность
    - **ANS apply** - выполняет Ansible-плейбук в соотвествующем окружении
- **ansible-apply-auto.yml** - автоматический запуск **ansible-apply** из расписания Гитлаба. Окружение задаётся через переменную **project** в настройке расписания. Подключать **env-**файлы не нужно.
    - **ANS apply AUTO** - автоматический запуск на исполнение кода Ansible в окружении, заданном через переменную **project**
- **env-prod.yml** - настройки джобов для окружения PROD (основной боевой облачный проект)
- **env-devqa.yml** - настройки джобов для окружения DEVQA (основной проект для тестовых сервисов и сервисов для разработчиков)
- **env-playground.yml** - настройки джобов для окружения PLAYGROUND (проект-песочница). В наших условиях добавление этого пайплайна позволит легко развернуть для проверки боевой код в песочнице.
# Примеры использования
См. [шаблон](https://gitlab.example.com/infrastructureascode/lib/iac-template)
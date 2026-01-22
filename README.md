
Домашнее задание по теме "Введение в Terraform" - Машаев Роман

Задание 1

Задание 1
1. Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.
2. Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токен
ы итд)
3. Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный
 ключ и его значение.
4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate. Объясните,
 в чём заключаются намеренно допущенные ошибки. Исправьте их.
5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps.
6. Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа. 
Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду terraform apply -auto-approve. Объясните своими словами,
в чём может быть опасность применения ключа -auto-approve.
Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно
приложите вывод команды docker ps.
7. Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.
8. Объясните, почему при этом не был удалён docker-образ nginx:latest. Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, 
а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker. (ищите в классификаторе resource docker_image )


ОТВЕТ:
1.,2.![Скриншот с выводом версии терраформа](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-19_15_21_23.png?raw=true) 

Согласно предоставленному файлу .gitignore, личную и секретную информацию (логины, пароли, ключи, токены и т.д.) допустимо сохранять в файле с именем:
personal.auto.tfvars. Эта запись означает, что файл с точным именем personal.auto.tfvars будет игнорироваться системой контроля версий Git 
(не будет добавлен в коммиты).Файлы с расширением .auto.tfvars (или *.auto.tfvars.json) автоматически загружаются Terraform при выполнении
команд terraform plan или terraform apply.Это стандартный способ определения значений переменных без необходимости каждый раз передавать 
их через командную строку или другие методы. Поскольку этот файл указан в .gitignore, ваши секреты не попадут в репозиторий. 
Файл останется только на локальной машине или в защищенном хранилище CI/CD системы (куда его нужно добавить отдельно, безопасным способом).

3. result: yUXm97Ujhv4im1X1

4. ![Скриншот с выводом ошибок после](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-19_16_02_58.png?raw=true)
   ![Скриншот с выводом ошибок после](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-19_16_32_37.png?raw=true)
- Ошибка 1: Missing name for resource. У ресурса docker_image отсутствует локальное имя (второй идентификатор). 
В Terraform каждый ресурс должен иметь формат: resource "ТИП_РЕСУРСА" "ЛОКАЛЬНОЕ_ИМЯ" { ... }. Напрмер: resource "docker_image" "nginx"
- Ошибка 2: Invalid resource name. Локальное имя ресурса "1nginx" начинается с цифры 1. По правилам Terraform имена ресурсов могут начинаться 
только с буквы или подчёркивания. Правильно будет, например: resource "docker_container" "nginx" { ... }
- Ошибка 3: Неверная ссылка на random_password. random_string_FAKE, а  должно быть random_string (как определено в коде). Кроме того resulT, а
 должно быть result (регистр имеет значение).

5.![Скриншот результата работы кода](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_00_23_06.png?raw=true)
  ![Скриншот результата работы кода](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_00_24_25.png?raw=true)
  ![Скриншот результата работы кода](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_00_24_49.png?raw=true)
  ![Скриншот фрагмента исправленного кода main.tf](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_00_45_06.png?raw=true) 

6. ![Скриншот результата работы кода](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_00_36_36.png?raw=true)
   ![Скриншот результата работы кода](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_00_37_57.png?raw=true)

Опасности использования -auto-approve:
- Отсутствие проверки изменений.  Terraform применяет план без возможности предварительно просмотреть и проверить изменения.
- Риск потери данных. Если в плане есть удаление ресурсов (особенно с данными), они будут удалены без подтверждения.
- Невозможность отменить ошибочную конфигурацию. Например, если случайно указал неверный размер диска или конфигурацию.
- Автоматическое применение ошибок. Ошибки в коде (неправильные IP-адреса, порты) сразу применяются к инфраструктуре.
- Финансовые риски. В облачных средах могут создаваться дорогостоящие ресурсы без контроля.
- Безопасность.  Если в коде есть уязвимости (открытые порты, слабые пароли), они сразу внедряются.

 Когда полезен -auto-approve:
- CI/CD пайплайны. В автоматизированных процессах сборки и развертывания.
- Тестовые среды. При частых пересозданиях тестовой инфраструктуры.
- Скрипты автоматизации. Когда Terraform вызывается из других скриптов.
- Демонстрации и обучение. Для упрощения показа работы Terraform.

В нашем же случае Terraform обновил существующий контейнер (не пересоздал). Изменилось только имя: example_yUXm97Ujhv4im1X1 на  hello_world. 
Контейнер продолжает работать без перезапуска сервиса. Порт 9090 по-прежнему проброшен на порт 80 контейнера.

7. Созданные ресурсы удалаем с помощью команды terraform destroy.
   ![Скриншот результата выполнения команды](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_01_09_25.png?raw=true)
   ![Скриншот результата выполнения команды](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_01_10_07.png?raw=true)
   ![Скриншот содержимого файла terraform.tfstate](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_01_10_51.png?raw=true)

В результате вполнения команды terraform destroy 3 ресурса успешно удалены:random_password.random_string, docker_container.nginx, docker_image.nginx.
Несмотря на пустое состояние, файл terraform.tfstate всё ещё существует и содержит метаданные (версию, serial, lineage). Это нужно для отслеживания истории изменений.

8. Образ nginx:latest не был удалён с локальной машины при выполнении terraform destroy из-за параметра keep_locally = true, указанного в ресурсе  docker_image в файле main.tf.
 Согласно официальной документации провайдера kreuzwerker/docker для ресурса docker_image: keep_locally - (Boolean) If true, then the Docker image won't be deleted on destroy operation. 
If this is false, it will delete the image from the docker local storage on destroy operation.
   ![Скриншот из документации провайдера](https://github.com/Mazaich/terraform_homework/blob/main/2026-01-23_01-25-12.png?raw=true)
При keep_locally = false (значение по умолчанию) - Terraform удаляет образ из локального хранилища Docker при уничтожении ресурса. 
При keep_locally = true (как в нашем случае) - Terraform прекращает управление образом, но оставляет его физически на диске.
 


Задание 3*
Установите opentofu(fork terraform с лицензией Mozilla Public License, version 2.0) любой версии
Попробуйте выполнить тот же код с помощью tofu apply, а не terraform apply.


Ответ:

![Скриншот результата выполнения команды](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_02_53_59.png?raw=true)
![Скриншот результата выполнения команды](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_02_48_11.png?raw=true)
![Скриншот результата выполнения команды](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_02_48_41.png?raw=true)
![Скриншот результата выполнения команды](https://github.com/Mazaich/terraform_homework/blob/main/Screenshot_2026-01-23_02_49_35.png?raw=true)


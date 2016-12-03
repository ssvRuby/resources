Подключение проекта Ruby on Rails к базе данных ORACLE
======================================================

- На  сервере IP 10.0.0.11 (операционная система Oracle Linux 6) развернут ORACLE 11g (11.2.0.4). Имя сервера в DNS не прописано. На сервере развернута учебная база данных HR, соответственно, пользователь hr, password - hr. Содержимое файла tnsnames.ora:

        demo =
         (DESCRIPTION =
           (ADDRESS_LIST =
             (ADDRESS = (PROTOCOL = TCP)(HOST = demo.virtual.box)(PORT = 1521))
           )
           (CONNECT_DATA =
             (SERVICE_NAME = demo.virtual.box)
           )
         )

    Имя сервиса demo.virtual.box, не совсем удобно (в строках подключения необходимо брать в кавычки), но, как есть.

- На ноутбуке (MacBook Air, macOS Sierra 10.12.1) развернут ruby on Rails 5.0.

- Задача: создать RoR-проект, который будет взаимодействовать с базой данных Oracle. Для этого необходимо подготовить рабочую среду RoR для подключения к базе Oracle.

1. Необходимо убедиться, что в файле etc/hosts прописано имя хоста (то, которое выдается командой hostname)
                
         > 127.0.0.1      localhost      hostname
         
    (если этого не сделать, в дальнейшем получим падение клиета Oracle c ошибкой ORA-21561: OID generation failed)

5. Установить Oracle Instant Client Packages. Устанавливаем с помощью [Homebrew](http://brew.sh/)
  Для этого скачать [отсюда](http://www.oracle.com/technetwork/topics/intel-macsoft-096467.html) (Версия 12.1.0.2.0  была последней на момент проведения этого эксперимента. Прекрасно работает с сервером Oracle версии 11g)
 Файлы не распаковывать, положить в папку  $HOME/Library/Caches/Homebrew
                 
          instantclient-basic-macos.x64-12.1.0.2.0.zip
          instantclient-sdk-macos.x64-12.1.0.2.0.zip
          instantclient-sdk-macos.x64-12.1.0.2.0.zip
       
6. Выполнить следующие команды:

          brew tap InstantClientTap/instantclient
          brew install instantclient-basic
          brew install instantclient-sdk
          brew install instantclient-sqlplus
             
7. Установить переменную окружения OCI_DIR

          export OCI_DIR=$(brew --prefix)/lib

8. Установить RUBY-gem ruby-oci8

          gem install ruby-oci8 

9. Запускаем генерацию нового проекта RoR:

          rails new ora_project --database=oracle

10. В Gemfile проекта проверяем наличие строк

          # Use oracle as the database for Active Record
          gem 'ruby-oci8'

        и добавляем ссылку на gem oracle-адаптера:
        ```ruby       
        gem 'activerecord-oracle_enhanced-adapter', '~> 1.7.0'
        ```  
11.  
      bundle install

12. Вносим изменения в файл database.yml в части определения параметров подключения. В данном случае это
       выглядит так:

            default: &default
              adapter: oracle_enhanced
              database: //10.0.0.11:1521/"demo.virtual.box"
              username: hr
              password: hr
   - В документации к [activerecord-oracle_enhanced-adapter](https://github.com/rsim/oracle-enhanced) описаны еще
    много различных вариантов описания параметров подключения:

                adapter: oracle_enhanced
                database: "(DESCRIPTION=
                           (ADDRESS_LIST=(ADDRESS=(PROTOCOL=tcp)(HOST=localhost)(PORT=1521)))
                           (CONNECT_DATA=(SERVICE_NAME=xe))
                           )"
                  username: user
                  password: secret

                или

                  adapter: oracle_enhanced
                  database: //localhost:1521/xe
                  username: user
                  password: secret

                или
                  adapter: oracle_enhanced
                  host: localhost
                  port: 1521
                  database: xe
                  username: user
                  password: secret

                и т.д.
                        

13.  Egentligen, на этом все. Генерим миграции, и так далее. Начинаем получать счастье от общения
        с базой Oracle.
        
  Ссылки
  ===============================================================================      
- [activerecord-oracle_enhanced-adapter](https://github.com/rsim/oracle-enhanced)
- [Install ruby-oci8 on OS X](https://github.com/kubo/ruby-oci8/blob/master/docs/install-on-osx.md)
- [Instant Client Downloads for Mac OS X (Intel x86)](http://www.oracle.com/technetwork/topics/intel-macsoft-096467.html)
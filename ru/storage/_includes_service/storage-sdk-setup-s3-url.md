Для настройки создайте в домашнем каталоге файлы конфигурации и задайте в них:

- Регион по умолчанию в файле `.aws/config`:
   ```
   [default]
               region=ru-central1
   ```
   {% note info %}

   Некоторые приложения, предназначенные для работы с Amazon S3, не позволяют указывать регион, поэтому {{ objstorage-name }} принимает также значение `us-east-1`.

   {% endnote %}

   Для доступа к {{ objstorage-name }} используйте адрес `s3.yandexcloud.net`.
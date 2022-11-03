1. Перейти в интерфейс {{ yq-full-name }} в раздел "Connections" и нажать кнопку "Create".
2. В открывшемся окне в поле `Name` указать название подключения к {{ objstorage-full-name }}.
3. В выпадающем поле `Type` выбрать `Object Storage`.
4. В поле `Bucket auth` выбрать `Public` или `Private`. `Public` означает, что бакет, откуда производится чтение, не защищен авторизацией. `Private` означает, что необходимо использовать данные, указанные в поле `Service Account`, при обращении к бакету.
5. В поле `Service account` выбрать сервисный аккаунт, который будет использоваться для чтения данных, или создать новый, выдав ему права [`storage.viewer`](../../storage/security/index.md).
6. Создать подключение, нажав кнопку `Create`.
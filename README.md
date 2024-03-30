# Профильное задание на позицию стажера "Back-End Developer"
## _Стажировка Вконтакте_
Условия задачи:  
Необходимо разработать упрощенный сервис с регистрацией и авторизацией.
Хранить данные можно в любой реляционной СУБД.

Сервис должен иметь 3 апи-метода (REST):
1. /register
    - Входные параметры: string email, string password.
    - Необходимо проверить, что пользователя с таким email еще нет в базе и email валидный.
    - Также нужно добавить проверку надежности пароля - если пароль легко подобрать, выкидывать ошибку weak_password.
    - На выходе возвращать int user_id, string password_check_status (good, perfect).
2. /authorize
    - Входные параметры: string email, string password.
    - В ответе - _string acces_token JWT_, в котором будет лежать user_id.
3. /feed
    - Идем в метод с access_token.
    - Возвращаем 200 код, если токен валиден, и выкидываем ошибку unauthorized, если нет.
      
***
## _Инструкция_
Установка NuGet пакетов:
   ```
   dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
   dotnet add package Microsoft.EntityFrameworkCore.Tools
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   ```
В качестве СУБД был выбран локальный MSSQL Server, поэтому Вам придётся осуществить настройку базы данных самостоятельно. Инструкция ниже.

Для успешной миграции и создания базы данных требуется:
1. Открыть PowerShell в директории проекта
2. Осуществить миграцию базы данных с помощью консольной команды:
   ```
   Add-Migration InitialCreate
   ```
3. Применить миграцию с помощью консольной команды:
   ```
   Update-Database
   ```
***
## _Примеры запросов_
В качестве приложения для тестирования API-запросов был выбран Postman.  

1. Регистрация нового пользователя
   - Метод: POST
   - URL: ```http://localhost:port/register```
   - Тело запроса (JSON):
     ```json
        {
          "Email": "example@example.com",
          "Password": "password123"
        }
     ```
2. Аутентификация пользователя
   - Метод: POST
   - URL: ```http://localhost:port/authorize```
   - Тело запроса (JSON):
     ```json
        {
          "Email": "example@example.com",
          "Password": "password123"
        }
     ```
3. Проверка авторизации
   - Метод: GET
   - URL: ```http://localhost:port/feed```
   - Заголовок Authorization: Bearer {AccessToken}
   - {AccessToken} - токен доступа, полученный после аутентификации.

## Очереди

Если в БД не существует таблицы ```jobs```, то создаём её
```
php artisan queue:table
php artisan migrate
```

Создаём задание 
```
php artisan make:job SendBillEmailJob
```

В методе ```handle``` прописываем внедрение зависимости нужного сервиса (если это необходимо) и описываем логику
```php
public function handle(MailerServiceContract $mailerService)
{
    $mailerService->sendTestMail();
}
```

В контроллере вызываем задание
```php
use App\Jobs\SendBillEmailJob;

public function test()
{
    SendBillEmailJob::dispatch();
}
```

После того, как событие сработает, задание добавится в очередь в таблице ```jobs``` в БД.
Чтобы инициализировать выполнение всех заданий, необходимо выполнить команду
```
php artisan queue:work
```

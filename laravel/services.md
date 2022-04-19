## Сервисы, сервис-провайдеры

### Порядок создания сервиса, который со временем может смениться на другой (с контрактом или интерфейсом)

Создаём контракт (интерфейс) для сервиса ```App/Contracts/MailerServiceContract.php```
```php
namespace App\Contracts;

interface MailerServiceContract
{
    ...
}
```

Создаем сервис ```App/Service/Mailer/SendPulseService.php```
```php
namespace App\Services\Mailer;

class SendPulseService implements MailerServiceContract
{
    ...
}
```

Создаём сервис-провайдер
```console
php artisan make:provider TesterServiceProvider
```
В сервис-провайдере регистрируем наш класс сервиса
```php
public function register()
{
    $this->app->bind(MailerServiceContract::class, function() {
        return new \App\Services\Mailer\SendPulseService;
    });
}
```

Теперь в контроллере можно использовать внедрение зависимости
```php
use App\Contracts\MailerServiceContract;

class MailController extends Controller
{
    protected object $mailerService;

    public function __construct(MailerServiceContract $mailerService)
    {
        $this->mailerService = $mailerService;
    }

    ...
}
```

В последствии мы можем создать другой сервис (напр. AnotherMailerService) и зарегистрировать его в сервис-провайдере, либо использовать условия, при которых будут регистрироваться различные сервисы.

### Можно создавать сервисы без контрактов, тогда в сервис-провайдере регистрация будет такой:
```php
use \App\Services\Mailer\SendPulseService;

public function register()
{
    $this->app->bind(SendPulseService::class, function() {
        return new SendPulseService();
    });
}
```

### Также можно не создавать сервис-провайдер, а напрямую внедрять сервис в контроллер
```php
use App\Services\SomeService;

class SomeController extends Controller
{
    protected object $service;

    public function __construct(SomeService $service)
    {
        $this->service = $mailerService;
    }

    ...
}
```

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

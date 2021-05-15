# Interface localization
### Example: from English to Russian

<!-- yii2--localization.md -->

## Yii2 Advanced

### Change config
update file<br/>
`frontend/config/main.php`
```php
...
return [
...
    'language' => 'ru-RU',
...
```

### Add vocabulary
create and fill file<br/>
`frontend/messages/ru-RU/app.php`
```php
<?php

/* 
 * localization
 */

return [
    'Home'=>'Главная',
    'ID'=>'ID',
    'Uid'=>'Uid',
    'Time'=>'Time',
    'Company'=>'Компания',
    'Rating'=>'Рейтинг',
    'Service'=>'Услуга',
    'Ip'=>'Ip',
    'Name'=>'Ваше имя',
    'Date'=>'Дата оказания услуги',
    'Adres'=>'Адрес',
    'Dignity'=>'Достоинства компании',
    'Flaw'=>'Недостатки компании',
    'Create'=>'Отправить',
    'Update'=>'Обновить',
    'Submit'=>'СОХРАНИТЬ',
    'Delete'=>'Удалить',
    'Login'=>'Войти',
    'Signup'=>'Регистрация',
    'Logout'=>'Выйти',
    'Tests'=>'Тесты',
    
];
```

### Add error vocabulary
create and fill file<br/>
`frontend/messages/ru-RU/app/error.php`
```php
<?php

/* 
 * localization
 * errors
 */
return [
    // ?
];
```

### Usage
Menu on header by example.<br/>
Open and update file<br/>
`frontend/views/layouts/main.php`
```php
...
    $menuItems = [
        ['label' => Yii::t('app', 'Home'), 'url' => ['/site/index']],
        ['label' => Yii::t('app', 'About'), 'url' => ['/site/about']],
        ['label' => Yii::t('app', 'Contact'), 'url' => ['/site/contact']],
        ['label' => Yii::t('app', 'Tests'), 'url' => ['/site/tests']],
    ];
    if (Yii::$app->user->isGuest) {
        $menuItems[] = ['label' => Yii::t('app', 'Signup'), 'url' => ['/site/signup']];
        $menuItems[] = ['label' => Yii::t('app', 'Login'), 'url' => ['/site/login']];
    } else {
...
```


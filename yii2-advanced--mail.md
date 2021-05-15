# Yii2-advanced Mail

<!-- yii2-advanced--mail.md -->

Wey sendig emails or save its to files,<br/>
defined by paremeter:<br/>
`yii\mail\BaseMailer::useFileTransport`<br/>
Path in which files will be saved<br/>
defined by parameter:<br/>
yii\mail\BaseMailer::fileTransportPath<br/>

## Emailing On
For activate sendig emails,<br/>
open config file:<br/>
`common/config/main-local.php`<br/>
and chenge paramet `useFileTransport` to `false`:
```php
...
 'useFileTransport' => false,
...
```

If you want to test emails and caught its on files<br/>
chenge paramet `useFileTransport` to `true`:
```php
...
 'useFileTransport' => true,
...
```

## Templates
Templates of email located at
`common/mail`
or
`@app/mail`

## Test email location
Basicly, sended emails save to a files.<br/>
Defaultly its located in dir:<br/>
`frontend/runtime/mail`<br/>
or<br/>
`@runtime/mail`

## Send example
```php

Yii::$app->mailer->compose([
    'html' => 'contact-html', // tpl html mail view
    'text' => 'contact-text', // tpl text mail view
])
    ->setFrom('from@domain.com')
    ->setTo('to@domain.com')
    ->setSubject('Message subject')
    ->send();
```

More information you can found here:
[Email sending](https://yiiframework.com.ua/ru/doc/guide/2/tutorial-mailing/)
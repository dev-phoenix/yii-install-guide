# yii2 advanced based steps
#yii2-advanced--based-steps.md
<!-- yii2-advanced--based-steps.md -->

## Create tables
### create tables migrate files
first step is planing db structure.<br/>
second step is create files for tables migration.<br/>
Type in root dir of your site:<br/>
```
cd <root-dir-of-your-site>
php yii migrate/create create_orders_table
php yii migrate/create create_payments_table

php yii migrate
```

maybe you will need next commans:
```
yii migrate/to m180108_093530_create_reviews_table
yii migrate/down m180108_093530_create_reviews_table
yii migrate/redo
yii migrate/redo 23
php yii migrate/history all
```
you can find description of comands `to, down, redo, redo <num>, history all` in google

if you caught error like:
```
... Your Composer dependencies require a PHP version ">= 7.2.0". You are running 5.4.37. ...
```
but you shure that you alweys have needed version of PHP<br/>
you may try found location where it located<br/>
and type your bash comand some like this:
```
/opt/php74/bin/php yii migrate
```
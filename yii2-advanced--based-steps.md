# yii2 advanced based steps
#yii2-advanced--based-steps.md
<!-- yii2-advanced--based-steps.md -->

## Create tables
### create tables migrate files
first step is planing db structure.
second step is create files for tables migration.
Type in root dir of your site:
```
cd <root-dir-of-your-site>
php yii migrate/create drop_reviews_table

php yii migrate
```

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
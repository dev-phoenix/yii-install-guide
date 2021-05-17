# Yii2-advanced. Migration

<!-- yii2-advanced--migration.md -->

## Create migration script
```
php yii migration/create create_<table-name>_table # create migration for table
```

After update migration script,<br/>
you can apply it
```
php yii migrate # use next all migrations
php yii migrate 3 # use next 3 migrations
php yii migrate/to m150101_185401_create_news_table # add only this migration
php yii migrate/down 3 # unset last 3 migrations
php yii migrate/redo 3 # restart last 3 migrations
php yii migrate/history all # show all migrations

yii migrate/new         # show 10 new migrations
yii migrate/new 5       # show  5 new migrations
yii migrate/new all     # show all new migrations
```
## More information:
[Database Migration](https://www.yiiframework.com/doc/guide/2.0/en/db-migrations)
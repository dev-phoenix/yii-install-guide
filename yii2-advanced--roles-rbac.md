# Roles. RBAC access system.

<!-- yii2-advanced--roles-rbac.md -->

## Turn ON 

change file:<br/>
`<your-site-dir>/common/config/main.php`<br/>
change section components:
```php
return [
    ...
    'components' => [
        ...
        'authManager' => [
//            'class' => 'yii\rbac\DbManager', // if you whont safe settigns in db
            'class' => 'yii\rbac\PhpManager',  // if you whont save settings in file
            // role which add for all users by default
            // here write needed roles
            'defaultRoles' => ['user','company','manager','chief','admin'],
            //set where will save RBAC config files
            'itemFile' => '@common/components/rbac/items.php',
            'assignmentFile' => '@common/components/rbac/assignments.php',
            'ruleFile' => '@common/components/rbac/rules.php'
        ],
    ],
    ...
```

if you whant use DbManager
you shell need uncoment row:
```php
            'class' => 'yii\rbac\DbManager', // if you whont safe settigns in db
```
and to commet next rows in this section.<br/>
Than open bash console and type:
```
php yii migrate --migrationPath=@yii/rbac/migrations
```
you will have new structure in db(database) after this.<br/>
<br/>
Now we try create role access system, based on files data.

## RBAC Controller console app
Add and change file<br/>
`<your-site-dir>/console/controllers/RbacController.php`
```php
<?php
namespace console\controllers;

use Yii;
use yii\console\Controller;
use common\components\rbac\UserRoleRule;

class RbacController extends Controller
{
    public function actionInit()
    {
        $auth = Yii::$app->authManager;
        $auth->removeAll(); //удаляем старые данные

        //Создадим для примера права для доступа к админке
        $dashboard = $auth->createPermission('adminPanel');
        $dashboard->description = 'Админ панель';
        $auth->add($dashboard);

        //Добавляем роли
        $user = $auth->createRole('user');
        $user->description = 'Пользователь';
        $auth->add($user);

        $moder = $auth->createRole('moder');
        $moder->description = 'Модератор';
        $auth->add($moder);

        //Добавляем потомков
        $auth->addChild($moder, $user);
        $auth->addChild($moder, $dashboard);

        $admin = $auth->createRole('admin');
        $admin->description = 'Администратор';
        $auth->add($admin);
        $auth->addChild($admin, $moder);
    }
}
```

## Roles Controller console app
Add and change file<br/>
`<your-site-dir>/console/controllers/RolesController.php`
```php
<?php

namespace console\controllers;

use common\models\User;
use Yii;
use yii\console\Controller;
use yii\console\Exception;
use yii\helpers\ArrayHelper;


class RolesController extends Controller
{

    public function actionAssign()
    {
        $username = $this->prompt('Username:', ['required' => true]);
        $user = $this->findModel($username);
        $roleName = $this->select('Role:', ArrayHelper::map(Yii::$app->authManager->getRoles(), 'name', 'description'));

        $authManager = Yii::$app->getAuthManager();
        $role = $authManager->getRole($roleName);
        
        $authManager->assign($role, $user->id);

        $this->stdout('Done!' . PHP_EOL);
    }


    public function actionRevoke()
    {
        $username = $this->prompt('Username:', ['required' => true]);
        $user = $this->findModel($username);
        $roleName = $this->select('Role:', ArrayHelper::merge(
            ['all' => 'All Roles'],
            ArrayHelper::map(Yii::$app->authManager->getRolesByUser($user->id), 'name', 'description'))
        );
        $authManager = Yii::$app->getAuthManager();

        if ($roleName == 'all') {
            $authManager->revokeAll($user->id);
        } else {
            $role = $authManager->getRole($roleName);
            $authManager->revoke($role, $user->id);
        }
        $this->stdout('Done!' . PHP_EOL);
    }


    private function findModel($username)
    {
        if (!$model = User::findOne(['username' => $username])) {
            throw new Exception('User is not found');
        }
        return $model;
    }
}
```

## role binding
type in bash console

```
yii roles/assign
```
than type user name and choose his role

## [next]

```php
```

## [next]

```php
```

#### Souces:
[Доступ к сайту на основе ролей (RBAC) в Yii2.](https://klisl.com/rbac.html) ( githab: [klisl](https://github.com/klisl) )<br/>
[Yii2-advanced: Гибкая настройка Yii2 RBAC (роли, разрешения, правила)](https://habr.com/ru/post/327170/) ( habr: [Jekshmek](https://habr.com/ru/users/Jekshmek/) )<br/>



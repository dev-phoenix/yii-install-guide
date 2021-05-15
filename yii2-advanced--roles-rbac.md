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
you shell need uncomment row:
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


## RBAC Start Controller
Add and change file<br/>
`<your-site-dir>/console/controllers/RbacStartController.php`
```php
<?php
namespace console\controllers;

use Yii;
use yii\console\Controller;

class RbacStartController extends Controller
{
    public function actionInit()
    {
        $auth = Yii::$app->authManager;

        // добавляем роль "user"
        $user = $auth->createRole('user');
        $auth->add($user);

        // добавляем роль "admin"
        $admin = $auth->createRole('admin');
        $auth->add($admin);
        $auth->addChild($admin, $user);
   }

}
```
### bash
```
php yii rbac-start/init
```

change method signup() in model SignupForm<br/>
`frontend/models/SignupForm.php`
```php
public function signup()
{
    if (!$this->validate()) {
        return null;
    }
    
    $user = new User();
    $user->username = $this->username;
    $user->email = $this->email;
    $user->setPassword($this->password);
    $user->generateAuthKey();

    //Добавляем роль по умолчанию для каждого зарегестрированного
    if($user->save()){
        $auth = Yii::$app->authManager;
        $role = $auth->getRole('user');
        $auth->assign($role, $user->id);

        return $user;
    }
    
    return null;
}

```

### add admin role by console controller
add file<br/>
`console/controllers/RbacAdminAssignController.php`
```php
<?php
namespace console\controllers;

use Yii;
use yii\console\Controller;
use common\models\User;
use yii\console\ExitCode;
use yii\helpers\Console;

//php yii rbac-admin-assign/init 1
class RbacAdminAssignController extends Controller
{
    public function actionInit($id){

        //Проверяем обязательный параметр id
        if(!$id || is_int($id)){
            // throw new \yii\base\InvalidConfigException("param 'id' must be set");
            $this->stdout("Param 'id' must be set!\n", Console::BG_RED);
            return ExitCode::UNSPECIFIED_ERROR;
        }

        //Есть ли пользователь с таким id
        $user = (new User())->findIdentity($id);
        if(!$user){
            // throw new \yii\base\InvalidConfigException("User witch id:'$id' is not found");
            $this->stdout("User witch id:'$id' is not found!\n", Console::BG_RED);
            return ExitCode::UNSPECIFIED_ERROR;
        }

        //Получаем объект yii\rbac\DbManager, который назначили в конфиге для компонента authManager
        $auth = Yii::$app->authManager;

        //Получаем объект роли
        $role = $auth->getRole('admin');

        //Удаляем все роли пользователя
        $auth->revokeAll($id);

        //Присваиваем роль админа по id
        $auth->assign($role, $id);

        //Выводим сообщение об успехе и возвращаем соответствующий код
        $this->stdout("Done!\n", Console::BOLD);
        return ExitCode::OK;
        
   }
}
```
### bash
for add admin role to user whith id = 1<br/>
type on bash:
```
php yii rbac-admin-assign/init 1
```

### how check role with rbac
exemple:
```php
<?php
//Если текущий пользователь - не гость
if (!Yii::$app->user->isGuest) {
    $userId = \Yii::$app->user->getId();

    //Все роли текущего пользователя
    var_dump(\Yii::$app->authManager->getRolesByUser($userId));
    PHP_EOL;

    //Разрешение пользователя
    var_dump(\Yii::$app->authManager->getAssignment('admin', $userId));
    PHP_EOL;

    //Все разрешения пользователя
    var_dump(\Yii::$app->authManager->getAssignments($userId));
    PHP_EOL;

    //Проверка доступа пользователя
    var_dump(\Yii::$app->authManager->checkAccess($userId, 'admin', $params = []));
    PHP_EOL;
    
    //Тоже проверка доступа пользователя
    var_dump(Yii::$app->user->can('admin'));


} else {
    echo "Здравствуйте, Гость!";
}
?>
```
simple exemple:
```php
if(Yii::$app->user->can('admin')){
    echo "Привет, админ!" . PHP_EOL;
}

//Аналогично работает с вариантом выше
if(\Yii::$app->authManager->checkAccess($userId, 'admin', $params = [])){
    echo "Привет, админ!" . PHP_EOL;
}
```

Как контролировать доступ по ролям в контроллере с фильтром AccessControl
## Controll access by roles on controller with AccessControl filter
with help `yii\filters\AccessControl`<br/>
in SiteController:
```php
public function behaviors()
{
    return [
        'access' => [
            'class' => AccessControl::className(),
            'only' => ['logout', 'signup'],
            'rules' => [
                [
                    'actions' => ['signup'],
                    'allow' => true,
                    'roles' => ['?'],
                ],
                [
                    'actions' => ['logout'],
                    'allow' => true,
                    'roles' => ['@'],
                ],
                
            ],

        ],
        'verbs' => [
            'class' => VerbFilter::className(),
            'actions' => [
                'logout' => ['post'],
            ],
        ],
    ];
}
```
for access only by admin role change to:
```php
public function behaviors()
{
    return [
        'access' => [
            'class' => AccessControl::className(),
            'only' => ['logout', 'signup'],
            'rules' => [
                [
                    'actions' => ['signup'],
                    'allow' => true,
                    'roles' => ['?'],
                ],
                [
                    'actions' => ['logout'],
                    'allow' => true,
                    'roles' => ['@'],
                ],
                
            ],

        ],

        //Доступ только для админа 
        [
            'class' => AccessControl::className(),
            'only' => ['index'],
            'rules' => [
                [
                    'actions' => ['index'],
                    'allow' => true,
                    'roles' => ['admin'],
                ],
            ],
            
        ],
  
        'verbs' => [
            'class' => VerbFilter::className(),
            'actions' => [
                'logout' => ['post'],
            ],
        ],
    ];
}
```

===============

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
[Yii2 RBAC DbManager](https://coderius.biz.ua/blog/article/yii2-rbac-dbmanager-cast-1-primer-nastrojki-i-sozdania-rolej)
 ( facebook: [Sergio Codev](https://www.facebook.com/sergio.codev.1) )<br/>
[Доступ к сайту на основе ролей (RBAC) в Yii2.](https://klisl.com/rbac.html) ( githab: [klisl](https://github.com/klisl) )<br/>
[Yii2-advanced: Гибкая настройка Yii2 RBAC (роли, разрешения, правила)](https://habr.com/ru/post/327170/)
 ( habr: [Jekshmek](https://habr.com/ru/users/Jekshmek/) )<br/>



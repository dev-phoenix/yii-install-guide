# yii-install-guide

# develop yii throw git

## install composer
use guide: [install git via bush](https://getcomposer.org/download/)
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

or
```
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```

## install git to your hosting
use guide: [install git via bush](https://git-scm.com/book/ru/v2/Введение-Установка-Git)<br>
vizit https://github.com/git/git/releases and select git version source url<br>
on CentOS use:
```
sudo yum update -y
sudo yum groupinstall "Development Tools" -y
sudo yum install gettext-devel openssl-devel perl-CPAN perl-devel zlib-devel -y
yum install wget -y

wget https://github.com/git/git/archive/v2.13.0.tar.gz -O git.tar.gz
tar -zxf git.tar.gz
cd git-2.13.0
make configure
./configure --prefix=/usr/local
sudo make install

git --version
```

install git-manual
```
cd /usr/local/share/man
git clone http://git.kernel.org/pub/scm/git/git-manpages.git
ls git-manpages/
cp -r /usr/local/share/man/git-manpages/* /usr/local/share/man/
git help config
```

git configurate
```
git config --local user.name "dev-phoenix"
git config --local user.email dev-phoenix@mail.ru
git config --list --show-origin
```

## for install yii2 anvanced
go to future site's dir<br/>
and for create clear installation<br/>
type to console:
```
composer create-project --prefer-dist yiisoft/yii2-app-advanced yii-advanced
php init
php yii migrate
```
if you caught trouble like php version, try found solve in this:
 [yii2 advanced based steps](https://github.com/dev-phoenix/yii-install-guide/blob/master/yii2-advanced--based-steps.md)

## Install bootstrap4:
use guide: [Twitter Bootstrap 4 Extension for Yii 2](https://github.com/yiisoft/yii2-bootstrap4)

some example for php widget:
```php
echo Button::widget([
    'label' => 'Action',
    'options' => ['class' => 'btn-primary'], // create class "btn btn-primary"
]);
```

After install and made needed operation,<br/>
maybe you may noticed that site not work.
Maybe you need change dirs and files owner:
```
sudo chown -R user-name:user-group <site-dir>
```

## create first repositories
In first step, create repo on your github project<br/>
then create your repo in dir of yii2-advanced installation: 
```
cd path-to-yii/yii2-advanced
git init
git add .
git commit -m "First repo"
git branch -M master
git remote add origin [your-repo-ssh_or_https-link]
# if you got mistake, you can change adress, that you now named 'origin'
git remote set-url origin [your-repo-ssh_or_https-link]
git push origin master
```

### if you have error like:
```
fatal: refusing to merge unrelated histories
```
when you try pull or push,
maybe you have always created repositories on hosting or local and on githab.

You can try next:
```
git pull origin master --allow-unrelated-histories
```
then how always: add, commit, push, ... etc.


## git ignore files
If you want exclude some files from repo,<br/>
insert it's path to file .gitignore
Look example of .gitignore in root of your site

You must add rule for excluding before create excluded file.
Otherwise you can get problem with non excluded file.
If you got this problem, you must exclude file from branch tracking:
```
git reset <file-name>
```
or
```
git rm --cached <file-name> 
```

## notice to add clear link
add file:
    fointed/web/.htaccess
```htaccess
Options +FollowSymLinks
IndexIgnore */*

RewriteEngine on

# if a directory or a file exists, use it directly
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d

# otherwise forward it to index.php
RewriteRule . index.php
```

## GII
go to 'http://[yout-domin]/gii'



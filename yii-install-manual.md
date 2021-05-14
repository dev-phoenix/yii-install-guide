# yii-install-guide

## develop yii throw git

install composer
use guide: [install git via bush](https://github.com/yiisoft/yii2-bootstrap4)
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

install git to your hosting
use guide: [install git via bush](https://github.com/yiisoft/yii2-bootstrap4)
```
sudo yum update -y
sudo yum groupinstall "Development Tools" -y
sudo yum install gettext-devel openssl-devel perl-CPAN perl-devel zlib-devel -y
yum install wget -y

# https://github.com/git/git/releases
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

git remote set-url origin git@github.com:dev-phoenix/yii-install-guide.git

for install yii2 anvanced, tipe to console:
```
composer create-project --prefer-dist yiisoft/yii2-app-advanced yii-application
```

```php
echo Button::widget([
    'label' => 'Action',
    'options' => ['class' => 'btn-primary'], // создаст класс "btn btn-primary"
]);
```

## Install bootstrap4:
use guide: [Twitter Bootstrap 4 Extension for Yii 2](https://github.com/yiisoft/yii2-bootstrap4)

## add clear link
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
# yii-install-guide
install and development yii2 via bash and composer and git and github

##
guide [how develop yii](https://github.com/dev-phoenix/yii-install-guide/blob/master/yii-install-manual.md):
=====

short plan:
```
instal composer,
git,
yii,
create branch on yii dir,
create branch on github,
push branch from histing to github,
clone branch from github to local machin,
update scripts,
push its to github,
pull from hithub to hostig,
update site page on your browser.
```

[preparation](https://github.com/dev-phoenix/yii-install-guide/blob/master/yii-install-manual.md)
[database tables](https://github.com/dev-phoenix/yii-install-guide/blob/master/yii2-advanced--based-steps.md)
[access system RBAC](https://github.com/dev-phoenix/yii-install-guide/blob/master/yii2-advanced--roles-rbac.md)

if you have error like:
```
fatal: refusing to merge unrelated histories
```
when you try pull or push,<br/>
maybe you have always created repositories on hosting or local and on githab.

You can try next:
```
git pull origin master --allow-unrelated-histories
```
then how always: add, commit, push, ... etc.
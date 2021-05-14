# yii-install-guide
install and development yii2 via bush and composer and git and github

##
guide [how develop yii](https://github.com/dev-phoenix/yii-install-guide/blob/master/yii-install-manual.md):
=====

short plan:
```
instal composer,
git,
yii,
chreate branch on yii dir,
chreate branch on github,
push branch from histing to github,
pull branch from github to local machin,
update scripts,
push its to github,
pull from hithub to hostig,
update site page on your browser.
```

if you have error like:
```
fatal: refusing to merge unrelated histories
```
when you try pull or push,
maybe you have always created repositories on hosting or local and on githab.

You can try next:
```
git pull origin master --allow-unrelated-histories
```
than how always: add, commit, push, ... etc.
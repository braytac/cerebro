http://blog.tfnico.com/2011/02/small-subversion-guide-for-git-users.html

git clone URL
svn co URL


git add file
svn add file


git status
svn st


git commit -m "MSG"; git push
svn commit -m "MSG"


git pull
svn update


git log -p
svn log --diff | less
svn log --diff fichero | less

svn log --diff -r 64233
  ver los cambios de una revision determinada


git checkout -l
svn ls svn://svn.zabbix.com/branches/


git checkout rama/
svn co svn://svn.zabbix.com/branches/2.4


git blame
svn blame
  nos pone última revision y autor que modificaron la línea

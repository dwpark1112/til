# git command

그냥 git command로 작업하던 것들 중 자주 안써서 익숙하지 않은 것들은 익숙해질때까지 써둬야겠다.

**remote에 잘못 올라간 directory 삭제 처리**

```
git rm -r --cached [removetarget]
git commit -m 'remove ...'
git push origin master
```
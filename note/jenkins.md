# Jenkins 이슈 정리

CI에서도 새로운게 많이 나오고 있어서... 이제 젠킨스를 오래 쓸 것 같진 않지만, 그 새로운걸 배우고 적용할 때까지는 젠킨스를 써야 하니깐 ㅠㅠ

## Parameterized Jenkins build for rollback purposes.

<http://blog.ramanshalupau.com/parameterized-jenkins-build-for-rollback-purposes>

**This build is parameterized**

프로젝트 구성에 들어가서, '이 빌드는 매개변수가 있습니다.'라고 체크한 뒤에 String parameter로 구성, 이름은 GIT_TAG로 하면 될 것 같고, default value는 있으면 좋은 것 같다. 없으면 불편

**Source code management** 탭에서

git repository 설정을 추가하는데, name에는 origin, Refspec은 아래 처럼 작성

```
+refs/tags/$GIT_TAG:refs/remotes/origin/tags/$GIT_TAG
```

**Branches to build** 에도 기존에 master로만 되어 있었다면, 

```
*/tags/$GIT_TAG
```

이제 빌드할 때마다 tag 정보를 넣어주면 된다.

다만... 배포할때마다 tag 정보를 넣어줘야 하기에 걸기적 거린다는 것... ㅠㅠ
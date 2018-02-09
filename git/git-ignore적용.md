## 간혹 gitignore가 적용되지 않는다.
```
$ git rm -r --cached .
$ git add .
$ git commit -m "fixed untracked files”
```
위명령을 수행하면 된다.
단, 위 명령은 저장소에 저장되어 있는 내용들도 모두 적용시켜버립니다.

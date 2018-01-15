# Django-guardian을 이용한 권한이 적용된 객체 가져오기

설정된 객체 권한에 대해 모든 객체 목록을 가져올때 사용하는 방법이다.
django-guardian는 shorcuts라는 라이브러리로 필요한 기능들을 여럿 제공하는데 그중 get_objects_for_user를 이용해서 객체를 가져오자.

## 사용법
```python
from guardian.shorcuts import get_objects_for_user

list_of_object = get_objects_for_user(user, permission)
```
위와 같이 사용하면 됩니다.
함수형 view를 사용하는 경우 user부분에 request.user를 사용하면 됩니다.
user가 가진 permission이 적용된 객체들을 불러오는 역할을 합니다.
예를 들면 permission이 'project.view_project'라면 project객체들 중 user가 view_project권한으로 등록되어있는 것을 반환합니다.
즉 보기 권한이 있는 녀석들만 반환하게 되겠죠.
반환된 객체는 뭐리해서 얻는 객체와 동일하게 사용하실 수 있습니다.

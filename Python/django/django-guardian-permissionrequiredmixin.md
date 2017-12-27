# Django-guardian 의 permissionRequiredMixin 사용기

Django는 훌륭한 인증 방식을 제공한다.
그러나 기본적인 인증 방식인 django.contrib.auth.mixins 패키지는 범용적으로는 좋으나 입맛에 맞추기에 맞지 않았다.
## mixin을 사용하는 이유
django는 view를 작성하는 방식이 크게 2가지가 있다.
* 함수형으로 작성하는 것
* 클래스형으로 작성하는 것

무엇이 좋고 나쁜지는 상황에 따라 다르다.
하지만 django의 강력함은 클래스형을 사용할때 나온다.
그 이유는 클래스를 상속받아서 그대로 사용하거나 일부 수정해서 사용하면 편리하기 때문이다.

mixin은 이런 django에서 클래스기반으로 작성된 view에서 사용하기 위한 기능이다.
함수를 사용할 경우에는 데코레이터를 사용해서 할 수 있지만 클래스는 그렇지 못한 경우가 있기 때문이다.
> 실제로는 사용하는 방법이 있음.

## django-guardian은?

django.contrib.auth는 일반적인 모델에 대한 접근 권한과 view에 접근을 제어한다.
그러나 object(데이터 row)에 접근은 제어하지 못한다.(아마도)
그래서 사용하는 것이 django-guardian이다.
객체 하나에 대한 접근을 제어하는데 최적화된 패키지다.

예를 들자면 블로그에서 하나의 글(post)에 접근을 제어하고 싶다면 사용할 수 있는 그런 패키지다.

이 패키지는 django의 auth를 따르기 때문에 django 권한을 생성하여 그대로 사용할 수도 있다.
자세한 설명은 추후에 작성할 것이므로 일단 본론으로 들어가겠다.

django-guardian에서 제공하는 permissionRequiredMixin을 사용하려고 했는데
초반에는 updateview, deleteview 클래스에서는 사용이 못했던거 같은데 지금은 수정되어 지원된다고 한다.

## CreateView에서의 문제
그러나 createView에서 그대로 적용해서 사용하려고 하니 아래와 같은 문제가 발생했다.

```python
from guardian.mixins import PermissionRequiredMixin
class AddProject(PermissionRequiredMixin, CreateView):
    template_name = 'myapp/add_new.html'
    form_class = AddModel
    model = Mymodel

    # permission_object = None
    permission_required = ['myapp.add_new']
    accept_global_perms = True
    raise_exception = True
    return_403 = True
```
```
AttributeError: Generic detail view {view_name} must be called with either an object pk or a slug.
```
permissionRequiredMixin을 상속받아서 그대로 했는데 위와 같은 에러가 발생했다.
문제가 무엇인지 찾아보았다.

## 해결책
아주 간단한 거다. permissionRequiredMixin은 인증할 객체에 대한 정보가 있어야 한다는 것인데
이를 get_object 메소드에서 호출하기에 그때 pk(객체의 id)를 찾을 수 없어서 발생한다는 것이다.
예전에는 get_object() 메소드를 수정해야했는데 지금은 간단히 변수만 지정해주면 된다.

위 코드에서 주석처리된 부분을 활성시키면 된다.
즉 인증객체를 None으로 지정해주어 pk를 사용하지 않도록 만들면 된다.
```python
from guardian.mixins import PermissionRequiredMixin
class AddProject(PermissionRequiredMixin, CreateView):
    template_name = 'myapp/add_new.html'
    form_class = AddModel
    model = Mymodel

    permission_object = None
    permission_required = ['myapp.add_new'] # 필요한 인증 권한
    accept_global_perms = True # 객체별이 아닌 전체 권한으로 검색 허용.
    raise_exception = True # 에러 발생시킴
    return_403 = True # 403에러 처리
```
생각보다 간단한 문제여서 다행이다^^

참고:
https://pypkg.com/pypi/django-guardian/f/example_project/articles/views.py
https://github.com/django-guardian/django-guardian/issues/279

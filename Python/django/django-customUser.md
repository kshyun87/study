# 장고(Django)에서 사용자를 맘대로 써보기

## 시행착오를 겪으면서 정리.

살다보면 편의상 제공하는 것보다 더 좋게 쓰고 싶을 때가 있습니다.
장고에서는 User클래스를 제공하고 있어서 해당 클래스를 사용하여 유저객체를 사용할 수 있습니다.
그러나 이를 이용하면 ID=Username으로 설정되어 있어서 이메일로 로그인하고 싶은 사람에게는 맞지 않습니다.
그래서 이메일 로그인 방식을 사용하기 위해 방법을 찾아보았습니다.

찾는데는 어렵지 않았습니다.
단, 권한 설정을 제대로 할 수 없는 형식으로 나와서 애를 먹었습니다.

일단 장고가 현재 2.0 버전이 등장했지만 1.11버전 기반으로 설명하겠습니다.

장고는 기존 복잡한 방식으로 유저를 맵핑해서 사용하는 방식이 있었다고 하네요.
그러나 1.5? 1.8버전 부터는 AbstractBaseUser나 AbstractUser를 이용하면 쉽고 간단하게 자신의 입맛대로 사용자계정 정보를 수정할 수 있습니다.

다른 방식은 다른 곳에 많이 있으니 찾아보기 바랍니다.

### 방법
* Proxy Model
https://simpleisbetterthancomplex.com/tutorial/2016/07/22/how-to-extend-django-user-model.html#proxy
* One-To-One방식
* AbstractBaseUser
* AbstractUser

방법은 위 4가지 방식이 있다.

#### AbstractBaseUser
AbstractBaseUser를 상속받아서 새로운 User 모델을 만드는 방법.

이 방식은 생각보다 위험하다. 그 이유는 settings.py를 건들여야 하기 때문인데, 어느정도 만들어진 프로젝트라면 건들 경우 위험할 수도 있으니 미리 계획해서 만드는 것이 좋다.
즉, 프로젝트 시작할때 손대는 것이 정신건강에 이롭다는 말.
##### 언제 써야할까?
django 인증 방식은 써야겠고 유저객체는 맘대로 변형하고 싶을때 사용한다.
예를 들자면 저자와 같이 이메일을 ID값으로 사용하고 싶을 경우.

#### AbstractUser
AbstractUser를 상속받아서 새로운 유저 모델을 만들때 사용한다.
이 것도 settings.py 를 수정해야하니 초장에 하는 것이 좋다.
##### 언제 사용?
다른 것은 건들지 않고 인증 수단만 변경하거나 제어해야하는 경우에 적합하다.
인증 관련한 내용이 없는 추가적인 필드를 생성하거나 하고 싶을때 사용하면 좋다.
상세 내용이라든지, 설문 내용이라든지 등등.
One-To-One방식과 비슷한 방식이다.

여기서 3번째 방식을 사용해보자.
https://milooy.wordpress.com/2016/02/18/extend-django-user-model/
이분이 정리가 잘되어 있어서 이 코드를 그대로 가져가서 사용하는 것도 좋을 것 같다.

자신의 상황에 맞게 가져가서 일부 변경해서 쓰던가
https://github.com/django/django/blob/master/django/contrib/auth/models.py <br>
여기에서 모델 정보를 확인해서 상속 받고 일부 오버라이딩이나 새로 추가해도 된다.

사실 기본 user 모델이 AbstractUser를 상속받아 구현된 것 같아서 그것과 비슷하게 만들어봤다.
단, 상속받은 것은 AbstractBaseUser와 permissionmixin을 상속받아서 구현했다.
```python
from django.contrib.auth import models as auth_models
from django.contrib.auth.models import BaseUserManager, AbstractBaseUser, PermissionsMixin
from django.utils.translation import ugettext_lazy as _

class MyUserManager(BaseUserManager):
    def _create_user(self, email, password,
                     is_staff, is_superuser, **extra_fields):
        """
        Creates and saves a User with the given email and password.
        """
        now = timezone.now()
        if not email:
            raise ValueError('The given email must be set')

        email = self.normalize_email(email)
        user = self.model(email=email,
                          is_staff=is_staff, is_active=True,
                          is_superuser=is_superuser, last_login=now,
                          date_joined=now, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_user(self, email, password=None, **extra_fields):
        return self._create_user(email, password, False, False,
                                 **extra_fields)
    def create_superuser(self, email, password, **extra_fields):
        return self._create_user(email, password, True, True,
                                 **extra_fields)    


class MyUser(AbstractBaseUser,  PermissionsMixin):
    email = models.EmailField(verbose_name='email', max_length=255, unique=True)
    name = models.CharField(u'이름', max_length=10, blank=False, unique=True, default='')

    is_active = models.BooleanField(default=True)
    is_admin = models.BooleanField(default=False)


    objects = MyUserManager()

    USERNAME_FIELD = 'email' # 이메일을 ID(username)으로 지정.
    REQUIRED_FIELDS = ['name']
    class Meta:
        verbose_name = _('user')
        verbose_name_plural = _('users')
        # abstract = False

    def get_full_name(self):
        # The user is identified by their email address
        return self.email

    def get_short_name(self):
        # The user is identified by their email address
        return self.email

    def __str__(self):
        return self.email

    def has_perm(self, perm, obj=None):
        "Does the user have a specific permission?"
        # Simplest possible answer: Yes, always

        # return True
        if self.is_active and self.is_superuser:
            return True
        if self.is_admin:
            return True
        return auth_models._user_has_perm(self, perm, obj)

    def has_module_perms(self, app_label):
        "Does the user have permissions to view the app `app_label`?"
        # Simplest possible answer: Yes, always
        if self.is_active and self.is_superuser:
            return True
        if self.is_admin:
            return True
        # # return True
        return auth_models._user_has_module_perms(self, app_label)


    @property
    def is_staff(self):
        "Is the user a member of staff?"
        # Simplest possible answer: All admins are staff
        return self.is_admin

```

https://github.com/django/django/blob/master/django/contrib/auth/admin.py

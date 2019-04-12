# 관계

## M: N

c9의 insta- SCHOOL

### 1. 중간table만들기

* Student , Lecture

  * 테이블 2개 가지고 관계를 정의할 수 없다. 

    * => 중간에 id를 정의하는 테이블을 추가해야 한다.

    * 즉 Student : Lecture = 1 : N, Lecture : Student = 1 : N 두가지를 합친 것과 유사하다.

      | student_id | pk   |
      | ---------- | ---- |
      |            |      |

      ​									+

      | pk   | lecture_id |
      | ---- | ---------- |
      |      |            |

      ​										=

      | student_id | pk   | lecture_id |
      | ---------- | ---- | ---------- |
      |            |      |            |

```git
piie:~/workspace $ mkdir SCHOOL/
piie:~/workspace $ cd SCHOOL/
piie:~/workspace/SCHOOL $ pyenv virtualenv 3.6.7 school-venv
piie:~/workspace/SCHOOL $ pyenv local school-venv
(school-venv) piie:~/workspace/SCHOOL $ pip install django==2.1.8
(school-venv) piie:~/workspace/SCHOOL $ pip install django-extensions
(school-venv) piie:~/workspace/SCHOOL $ django-admin startproject school .
(school-venv) piie:~/workspace/SCHOOL $ python manage.py startapp sugangs
```



* instance method vs class method
  * instance method : 객체가 사용하는 메서드, 객체 생성 필요
  * class method : 클래스 자체가 사용하는 메서드, 객체 생성 없이 사용 가능
    * objects함수는 class method :  Student.objects.~()
  * class/instance method 수정시에는 migration 안해줘도 된다.



### student_id  = 1 학생이 듣고 있는 수업을 쓰시오

```python
models.py

from django.db import models
from faker import Faker

faker = Faker('ko_kr')
# 2000 ~ 2020
import random

# Create your models here.
class Student(models.Model):
    name = models.CharField(max_length=30)
    student_id = models.IntegerField()
    
    # 더미 데이터를 자동으로 넣어주는 친구
    @classmethod
    def dummy(cls, n):
        for i in range(n):
            cls.objects.create(name=faker.name(), student_id=random.randint(2000, 2020))
        
    def __str__(self):
        return self.name
    
    
class Lecture(models.Model):
    title = models.CharField(max_length=100)
    
class Enrollment(models.Model):
    lecture = models.ForeignKey(Lecture, on_delete=models.CASCADE)
    student = models.ForeignKey(Student, on_delete=models.CASCADE)
    
    def __str__(self):
        return f"{self.student.name}님이 {self.lecture.title} 과목을 수강하였습니다."
    
```



* Shell Plus로 확인할 수 있다.

```git
terminal(test작업)

(school-venv) piie:~/workspace/SCHOOL $ python manage.py shell_plus

>>> Student.dummy(10)
>>> Student.objects.all()
<QuerySet [<Student: 이재현>, <Student: 김수빈>, <Student: 김승현>, <Student: 김옥자>, <Student: 김선영>, <Student: 김지후>, <Student: 백준서>, <Student: 김은경>, <Student: 이상철>, <Student: 서현숙>]>
>>> Lecture.objects.create(title="알고리즘")
<Lecture: Lecture object (1)>
>>> Lecture.objects.create(title="자료구조")
<Lecture: Lecture object (2)>
>>> Lecture.objects.create(title="데이터베이스")
<Lecture: Lecture object (3)>
>>> Lecture.objects.create(title="운영체제")
<Lecture: Lecture object (4)>
>>> Lecture.objects.create(title="인공지능")
<Lecture: Lecture object (5)>
>>> Lecture.objects.all()
<QuerySet [<Lecture: Lecture object (1)>, <Lecture: Lecture object (2)>, <Lecture: Lecture object (3)>, <Lecture: Lecture object (4)>, <Lecture: Lecture object (5)>]>
>>> Enrollment.objects.create(student_id=1, lecture_id=1)
<Enrollment: Enrollment object (1)>
>>> enroll = Enrollment.objects.first()
>>> enroll.student
<Student: 이재현>
>>> enroll.lecture
<Lecture: Lecture object (1)>
>>> enroll.lecture.title
'알고리즘'
>>> Enrollment.objects.all()
```



* 객체를 뽑아서 객체를 넣는것이 좋음

```git
>>> kcm = Student.objects.first()

>>> for num in range(2, 6)
... 	Enrollment.objects.create(student = kcm, lecture_id=num)
...

# 아래 3개 같은 명령어
>>> kcm = Student.objects.get(pk=1)
>>> kcm = Student.objects.get(id=1)
>>> kcm = Student.objects.get(name="강창모)

# for문 예시
>>> lectures = Lecture.objects.all()
>>> for lecture in lectures:
... 	lecture.title
...

# 마지막 객체 뽑기
>>> Student.objects.last()
```

* Enrollment 클래스에 중복으로 추가되지 않도록 제어해야 한다.(filtering X)





##### 미션

1. 나의 데이터를 넣는다.

2. 나는 운영체제와 인공지능을 수강 중이다.

3. 내가 듣고 있는 강의 목록을 출력하려면

   ```git
   >>> jcm.enrollment_set.all()
   ```





### 2. django-model 사용하기

* Client, Resort 테이블

MVT, MVC pattern을 사용할 때 model fat 을 추구한다.

* Metadata : 데이터에 대한 데이터

  * ordering : 저장된 정렬 기준대로 정렬한다.

    ```python
    class Meta:
        ordering = ('name', ) # 정렬 기준 ['name', ]으로 사용해도 됨
    ```

* table을 추가로 하나 더 만드는 것이 아니라 ManyToManyField함수를 사용한다.

  * 관계를 설정할 두 개의 테이블 중에 한 곳에만 추가한다.
  * ManyToManyField는 어느 클래스에 추가해도 상관없지만 python구조상 위에서 아래로 실행되기 때문에 아래에 위치하는 class에 추가해줘야 한다.
  * 관계 table을 추가해주지 않아도 위의 함수에 의해 django를 통해 만들어준다.

  ```python
  from django.db import models
  from faker import Faker
  
  faker = Faker('ko_kr')
          
  # 예약, 수강, ...
  class Client(models.Model):
      name = models.CharField(max_length=30)
      
      # class Meta:
      #     ordering = ('name') # 정렬 기준 ['name', ]으로 사용해도 됨
  
      @classmethod
      def dummy(cls, n):
          for i in range(n):
              cls.objects.create(name=faker.name())
      
  class Resort(models.Model):
      name = models.CharField(max_length=30)
      # clients = models.ManyToManyField(모델클래스이름, on_delete필수X)
      clients = models.ManyToManyField(Client, related_name='resorts')
      # related_name : 쌍방 관계를 위해 이름 정의해준다. Resort에서 client의 name을 기준으로 사용된다. Resort기준으로 사용하려면 Client클래스에도 related_name을 입력해주면 된다.
      
      @classmethod
      def dummy(cls, n):
          for i in range(n):
              cls.objects.create(name=faker.company())
      
  ```

* sugangs앱에서 0002migration을 sql문으로 보기

```git
$ python manage.py sqlmigrate sugangs 0002
```



* Shell Plus에서 확인해보기

  ```git
  >>> Resort.dummy(5)
  >>> Resort.objects.all()
  <QuerySet [<Resort: 이오>, <Resort: 이윤>, <Resort: 이엄노>, <Resort: 박김김>, <Resort: 한황>]
  >>> Resort.objects.first().name
  '이오'
  >>> Client.dummy(20)
  >>> Client.objects.all()
  <QuerySet [<Client: 윤영순>, <Client: 김성진>, <Client: 고지혜>, <Client: 오지훈>, <Client: 이준영>, <Client: 이민서>, <Client: 양민석>, <Client: 손미영>, <Client: 김지아>, <Client: 김미숙>, <Client: 안영길>, <Client: 김선영>, <Client: 남준영>, <Client: 박광수>, <Client: 유도윤>, <Client: 강민재>, <Client: 송상철>, <Client: 강성민>, <Client: 송성수>, <Client: 김지우>]>
  >>> resort = Resort.objects.get(pk=2)
  >>> resort
  <Resort: 이윤>
  >>> resort.clients.all()
  <QuerySet []>
  >>> resort.clients.add(Client.objects.first())
  >>> resort.clients.all()
  <QuerySet [<Client: 윤영순>]>
  
  # resort에 client 추가
  >>> resort.clients.add(Client.objects.last())
  >>> resort.clients.all()
  <QuerySet [<Client: 윤영순>, <Client: 김지우>]>
  
  # resort에 client 제거
  >>> resort.clients.remove(Client.objects.last())
  >>> resort.clients.all()
  <QuerySet [<Client: 윤영순>]>
  
  # 쌍방관계 사용(related_name추가했을 때)
  >>> client = Client.objects.first()
  >>> client.resorts
  <django.db.models.fields.related_descriptors.create_forward_many_to_many_manager.<locals>.ManyRelatedManager object at 0x7f7c6f3bbef0>
  >>> client.resorts.all()
  <QuerySet [<Resort: 이윤>]>
  ```

* 조합이 중복되는 것을 자체적으로 막아준다.(filtering O)
# 3. Metaclass `__new__` and `__call__`
클래스를 만드는 함수 또는 클래스이다. 그래서 두 가지 방식으로 구현할 수 있다.
- `type`을 사용한 동적 구현
- `type`을 상속받은 클래스로 구현(e.g. `abc.ABCMeta`)

## 3.1 `type`을 사용한 동적 구현
```python
클래스 = type('클래스이름', (Base클래스튜플), {속성/메서드dict})
```


```python
Hello = type('Hello', (), {}) # 비어있는 클래스 생성
Hello # 클래스
```




    __main__.Hello




```python
h = Hello()
h # 인스턴스
```




    <__main__.Hello at 0x7f02182e97c0>




```python
def replace(self, old, new):   # 클래스에 들어갈 메서드 정의 self 
    while old in self:
        self[self.index(old)] = new

# list 를 상속받고 속성 desc, 메서드 replace추가         
AdvancedList = type('AdvancedList', (list, ), {'desc': 'Advanced list', 'replace': replace})

AdvancedList
```

    __main__.AdvancedList


```python
x = AdvancedList([1, 2, 3, 4, 5, 6, 7, 8, 9])
x.desc
```


    'Advanced list'


```python
x.replace(3, 30)
x
```
    [1, 2, 30, 4, 5, 6, 7, 8, 9]



## 3.2 `type`을 상속받은 클래스로 구현
```python
class 메타클래스이름(type):
    def __new__(metacls, name, bases, namespace):
        코드
```


```python
class MakeCalc(type):
    def __new__(metacls, name, bases, namespace):
        namespace['desc'] = 'Calculator Class'
        namespace['add'] = lambda self, a, b: a + b
        return type.__new__(metacls, name, bases, namespace)
    
Calc = MakeCalc('Calc', (), {})
Calc.__dict__
```

    mappingproxy({'desc': 'Calculator Class',
                  'add': <function __main__.MakeCalc.__new__.<locals>.<lambda>(self, a, b)>,
                  '__module__': '__main__',
                  '__dict__': <attribute '__dict__' of 'Calc' objects>,
                  '__weakref__': <attribute '__weakref__' of 'Calc' objects>,
                  '__doc__': None})


```python
c = Calc()
c.desc
```

    'Calculator Class'


```python
c.add(2, 5)
```
    7



## 3.3 Metaclass는 클래스의 동작제어 용도로 사용
- 다음의 sington 디자인 패턴으로  제어하는 경우
- 추상클래스의 `abc.ABCMeta`


```python
# Singleton 패턴으로 제어하는 경우
class Singleton(type):
    __instances = {} # 클래스의 인스턴스 저장용
    def __call__(cls, *args, **kwargs):
        if cls not in cls.__instances:
            cls.__instances[cls] = super().__call__(*args, **kwargs)
        return cls.__instances[cls]
    
class Hello(metaclass=Singleton):
    pass

a = Hello()
b = Hello()
a is b
```

    True



## 3.4 `__new__()`

|   | new 	                                                | init                                      |
|:--|:------------------------------------------------------|:------------------------------------------|
|1  | Called before init 	                                | Called after new                          |
|2  | Accepts a type as the first argument 	                | Accepts an instance as the first argument |
|3  | Is supposed to return an instance of the type received| Is not supposed to return anything        |
|4  | Used to control instance creation 	                | Used to initialize instance variables     |


```python
# 1, 2, 3
class Example1:      # class Example1(object):
    
    def __init__(self):
        print("__init__ called")
        return None
    
    def __new__(cls):
        print("__new__ called")
        return object.__new__(cls)
        
example1 = Example1()
```

    __new__ called
    __init__ called



```python
# 4 메타클래스가 아닌 클래스로 Singleton 구현(type 대신 object를 상속받는다)
class Singleton2(object):      # class Singleton2:
    __instances = None
    def __new__(cls):
        if cls.__instances is None:
            print("creating...")
            cls.__instances = object.__new__(cls)
        return cls.__instances
    
s = Singleton2()
t = Singleton2()
s, t
```

    creating...
    (<__main__.Singleton2 at 0x7f0218338bb0>,
     <__main__.Singleton2 at 0x7f0218338bb0>)



## 3.5 `__call__()`
- 클래스의 인스턴스를 function처럼 기동하게 하는 메서드이다. 즉, 인스턴스 `x`에 대하여  

```python
x(arg1, arg2, ...)
```

는 다음을 줄여 쓴 것이다.   

```python
x.__call__(arg1, arg2, ...)
```


```python
class Example2:
    def __init__(self):
        print("Instance is created.")
    def __call__(self):
        print("Instance is called via __call__ method.")
        
e2 = Example2()
```

    Instance is created.



```python
e2()
```

    Instance is called via __call__ method.



```python
class Example3:
    def __init__(self):
        print("Instance is created.")
    def __call__(self, a, b):
        print(f"{a} and {b} are typed.")
        
e3 = Example3()
whatistyped = e3('This', 'that')
print(whatistyped)
```

    Instance is created.
    This and that are typed.
    None


## References

1. 남재윤, <a href="https://dojang.io/mod/page/view.php?id=2468">*파이썬 코딩도장, 47.9 메타클래스 사용하기*</a>, (2019)
2. <a href="https://santoshk.dev/posts/2022/__init__-vs-__new__-and-when-to-use-them/">https://santoshk.dev/posts/2022/__init__-vs-__new__-and-when-to-use-them/</a>
3. <a href="https://www.geeksforgeeks.org/__call__-in-python/">https://www.geeksforgeeks.org/__call__-in-python/</a>



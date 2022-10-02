# Privates and Property in classes

파이썬에서는 private과 getter setter 지정이 느슨한 편에 속한다. 그렇듯 엄격하지는 않아도 광범위하게 통용되어 
이어져온 rules에 익숙해지도록 하자.

## 1. Privates in Classes
파이썬에서는 모든 것들이 다 public이다. 그러나 경우에 따라 private으로 숨겨두는 것이 
나을 수 있다. 그러한 작성자의 의도를 명기할 필요가 있는데, python에는 그런 명시적 
예약어가 없는 것이 문제이다. 이런 고심 끝에 탄생하여 convestion으로 정착한 것이 
"*namespace mangling* 이름 변형시키기" 수작이다. 이를 테면 변수 `variable`을 
살짝 변형하여 `_variable` 처럼 앞이나 뒤에 `_`uderscore를 하나 혹은 둘 씩 붙이는 
수작을 부린다.


```python
class Account:
    def __init__(self, name, date_of_birth, password):
        self.name = name
        self.date_of_birth = date_of_birth
        self._numbers = None
        self.__password = password
        
    def __repr__(self):
        return f"Customer: {self.name}  {self.date_of_birth}"
myaccount = Account('White', '2000-01-01', '1234')
myaccount
```




    Customer: White  2000-01-01



- underscore가 하나 앞에 붙은 `_numbers`와 두 개가 붙은 `__password`를 사용하여 비공개의도를 명시하였다. 그래서 인스턴스를 representation하더라도 password가 포함되지 않도록 작성했다.
- 그러나 다음에서 보듯, 외부에서 접근할 수 있다. 
- 특이한 점은 `__password`가 `_Account__password`로 대체되었다는 점이다.


```python
myaccount._numbers, myaccount._Account__password
```




    (None, '1234')



- 유용한 [사용예](https://docs.python.org/3/tutorial/classes.html)는 다음처럼 subclass의 메서드와 혼동을 피할 목적으로 사용하는 경우다.


```python
class Mapping:
    def __init__(self, iterable):
        self.items_list = []
        self.__update(iterable)

    def update(self, iterable):
        for item in iterable:
            self.items_list.append(item)

    __update = update   # private copy of original update() method

class MappingSubclass(Mapping):

    def update(self, keys, values):
        # provides new signature for update()
        # but does not break __init__()
        for item in zip(keys, values):
            self.items_list.append(item)
```

## 2. Property in classes

- Property 데코레이터는 getter 와 setter를 통일된 한가지 방식으로 단순화한다.
- 위에서 설명된 것처럼 언더스코어를 사용하여 `_x` 로 놓는 것은 getter/setter 메서드명과 혼동을 피하기 위해서이다.


```python
class C:
    def __init__(self):
        self._x = None
    def setx(self, value):
        self._x = value
    def getx(self):
        return self._x
    def delx(self):
        del self._x
    x = property(getx, setx, delx, "I'm the 'x' property.")

c = C()
c.x = 'small'
c.x
```




    'small'




```python
del c.x
```

- 이러한 property의 모양은 다음처럼 데코레이터로 구현할 수 있음을 말해준다.


```python
class Parrot:
    def __init__(self):
        self._voltage = 100000
        
    @property
    def voltage(self):
        """Get the current voltage."""
        return self._voltage
    
p = Parrot()
p.voltage
```




    100000




```python
# 처음의 예시와 정확히 같은 클래스이다.
class C:
    def __init__(self):
        self._x = None
    @property
    def x(self):
        """I'm the 'x' property."""
        return self._x
    @x.setter
    def x(self, value):
        self._x = value
    @x.deleter
    def x(self):
        del self._x
```

## References
1. [공식튜토리얼](https://docs.python.org/3/tutorial/classes.html)
2. [`class Property` python doc](https://docs.python.org/3/library/functions.html?highlight=property#property)

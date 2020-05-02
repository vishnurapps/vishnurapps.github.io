Here is a basic class in python. There are two functions in the class. One for setting value and other for getting value.

```python
class MyClass(object):

    def set_val(self, value):
        self.value = value

    def get_value(self):
        print(self.value)
        return self.value

a = MyClass()
b = MyClass()

a.set_val(20)
b.set_val(200)

a.get_value()
b.get_value()
```
The output is given below.

```
vishnu@Oakwood:~/development/python$ python 001_oops.py 
20
200
```

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

a.value = 30

a.get_value()
b.get_value()
```

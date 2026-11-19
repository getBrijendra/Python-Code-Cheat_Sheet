In Python, both @classmethod and @staticmethod are decorators used to define methods inside a class.

Main difference:
- @classmethod → Has access to the class using cls. It can access or modify class variables.
- @staticmethod → Does not automatically receive self or cls. It behaves like a normal function inside a class.

'''
class Student:
    school = "ABC School"

    @classmethod
    def change_school(cls, name):
        cls.school = name

    @staticmethod
    def add(a, b):
        return a + b


# Calling classmethod
Student.change_school("XYZ School")
print(Student.school)  # XYZ School

# Calling staticmethod
print(Student.add(10, 20)) # 30
'''


## **Difference Table **

| Feature                       | `@classmethod`                       | `@staticmethod`              |
| ----------------------------- | ------------------------------------ | ---------------------------- |
| **First parameter**           | `cls`                                | No special parameter         |
| **Access class variables**    | Yes, using `cls`                     | Yes, via explicit class name |
| **Modify class variables**    | Yes, using `cls`                     | Yes, via explicit class name |
| **Access instance variables** | Not directly                         | Not directly                 |
| **Needs object creation**     | No                                   | No                           |
| **Common use**                | Factory methods, class configuration | Utility/helper functions     |
| **Called using**              | Class or object                      | Class or object              |

---- 

'''
class Employee:
    company = 'Google'

    def normal_method(self):
        print("Instance:", self.company)

    @classmethod
    def class_method(cls):
        print("Class:", cls.company)

    @staticmethod
    def static_method():
        print("Static: Hello")

emp = Employee()
emp.normal_method()
Employee.class_method()
Employee.static_method()
'''


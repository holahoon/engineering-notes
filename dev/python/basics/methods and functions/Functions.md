We use `def` to define a function in Python. We can also put some docstring that explains a function.
```python
def name_of_function(name):
	'''
	Docstring explains function
	'''
	print("Hello" + name)
```

Some example:
```python
work_hours = [('DK', 100), ('SH', 4000), ('Jessie', 800)]

def employee_check(work_hours):
    current_max = 0
    employee_of_month = ''

    for employee, hours in work_hours:
        if hours > current_max:
            current_max = hours
            employee_of_month = employee

    # Return tuple
    return (employee_of_month, current_max)

result = employee_check(work_hours)
print(result) # ('SH', 4000)
name, hours = employee_check(work_hours)
print(name) # 'SH'
```





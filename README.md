# py-calculator

Build a Python calculator using Tkinter.

## Tkinter

Tkinter is the most commonly used utility to design the GUI with Python as it is

* fast
* easy
* cross-platform

## Steps

**1. Import Tkinter and `factorial` function from `math` module.**

```python
from tkinter import *
import parser
from math import factorial
```

**2. Make a window where to accomodate buttoms**

```python
root = Tk()
root.title('Py-Calculator')
root.mainloop()
```

**3. Design the buttons**

Design buttons for the calculator and arrange them on the app window.

```python
# add the input field
display = Entry(root)
display.grid(row=1,columnspan=6,sticky=N+E+W+S)
 
# add buttons to the Calculator
Button(root,text="1",command = lambda :get_variables(1)).grid(row=2,column=0, sticky=N+S+E+W)
Button(root,text=" 2",command = lambda :get_variables(2)).grid(row=2,column=1, sticky=N+S+E+W)
Button(root,text=" 3",command = lambda :get_variables(3)).grid(row=2,column=2, sticky=N+S+E+W)
 
Button(root,text="4",command = lambda :get_variables(4)).grid(row=3,column=0, sticky=N+S+E+W)
Button(root,text=" 5",command = lambda :get_variables(5)).grid(row=3,column=1, sticky=N+S+E+W)
Button(root,text=" 6",command = lambda :get_variables(6)).grid(row=3,column=2, sticky=N+S+E+W)
 
Button(root,text="7",command = lambda :get_variables(7)).grid(row=4,column=0, sticky=N+S+E+W)
Button(root,text=" 8",command = lambda :get_variables(8)).grid(row=4,column=1, sticky=N+S+E+W)
Button(root,text=" 9",command = lambda :get_variables(9)).grid(row=4,column=2, sticky=N+S+E+W)
 
# add other buttons to the calculator
Button(root,text="AC",command=lambda :clear_all()).grid(row=5,column=0, sticky=N+S+E+W)
Button(root,text=" 0",command = lambda :get_variables(0)).grid(row=5,column=1, sticky=N+S+E+W)
Button(root,text=" .",command=lambda :get_variables(".")).grid(row=5, column=2, sticky=N+S+E+W)
 
 
Button(root,text="+",command= lambda :get_operation("+")).grid(row=2,column=3, sticky=N+S+E+W)
Button(root,text="-",command= lambda :get_operation("-")).grid(row=3,column=3, sticky=N+S+E+W)
Button(root,text="*",command= lambda :get_operation("*")).grid(row=4,column=3, sticky=N+S+E+W)
Button(root,text="/",command= lambda :get_operation("/")).grid(row=5,column=3, sticky=N+S+E+W)
 
# add new operations
Button(root,text="pi",command= lambda :get_operation("*3.14")).grid(row=2,column=4, sticky=N+S+E+W)
Button(root,text="%",command= lambda :get_operation("%")).grid(row=3,column=4, sticky=N+S+E+W)
Button(root,text="(",command= lambda :get_operation("(")).grid(row=4,column=4, sticky=N+S+E+W)
Button(root,text="exp",command= lambda :get_operation("**")).grid(row=5,column=4, sticky=N+S+E+W)
 
Button(root,text="<-",command= lambda :undo()).grid(row=2,column=5, sticky=N+S+E+W)
Button(root,text="x!", command= lambda: fact()).grid(row=3,column=5, sticky=N+S+E+W)
Button(root,text=")",command= lambda :get_operation(")")).grid(row=4,column=5, sticky=N+S+E+W)
Button(root,text="^2",command= lambda :get_operation("**2")).grid(row=5,column=5, sticky=N+S+E+W)
Button(root,text="=",command= lambda :calculate()).grid(columnspan=6, sticky=N+S+E+W)
```

`Entry` function helps to make a text input field.  
`.grid()` defines the position associated with the button or input field.  
`root` is the name to refer to the window.  
`text` is text to be displayed in the button.  
`row` is the row index of the button.  
`column` is the column index of the grid.  
`columnspan` is the span or the number of columns.  
`sticky` defines how to expand the widget when the resulting cell is larger than the widget.  
`N+E+W+S` means can expand in all directions.  

**4. Mapping buttons to their functionalities**

_4.1. Mapping the digits_

```python
# i keeps the track of current position on the input text field
i = 0
# Receives the digit as parameter and display it on the input field
def get_variables(num):
    global i
    display.insert(i,num)
    i+=1
```
where
* **i** is the position in the input field to insert the digit, is incremented each time to get updated
with the position to insert the next digit or next operator.  
* **num** is the digit.

_4.2. Mapping the operator buttons_

```python
def get_operation(operator):
    global i
    length = len(operator)
    display.insert(i, operator)
    i+=length
```
**get_operation()** receives the operator as a parameter which is inserted into the input field at the `ith` position.

_4.3. Mapping the AC button_

```python
def clear_all():
    display.delete(0,END)
```

_4.4. Mapping the undo button_

```python
def undo():
    entire_string = display.get()
    if len(entire_string):
        new_string = entire_string[:-1]
        clear_all()
        display.insert(0,new_string)
    else:
        clear_all()
        display.insert(0,"Error")
```
`**.get()**` fetches the string present on the input field.  
If there is really a string, slice out the last character, clear the input field, push
the new string back to the input field. The new string does not contain the last character.

_4.5. Mapping the '=' button_

```python
def calculate():
    entire_string = display.get()
    try:
        a = parser.expr(entire_string).compile()
        result = eval(a)
        clear_all()
        display.insert(0,result)
    except Exception:
        clear_all()
        display.insert(0,"Error")
```
`**.get()**` fetches the string presented on the input field.  
In module `parser`, use `**.expr()**` to accept the string as a parameter.
`**.compile()**` evaluates the string syntax. Then clear the input field and then push the result on it.

_4.6. Mapping the factorial key_

```python
def fact():
    entire_string = display.get()
    try:
        result = factorial(int(entire_string))
        clear_all()
        display.insert(0,result)
    except Exception:
    clear_all()
    display.insert(0,"Error")
```
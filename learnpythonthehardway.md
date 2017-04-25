## Learn python the hard way notes
### pydoc
Help on built-in function input in module builtins.

``` python
python -m pydoc input
```

Parameters, Unpacking, Variables
``` python
from sys import argv
script, first, second, third = argv  # unpack
print "The script is called:", script
print "Your first variable is:", first
print "Your second variable is:", second
print "Your third variable is:", third
```


``` python
from sys import argv
script, user_name = argv
print "Do you like me %s?" % user_name
prompt = '> '
likes = raw_input(prompt)

```

Do you like me zed?
>  Yes


### Reading Files
``` python
from sys import argv
script, filename = argv
txt = open(filename)
print("Here's your file %r:" % filename)
print(txt.read())
print("Type the filename again:")
file_again = input("> ")
txt_again = open(file_again)
print(txt_again.read())
```

Python will not restrict you from opening a file more than once and sometimes this is necessary.

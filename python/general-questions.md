# Module vs. Package
Module: single file or files which contain functions and classes that can be used  
Package: directories of modules organized in a way that scales well for larger projects, uses `__init__.py`

# Python is interpreted
Evaluated at runtime rather than compiled into machine code first

# Why Python?
- simplicity, can be written quickly for tools and scripts
- versatility
- extensive libraries and frameworks for app development
- strong community support
- **dynamic typing**: code is flexible because types can be assigned on the fly and mutable

# Global, protected, private attributes
global: variables defined in the global scope. to access within functions and classes, `global` keyword is required
```python
x = 10
def change():
    global x
    x = x + 5
```

# Python case-sensitive?
Yes. 

# exception handling
```
try:
    #
except:
    #
finally:
    #
```
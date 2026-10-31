# The C Programming Language (Kernighan & Ritchie)

## Chapter 1

- `stdio.h` is the standard input/output library, include it at the top of most programs (`#include <stdio.h>`)
- basic hello world in a file called `hello.c`:
```{c}
#include <stdio.h>
int main () {
    printf("hello, world\n");
}
```
- compile with `cc` or `gcc` and then run the executable output file 
```
gcc [-o out_file] hello.c
```
- `main()` is special; program will look for this in a program and start from that function
- variables must be declared before assignment/reference:
```{c}
int main() {
    int fahrenheit, celsius;
    fahrenheit = 20;
    celsius = 5 * (fahrenheit - 32) / 9
}
```
- **integer division truncates** (any fractional part is discarded): in the above example, we need to do `5 * (fahrenheit - 32) / 9` rather than `(5 / 9) * (fahrenheit - 32)`. if we did, the first expression would be 0 and the celsius conversion would incorrectly be 0 as well.
### printing
- the `printf()` function prints its first argument (string/character array) and allows for optional subsititutions in the first argument with other (second, third...) arguments by using `%`. the form the other arguments should be printed it can be specified with e.g. `%d` for integer. for example: `printf("fahrenheit: %d\n", fahrenheit)` will print "fahrenheit: 20" if fahrenheit is 20. 
- you can augment the values that replace the `%d` by adding a width, e.g. `%4d` will print the integer right-justified in a field 4 digits wide, i.e. "  20"
- `%f` for floating point, `%6.2f` for floating point at least 6 digits wide and 2 characters after decimal point
- character string - `%s`, character - `%c`, octal - `%o`, hexadecimal - `%x`
### loops
```{c}
/* while loop example ... */
fahr = 0;
step = 20;
while (fahr <= 300) {
         cels = (5.0/9.0) * (fahr-32.0);
         printf("%3.0f %6.1f\n",fahr,cels);
         fahr = fahr + step;
     }
}

/* for loop example ... */
for (fahr = 0; fahr <= 300; fahr = fahr + step) {
    cels = (5.0/9.0) * (fahr-32.0);
    printf("%3.0f %6.1f\n",fahr,cels);
}
```
### symbolic constants
- these can be used to represent "magic numbers", i.e. things like 300 for an `upper` variable, and 0 for a `lower` variable in a function
- _symbolic constants_ can be used instead, and convey more information to readers of the program later
- they are defined at the top, under `#include`s, using the `#define` operator:
```{c}
#include <stdio.h>

#define UPPER 300
#define LOWER 0
```
- **format**: `#define` _name_  _replacement text_
- _replacement text_ can be any sequence of characters, not just numbers
- they are traditionally written in all-caps
- their lines do not end with a semicolon
- they can be referenced in the program later just like variables, e.g. `fahr = UPPER`

### `long` integers, `double`, and increment operator
- `long n;` defines n as a _long integer_, i.e. a 32-bit integer, rather than an `int` which is a 16-bit integer (max value 32767) on some systems (old ones? — author says `int` and `long` are the same size on some machines)
    - using `printf()`, we use the conversion specification `%ld` to tell it that the corresponding argument is a long integer
- `++n` increments n by 1. `n++` is also valid, but slightly different (expanded in Chapter 2)
- `double nc;` defines nc as a _double precision float_
    - using `printf()`, we use the conversion specification `%f`, just like with normal `float`s

### character constants
- a character written between single quotes (`'A'`) represents an integer value equal to the numerical value of the character in the machine's character set (e.g. ASCII, A is 65)
- escape sequences used in string constants are also legal in character constants, e.g. `'\n'` is the newline character, which is 10 in ASCII — just an integer
    - conversely, `"n"` in double quotes is a string constant that happens to contain only one character (newline)

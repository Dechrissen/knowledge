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

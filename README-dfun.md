# Docker Fundamentals

Some files that you can copy from

## 8 Multi-stage Builds

Following is the `hello.c` code:

```c
#include <stdio.h>
int main (void)
{
   printf ("Hello, world!\n");
   return 0;
}
```

Dockerized hello application
 
```dockerfile
FROM alpine:3.5
RUN apk update && \
      apk add --update alpine-sdk
RUN mkdir /app
WORKDIR /app
COPY hello.c /app
RUN mkdir bin
RUN gcc -Wall hello.c -o bin/hello
CMD /app/bin/hello
```



* [Signature Assignement](README-dfun-sig.md)


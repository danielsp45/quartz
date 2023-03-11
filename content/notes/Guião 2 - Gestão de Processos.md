---
date: 07/03/2023
---
[[Sistemas Operativos]]

### Exercício 2
```c
int main() {
    int pid = fork();
    if (pid == 0) {
        printf("child PID: %d\nparent PID: %d\n", getpid(), getppid());
    } else {
        // parent
        wait(NULL);
    }
}
```

`getppid()` retorna o identificador do processo pai.
`getpid()` retorna o identificador do processo atual

### Exercício 3
```c
void pid_looper(int counter) {
    int pid = fork();

    if (pid == 0 && counter > 0) {
        printf("PPID: %d\n", getppid());
        printf("PID: %d\n", getpid());
        printf("\n");
        pid_looper(counter - 1);
    }
    else {
        wait(NULL);
    }
}

int main() {
    pid_looper(10);
    return 0;
}
```
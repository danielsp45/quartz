---
title: Guião 1 - Acesso a ficheiros
tags:
date: 28/02/2023
---
[[Sistemas Operativos]]

__Descritor de Ficheiro__:
De forma simples, quando um ficheiro é aberto, o sistema operativo cria uma entrada que representa esse mesmo ficheiro e guarda informação sobre esse ficheiro aberto. Exemplificando, se se tiver 100 ficheiros abertos no sistema, então irá também haver 100 entradas. Este número é um inteiro não negativo e designa-se por __descritor de ficheiro__. 
Para além disto também existem descritores standard como:
+ __Standard Input__ (valor inteiro 0)
+ __Standard output__ (valor inteiro 1)
+ __Standard error__ (valor inteiro 2)

## Estruturas de Kernel

##### Tabela de processos (TP)
Por cada processo no computador existe a tabela designada em cima que guarda descritores de ficheiro abertos. Para saber quantos ficheiros abertos posso ter utiliza-se `ulimit -n`

##### Tabela de ficheiros
Tabela partilhada pelo sistema operativo que guarda o modo de abertura e posição de leitura/escrita de cada descritor.

##### Virtual nodes (V-node)
Um V-node é uma estrutura usada para representar um ficheiros, diretorias, FIFO's, domain sockets, etc, num sistema de ficheiros do kernel que contém informações sobre o seu tipo, permissões, dono entre outros.

##### Index node (I-node)
É uma estrutura usada pelo kernel para representar um ficheiro ou diretoria no sistema de ficheiros do SO onde são guardados metadados/atributos dos ficheiros. Para além disso também guarda a localização dos dados no recurso físico de armazenamento. 

> **Nota**:
>  Em sistemas Unix, os I-nodes servem também como v-nodes, não havendo uma implementação explícita para os últimos.

![[Pasted image 20230223181013.png]]

## Chamadas ao sistema
```c
int open(const char *path, int oflag [, mode]);
```
Inicializa um descritor para um determinado ficheiro, devolvendo o descritor ou erro.
* `path` - path do ficheiro a abrir
* `oflag` - modo de abertura (O_RDONLY, O_WRONLY...)
* `mode` - permissões de acesso para O_CREAT (e.g. 0640  
equivale a rw-r-----)

```c
ssize_t read(int fildes, void *buf, size_t nbyte);
```
Devolve o número de bytes lidos ou erro.
+ **fildes** - descritor de ficheiro
+ **buf** - buffer para onde conteúdo é lido
+ **nbyte** - número max de bytes a ler

```c
ssize_t write(int fildes, const void *buf, size_t nbyte);
```
Devolve o número de bytes escritos ou erro.
+ **fildes** - descritor de ficheiro
+ **buf** - buffer com contúdo a escrever
+ **nbyte** - número max de bytes a ler

```c
off_t lseek(int fd, off_t offset, int whence);
```
`lseek()` is é uma chamada ao sistema que é usada para alterar o read/write pointer de um file descriptor.
- **fd** - o file descriptor do pointer que vai ser movido
- **offset** - o offset do pointer
- **whence** - o método em que o offset vai ser interpretado (rela, absolute, etc.)
- **return** - esta função retorna o offset do pointer (em bytes) desde o inicio do ficheiro

```c
int close(int fildes);
```
Apaga o descritor da tabela do processo.
Devolve 0 caso a operação seja executada com sucesso, -1 caso contrário.

A cada operação de leitura/escrita efetuada sobre o mesmo descritor, a posição de ler/escrever é atualizada consoante o número de bytes efetivamente lidos/escritos.

> **Nota**: Cada chamada ao sistema têm um custo elevado. Isto é importante notar para usecases mais potentes.
--- 

# Exercícios

### Exercício 1
Para resolver este exercício temos de fazer uma função que recebe um file path para dois ficheiros e lê o seu conteúdo escrevendo-o no segundo ficheiro recebido.
```c
void cp(char *file1, char *file2){
    // read file 1
    int fd1 = open(file1, O_RDONLY);
    // write file 2
    int fd2 = open(file2, O_WRONLY | O_CREAT, 0666);

    char buff[MAX_BUFFER];

    int c;
    while ((c = read(fd1, buff, MAX_BUFFER)) > 0) {
        write(fd2, buff, c);
    }

    close(fd1);
    close(fd2);
}
```

### Exercício 2


### Exercício 3
__`fd`__ -> esta variável representa o file desecriptor, ou seja, o ficheiro que queremos ler
__`line`__ -> nesta varíavel vamos guardar a linha lida
__`size`__ -> tamanho máximo de bytes que vamos ler e guardar na variável `line`

```c
int readch(int fd, char* buf) {
    return read(fd, buf, 1);
}

ssize_t readln(int fd, char* line, size_t size) {
    int next_pos = 0;
    int read_bytes = 0;

    while (next_pos < size && readch(fd, line + next_pos) > 0) {
        read_bytes++;

        if (line[next_pos] == '\n') {
            break;
        }

        next_pos++;
    }

    return read_bytes;
}
```

### Exercício 4
Para este exercício temos de verificar se o último caracter do buffer é um `\n` caso contrário a função irá colocar enumerações no meio de linhas.

```c
void nl() {
    int line_counter = 0;
    char buffer[MAX_BUFFER];
    int bytes_read = 0;
	int is_newline = 1;

    while ( (bytes_read = readln(STDIN_FILENO, buffer, MAX_BUFFER)) > 0) {
        char line_number[10] = "";
        // nl skips empty lines
        
		// estamos a verificar se a linha lida anteriormente 
		// era maior que o tamanho máximo do buffer
        if (is_newline && buffer[0] != '\n') {
            snprintf(line_number, 10, "%d: ", line_counter);
            write(STDOUT_FILENO, line_number, sizeof(line_number));
            line_counter++;
        }

        write(STDOUT_FILENO, buffer, bytes_read);

		// caso o último caracter lido passa a flag para 0
        if (buffer[bytes_read] != '\n') {
            is_newline = 0;
        }
        else {
            is_newline = 1;
        }
    }
}
```

> **snprintf**:
> Formats and stores a series of characters and values in the array buffer;
> Accepts an argument `n` which indicates the maximum number of characters to be written in the buffer.
> Is used to redirect the output of `printf()` function onto a buffer.
> Returns the number of characters that were supposed to be written on to the buffer, so only when the returned value is non-negative and less than `n`, the string has been completely written as expected.

> **printf**:
> O `printf()` não foi usado neste exercício pois ele contém um mecanismo de buffering interno onde apenas para de escrever quando encontra um `/n` ou `/0` de forma a que possa quebrar o nosso programa.

Resumidamente o que está a ser feito, é correr o `readln()` até que 

### Exercício 5
Assumindo a seguinte struct:
```c
struct person {
    char name[32];
    int age;
};

int main(int argc, char *argv[]) {
    // function that writes a struct to the binary_file and then reads it to the terminal
    int struct_size = sizeof(struct person);
    int fd = open("binary_file", O_RDWR | O_CREAT, 0666);
    int off = 0;
    
    /* if (argc != 4) { */
    /*     printf("Usage: %s <flag> <name> <age>\n", argv[0]); */
    /*     return 1; */
    /* } */
    
    if (argv[1][1] == 'i') {
        struct person new;
        struct person p;
        new.age = atoi(argv[3]);
        strcpy(new.name, argv[2]);

        lseek(fd, off, SEEK_CUR);

        while (read(fd, &p, struct_size) > 0) {
            off += struct_size;
            lseek(fd, off, SEEK_CUR);
        }

        write(fd, &new, struct_size);
    }
    else if (argv[1][1] == 'f') {
        struct person p;
        lseek(fd, off, SEEK_CUR);

        while ( (read(fd, &p, struct_size) > 0) && (strcmp(p.name, argv[2]) != 0)) {
            off += struct_size;
            lseek(fd, off, SEEK_CUR);
        }
        if (strcmp(p.name, argv[2]) == 0)
            printf("name: %s; age: %d;\n", p.name, p.age);
        else
            printf("Person not found.\n");
    }
    else if (argv[1][1] == 'u') {
        struct person p;
        struct person new;
        lseek(fd, off, SEEK_CUR);

        while ( (read(fd, &p, struct_size) > 0) && (strcmp(p.name, argv[2]) != 0)) {
            off += struct_size;
            lseek(fd, off, SEEK_CUR);
        }

        if (strcmp(p.name, argv[2]) == 0) {
            new.age = atoi(argv[3]);
            strcpy(new.name, argv[2]);
            printf("name: %s; age: %d;\n", new.name, new.age);
            write(fd, &new, struct_size);
        }
        else
         printf("Person not found.\n");
    }
    else if (argv[1][1] == 'p') {
        struct person p;
        lseek(fd, off, SEEK_CUR);

        while ( (read(fd, &p, struct_size) > 0)) {
            printf("name: %s; age: %d;\n", p.name, p.age);
            off += struct_size;
            lseek(fd, off, SEEK_CUR);
        }
    }
        
    close(fd);

    return 0;
}
```
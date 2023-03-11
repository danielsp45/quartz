---
date: 28/02/2023
---
## Arrays em Java
Os arrays em Java são uma entidade para agregar entidades do mesmo tipo de dados.
Sobre este array podemos invocar métodos de Java que realizam operações sobre este. 
Em java quando se tenta acessar um índice do array que não existe, este vai devolver um erro, ao invés de `NULL`.

### Exercício 1
```java
public class Main {  
    public static void main(String[] args) {  
        Scanner scanner = new Scanner(System.in);  
  
        int [] aux = new int[5];  
        int a0 = 0;  
        int a1 = 0;  
        int a2 = 0;  
        int a3 = 0;  
        int a4 = 0;  
  
        for(int j = 0; j < 5; j++) {  
            System.out.println("Insira um inteiro: ");  
            aux[j] = scanner.nextInt();  
        }  
  
  
        for(int j = 0; j < 5; j++) {  
            System.out.println(""+aux[j]);  
        }  
  
        Ficha2 ficha = new Ficha2();  
        ficha.setArray(aux);  
  
        ficha.doisIndices(0, 3);  
        String aux2 = ficha.toString();  
        System.out.println(aux2);  
    }  
}
```
```java
public class Ficha2 {  
    private int [] array;  
  
    public Ficha2(){  
        this.array = new int [5];  
    }  
  
    public void setArray(int [] a) {  
        this.array = a;  
    }  
  
    public void doisIndices(int a, int b) {  
        this.array = Arrays.copyOfRange(this.array, a, b);  
    }  
  
    public String toString(){  
        String aux = "";  
  
        for(int j = 0; j < this.array.length; j++) {  
            aux = aux + this.array[j];  
        }  
        return aux;  
    }  
}
```

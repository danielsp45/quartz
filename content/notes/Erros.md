[[MNOnL]]

Este capítulo é referente aos erros induzidos pelos cálculos da calculadora ou do computador, quando procedemos à utlização dos mesmos. 

Como é obvio, não existem equipamentos de com uma precisão de 100%, sejam eles balanças, voltímetros, amperímetros, termómetros,  barómetros, etc. Desta forma qualquer dado de entrada contêm uma *imprecisão inerente*, isto é, não há como os evitar.  A isto chamamos __incerteza nos dados__. 

Assim como o computador não tem bits infinitos para poder realizar os calculos de forma exata. Ou quando usamos o pi, não sendo possível utilizar todos os seus números, tendo de o arredondar. Desta forma, os erros de arredondamentos vão-se acumulando à medida que realizamos os cálculos, ampliando cada vez mais o nível de incerteza nos resultados. Aqui já estamos a lidar com __erros de arredondamento__. 

Mas também surgem erros de truncamento, sendo eles:
+ na substituição de um problema contínuo em um *discreto. 
+ na substituição de um processo de calculo finito por um finito (*métodos iterativos*), pois como é obvio, na prática não é possível realizar cálculos infinitos. 
Como por exemplo, quando se pretende calcular $e^x$ usando a expansão em série de Taylor da função:

$$e^x = 1 + x + \frac{1}{2!}x^2 + \frac{1}{3!}x^3 + \frac{1}{3!}x^3 + ... + \frac{1}{n!}x^n + ... +$$
Se se usar apenas o *n* termos da série (se a "truncarmos" no *enésimo* termo) obtém-se uma aproximação de $e^x$.

## Formato da vírgula flutuante
$$fl(x) = \pm0.d_1d_2...d_3 \times b^e$$
+ a mantissa é o número de dígitos significativos que define o comprimento da palavra *k. 
+ $b$ é a base de representação, tipo $10^3$
+ $e$ é o expoente
Se $d_1 \ne 0$ o formato diz-se *normalizado.  

## Erro absoluto
O erro absoluto representa o o valor de erro ou engano a medir algo.
Considerando $\bar{x}$ é o valor exato de um número e $x$ o valor aproximado desse mesmo número:
+ $d_x = |\bar{x} - x|$ -> **erro absoluto** (desconhecido!), pois estamos a calcular o módulo da diferença entre o valor real e o valor estimado. 
+ $|\bar{x} - x| < \delta$ -> limite superior do erro absoluto, isto significa que estamos a estabelecer o limite superior do erro;
+ $\bar{x} \in [x - \delta_x, x+ \delta_x]$ -> intervalo de incerteza, este intervalo representa o espaço de valores onde o valor real pode estar contido. 

## Erro relativo
O erro relativo compara o erro absoluto com o tamanho real do objeto medido. Para calcular o erro relativo, também é preciso calcular o **erro absoluto**. 
+ $r_x = \frac{|\bar{x}-x|}{|\bar{x}|}, \bar{x} \ne 0$  -> **erro relativo** (desconhecido!)
+ $r_x \approx \frac{|\bar{x}-x|}{|\bar{x}|}\leq\frac{\delta_x}{|x|}$ (100%r_x - percentagem de erro)
Tal como podemos observar, o erro relativo é calculado através da divisão entre o valor do erro absoluto pelo valor real do que estamos a medir. 

## Majorante de um erro
Associado a um número $x$ está o limite superior do erros absoluto $\delta_x$ que é calculado pela metade da última casa décimal de $x$.  
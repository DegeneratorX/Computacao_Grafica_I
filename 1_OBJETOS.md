# Representação de Objetos

Representamos objetos tridimensionais através de:

- Lista de vértices
- Lista de arestas
- Lista de faces

Podemos subdividir praticamente qualquer objeto tridimensional ou bidimensional em faces triangulares.

Triângulos nunca podem ser não-planares. Ou seja, todo triângulo é obrigatoriamente plano. A partir de um quadrilátero, o objeto pode não ser plano, e isso leva a cálculos mais complexos para renderizar algum modelo tridimensional, pois levaria em consideração a sua curvatura. Quadriláteros são utilizados para modelar rostos.

Por hora irei trabalhar apenas com triângulos.

O triângulo é eficiente em memória e processamento, pois o mínimo de pontos para se definir uma superfície plana são 3, e portanto qualquer forma pode ser simulada através de triângulos.

O processo de conversão de faces não triangulares para triangulares é chamado de **triangulação**. Existem diversos algoritmos eficientes que buscam triangular superfícies de forma automática.

Veja esse cubo abaixo por exemplo. As duas figuras mostram ele opaco e transparente para melhor visualização. As linhas laranjas representam a triangulação. A escolha para formar as arestas de triangulação (linhas laranja) são normalmente aleatórias.

![image](https://user-images.githubusercontent.com/98990221/209157097-8add3d6a-8585-48f0-83c7-ee0827ba8591.png)

- Essas são as coordenadas dos vértices (observe o cartesiano 3D).

| Lista de Vértices  | X | Y | Z |
| ------------- | --- | --- | --- |
| ID: 0 | 0 | 0 | 0 |
| ID: 1 | 0 | 0 | 2 |
| ID: 2 | 2 | 0 | 2 |
| ID: 3 | 2 | 0 | 0 |
| ID: 4 | 0 | 2 | 0 |
| ID: 5 | 0 | 2 | 2 |
| ID: 6 | 2 | 2 | 2 |
| ID: 7 | 2 | 2 | 0 |

- Essas são as listas das arestas (não preciso enumerar na imagem, basta fazer uma contagem de todas).
> Nota: para contabilizar arestas, basta eu pegar o início e fim dos vértices que formam ela. O início e fim são escolhidos aleatoriamente. Por convenção e organização, o interessante é contar por faces.

| Lista de Arestas  | Vértice Inicial | Vértice Final |
| ------------- | --- | --- |
| ID: 0 | 0 | 1 |
| ID: 1 | 1 | 2 |
| ID: 2 | 2 | 3 |
| ID: 3 | 3 | 0 |
| ID: 4 | 4 | 5 |
| ID: 5 | 5 | 6 |
| ID: 6 | 6 | 7 |
| ID: 7 | 7 | 4 |
| ID: 8 | 0 | 4 |
| ID: 9 | 1 | 5 |
| ID: 10 | 2 | 6 |
| ID: 11 | 3 | 7 |
| ID: 12 | 2 | 7 |
| ID: 13 | 5 | 7 |
| ID: 14 | 5 | 2 |
| ID: 15 | 1 | 4 |
| ID: 16 | 1 | 3 |
| ID: 17 | 3 | 4 |

- Por fim, as faces.
> Nota: as arestas são contabilizadas no sentido anti-horário.

| Lista de Faces  | Aresta 1 | Aresta 2 | Aresta 3 |
| ------------- | --- | --- | --- |
| ID: 0 | 6 | 10 | 12 |
| ID: 1 | 12 | 2 | 11 |
| ID: 2 | 7 | 4 | 13 |
| ID: 3 | 13 | 5 | 6 |
| ID: 4 | 5 | 14 | 10 |
| ID: 5 | 9 | 1 | 14 |
| ID: 6 | 4 | 15 | 9 |
| ID: 7 | 8 | 0 | 15 |
| ID: 8 | 1 | 16 | 2 |
| ID: 9 | 3 | 16 | 10 |
| ID: 10 | 11 | 17 | 7 |
| ID: 11 | 3 | 8 | 17 |


# Curvas Paramétricas

As **curvas paramétricas** são curvas originadas quando se tem um input para uma função, e resulta em 2 ou mais outputs. 

No caso de uma curva bidimensional, inserir um valor em uma função produz uma coordenada X e uma coordenada Y, e diferentes valores podem gerar diferentes coordenadas, de tal forma que ao ligar todos os pontos, pode produzir um gráfico não linear (curva).

$$F(t) = \begin{pmatrix}
x(t) \\
y(t)
\end{pmatrix}$$

Exemplo: $x = 2t$ e $y = t² - 1$. Para $t = 1$, produz $x = 2$ e $y = 0$.


## Curvas de Bézier Cúbicas

As **curvas de Bézier Cúbicas** são necessárias para produzir desenhos em 3D. Segue a seguinte função:

$$\large F(t) = \begin{pmatrix}
x(t) \\
y(t) \\
z(t)
\end{pmatrix} = a+bt+ct^{2}+dt^{3}$$

E sua derivada:

$$\large F'(t) = \begin{pmatrix}
x'(t) \\
y'(t) \\
z'(t)
\end{pmatrix} = b+2ct+3dt^{2}$$


Onde a, b, c, d são valores que podem ser diferentes para cada coordenada x, y e z. Tanto a função quanto a derivada serão utilizadas.

### Condições de contorno

Precisamos achar os valores a, b, c, d para cada caso. Pra isso utilizamos as seguinte notação:

- Seja $P_{0}$, $P_{1}$, $P_{2}$ e $P_{3}$ pontos aleatórios de controle sobre um plano. Seja uma linha, e $P_{0}$ sempre o ponto de controle inicial dessa linha e $P_{3}$ o ponto de controle final.
- $P_{1}$ e $P_{2}$ são pontos que estão entre $P_{0}$ e $P_{3}$.

![image](https://user-images.githubusercontent.com/98990221/210604462-2419a145-0947-4689-b5f0-8fe481a4dbc5.png)

- Sempre que um ponto de controle aparece entre $P_{0}$ e $P_{3}$, ele "puxa" a linha de forma tangente, formando uma curva paramétrica. Ou seja,, nesse caso $P_{1}$ e $P_{2}$.

Usamos o intervalo $[0, 1]$ substituinto em $t$ pra definir os pontos infinitesimais dessa linha. O ponto 0 define o ponto inicial da curva $P_{0}$, e o ponto 1 define o ponto final dessa curva $P_{3}$.

Já os pontos $P_{1}$ e $P_{2}$ (que não pertencem a curva) são subtraídos por $P_{0}$ e $P_{3}$, respectivamente, para representar o vetor tangente a curva na saída e na chegada, respectivamentes, pois são as tangentes correspondentes a esses pontos (inicio e fim da linha). E a subtração deles deve resultar na derivada da função $a+bt+ct^{2}+dt^{3}$ e substituição de t por 0 ou 1.

**Exemplo pra ficar mais claro:**

- Ponto $P_{0}$, substituímos por 0.

$$\large P_{0} = P(0) = \begin{pmatrix} 
x(0) \\ 
y(0) \\ 
z(0) 
\end{pmatrix} = a+b0+c0^{2}+d0^{3} \Rightarrow \boxed{P_{0} = a}$$

- Ponto $P_{3}$, substituímos por 1.

$$\large P_{3} = P(1) = \begin{pmatrix} 
x(1) \\ 
y(1) \\ 
z(1) 
\end{pmatrix} = a+b1+c1^{2}+d1^{3} \Rightarrow \boxed{P_{3} = a+b+c+d}$$

- $P_{1}-P_{0}$, substituímos a derivada por 0

$$\large P_{1}-P_{0} = P'(0) = \begin{pmatrix}
x'(0) \\
y'(0) \\
z'(0)
\end{pmatrix} = b + 2c0 + 3d0^{2} \Rightarrow \boxed{P_{1}-P_{0} = b}$$

- $P_{3}-P_{2}$, substituímos a derivada por 1.

$$\large P_{3}-P_{2} = P'(1) = \begin{pmatrix}
x'(1) \\
y'(1) \\
z'(1)
\end{pmatrix} = b + 2c1 + 3d1^{2} \Rightarrow \boxed{P_{3}-P_{2} = b + 2c + 3d}$$

Nesse momento temos 4 equações e 4 variáveis a, b, c, d. Podemos montar um sistema linear.

Sabendo que $b = P_{1}-P_{0}$ e $a = P_{0}$, substituímos a segunda e a quarta equação:

$$\large\overset{a}{P_{0}}+\overset{b}{P_{1}-P_{0}}+c+d=P_{3} \Rightarrow \boxed{P_{3}-P_{1} = c+d}$$

$$\overset{b}{P_{1}-P_{0}}+2c+3d = P_{3}-P_{2} \Rightarrow \boxed{P_{3}-P_{2}+P_{0}-P_{1}=2c+3d}$$

Multiplicando 2x a primeira equação e igualando $2c = 2c$ da primeira com a segunda, resulta em:

$$\large\boxed{d = -P_{3} - P_{2} + P_{0} + P_{1}}$$

Pegando esse $d$ descoberto e substituindo na primeira equação, temos que

$$\large c = P_{3} - P_{1} -(-P_{3}-P_{2}+P_{0}+P_{1}) \Rightarrow \boxed{c = -P_{0} - 2P_{1} + P_{2} + 2P_{3}}$$

Com a, b,c, d descobertos, agora podemos substituir a equação original $P(t)$:

$$\large P(t) = P_{0} + (P_{1}-P_{0})t + (-P_{0}-2P_{1}+P_{2}+2P_{3})t^{2}+(-P_{3}-P_{2}+P_{0}+P_{1})t^{3}$$

Isolando os pontos $P_{0}$, $P_{1}$, $P_{2}$, $P_{3}$, temos a equação final:

$$\Large\boxed{P(t) = P_{0}(1-t-t^{2}+t^{3})+P_{1}(t-2t^{2}+t^{3})+P_{2}(t^{2}-t^{3})+P_{3}(2t^{2}-t^{3})}$$

> Nota: Perceba que se eu substituir $t$ por 0, dará $P_{0}$. Se eu substituir $t$ por 1, dará $P_{3}$. Se não der, está errado.

Cada parte dessa função $P(t)$ é chamada de **Função Blending de Bézier**, e é definida por $B_{i}(t)$. Nesse caso:

- $\large B_{0}(t) = P_{0}(1-t-t^{2}+t^{3})$
- $\large B_{1}(t) = P_{1}(t-2t^{2}+t^{3})$
- $\large B_{2}(t) = P_{2}(t^{2}-t^{3})$
- $\large B_{3}(t) = P_{3}(2t^{2}-t^{3})$

$$\Large P(t) = B_{0}(t)+B_{1}(t)+B_{2}(t)+B_{3}(t)$$

Uma curva de Bézier cúbica só pode ter 1 inflexão, pois é um polinômio de grau cúbico (o gráfico de terceiro grau possui uma inflexão). Mais do que uma inflexão, representa um polinômio de quarto grau ou maior. Nesse caso se aplica uma técnica de emendar várias curvas de Bézier cúbicas ou outro tipo de curva, como a curva B-Spline.

![image](https://user-images.githubusercontent.com/98990221/210648386-149bccb2-8b75-4851-bd2d-f09f507b9557.png)


### Construção Recursiva de Casteljau

Para compatibilizar a construção de Curvas de Bézier Cúbicas através do uso do método recursivo de Casteljau, é preciso pegar os vetores de derivada $P_{1}-P_{0}$ e $P_{3}-P_{2}$ e multiplicar por 3. E depois refazer os cálculos acima, só que multiplicados por 3. Assim:

- $\large 3(P_{1}-P_{0})$
- $\large 3(P_{3}-P_{2})$
- Mantem o $P_{0}$ e $P_{3}$.

Refazendo tudo com o objetivo de chegar em $P(t)$ novamente, fica:

$$\Large P(t) = P_{0}[(1-t)^{3}] + P_{1}[3(1-t)^{2}t]+P_{2}[3(1-t)t^{2}]+P_{3}[t^3]$$

Uma forma alternativa de desenvolver o polinômio de Bézier é usando a construção recursiva de Casteljau, isto é o polígono de controle de um polinômio de Bézier tem três segmentos:

- $\large S_{1}(t) = P_{0}(1-t)+P_{1}t$
- $\large S_{2}(t) = P_{1}(1-t)+P_{2}t$
- $\large S_{3}(t) = P_{2}(1-t)+P_{3}t$

Essas funções $S$ são segmentos, e estão representadas nessa figura. Basicamente segmentos de um ponto para outro:

![image](https://user-images.githubusercontent.com/98990221/217605836-05a445c8-f597-44b5-9d2f-3637f87b1a89.png)

Perceba que se eu substituir $t$ por $0$ nessas equações, se obtém o ponto inicial. E se eu substituir por $1$, obtenho o ponto final. Ou seja, $t = 0.5$ será um ponto no meio de $S_{1}$, $S_{2}$ ou $S_{3}$, e a escolha pra $t$ é infinitesimal, desde que esteja no intervalo $(0,1)$.


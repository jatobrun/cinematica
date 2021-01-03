# Rigid-Body Motions

## Introduccion
En el anterior capitulo aprendimos que necesitamos al menos 6 numeros para determinar la posicion y orientacion de un cuerpo rigido en el espacio. En este capitulo desarrollaremos una forma sistematica de describir la posicion y orientacion de un cuerpo rigido usando cambiando los marcos de referencia. Utilizaremos una matriz 4x4, esto es una forma de representacion implicita como vimos en el capitulo pasado. 

⭕️ Los 6 parametros es obtenido por 16 parametros de la matriz 4x4 mas 10 restricciones

⭕️ Esta matriz no solo nos sirve para representar la configuracion del espacio sino tambien para realizar operaciones como traslacion y rotacion.

⭕️ Tambien para cambiar la representacion de un vector que tiene coordenadas bajo un marco de referencia a otro.

⭕️ Estas 3 operaciones las podemos realizar de forma sencilla con la matriz 4x4 aplicando algebra lineal es por esta razon "simplicidad" que escogemos esta configuracion.

⭕️ Spatial velocity or twist es el vector de velocidad de 6 dimensiones 3 para las velocidades lineales y 3 para las angulares.

⭕️ Spatial Force or wrench es el vector de fuerzas o momentos de 6 dimensiones.

✍🏽 **Vector Libre:** Es una cantidad geometrica con magnitud y direccion. Se lo llama libre porque no es necesario que este anclado a una referencia.

    🔆 La velocidad lineal es un ejemplo de un vector libre, la magnitud del vector es la velocidad y la direccion del vector es la direccion de la velocidad.

⭕️ Un marco de referencia lo podemos anclar en cualquier parte del espacio. 

⭕️ Normalmente asumimos que tenemos un marco referencia fijo `space frame`, asimismo asumimos que al menos un marco de referencia es anclado a un cuerpo rigido `body frame`, el mismo que acompana al cuerpo en cualquier instante de tiempo.

> 💥 **Importante:** Todos los marcos de referencia son estacionarios es decir que aunque el cuerpo se este moviendo nos referimos al marco estacionario coincidente con el marco del cuerpo en un instante de tiempo particular.

## 3.1 Movimientos de un cuerpo rigido en el plano

⭕️ Solo necesitamos la posicion y la orientacion del objeto para representar su configuracion.

⭕️ Representaremos el eje del robot en terminos del eje referencia.

⭕️ La posicion la representaremos con un vector de n dimensiones y la orientacion con una matriz nxn. Siendo n la dimension del espacio en el que estamos.

⭕️ Esta matriz para determinar la orientacion se la conoce como matriz de rotacion. 

    1️⃣ Cada vector dentro de la matriz es unitario.

    2️⃣ Todos los vectores deben ser ortogonales entre ellos.

⭕️ La matriz de rotacion se la puede usar para 3 cosas:

    1️⃣ Para representar la configuracion del cuerpo con respecto a la referencia.

    2️⃣ Para cambiar el eje de referencia.

    3️⃣ Para desplazar el cuerpo.

⭕️ La rotacion y desplazamiento de un cuerpo se puede representar como un rotacion sobre un punto fijo en el espacio. Esto se lo conoce como `screw motion`.

⭕️ Screw motion tiene 3 parametros $ (\beta, s_x, s_y) $, donde $(s_x, s_y)$ es la posicion del punto fijo y β es el angulo al que voy a girar el cuerpo con respecto al punto fijo.

⭕️ Otra forma de representar el Screw motion es por medio de la velocidad angular y lineal formando el vector $ S = (\omega, \upsilon_x,\upsilon_y)$ el cual es una representacion del `screw axis`.

⭕️ Multiplicar el vector S por un angulo $\theta$ nos da como resultado el desplazamiento neto.  

⭕️ El desplazamiento neto obtenido por rotar un cuerpo sobre el `screw axis` a un angulo $\theta$ es equivalente al desplazamiento obtenido por rotar un cuerpo sobre el `screw axis` a un velocidad $\dot{\theta}$

⭕️ Existen otras formas de representar la orientacion aparte de las coordenadas exponenciales como los angulos de euler, los parametros de Cayley-Rodrigues y los cuarterniones.

> ⚠️ Utilizaremos las coordenadas exponenciales para representar la configuracion de un cuerpo rigido basados en el teorema de Chasles-Mozzi 

✍🏽 **Coordenadas exponenciales:** Son las que definen un eje de rotacion y un angulo para rotar alrededor del eje.

✍🏽 **Teorema de Chasles-Mozzi:** Cualquier desplazamiento de un cuerpo rigido puede ser obtenido por rotaciones finitas alrededor de un eje fijo.

## 3.2 Rotaciones y Velocidades Angulares

### 3.2.1 Matrices de Rotacion

⭕️ De los nueve parametros en una matriz de rotacion 3x3, 6 de esos son restricciones y 3 se los escoge de manera independiente.

⭕️ Las siguientes condiciones se deben cumplir para R, la cual es la matriz de rotacion.

$ \quad $ 1️⃣ Norma unitaria: $\hat{x}_b, \hat{y}_b, \hat{z}_b$ todos son vectores unitarios.


```math
    r_{11}^2 + r_{21}^2 + r_{31}^2 = 1, \newline
    r_{12}^2 + r_{22}^2 + r_{32}^2 = 1, \newline
    r_{13}^2 + r_{23}^2 + r_{33}^2 = 1, 
```

$ \quad $ 2️⃣ Ortogonalidad: $\hat{x}_b \cdot \hat{y}_b = \hat{x}_b \cdot \hat{z}_b = \hat{y}_b \cdot \hat{z}_b = 0$.

```math

    (r_{11}) (r_{12}) + (r_{21})(r_{22}) + (r_{31})(r_{32}) = 0, \newline

    (r_{12}) (r_{13}) + (r_{22})(r_{23}) + (r_{32})(r_{33}) = 0, \newline

    (r_{11}) (r_{13}) + (r_{21})(r_{23}) + (r_{31})(r_{33}) = 0,

```

Estas 6 restricciones las podemos expresar en su forma compacta como

```math

    R^TR = I,

```
donde $I$ es la matriz identidad y $R^T$ es la transpuesta de la matrix R.

> ⚠️ Ademas agregaremos una condicion extra la cual nos asegurara que podemos utilizar ejes de referencia utilizando la regla de la mano derecha 

```math

    \det{R}= 1.

```
✍🏽 **Grupo Ortogonal Especial:** Son todas las matrices reales 3x3 $R$ que cumplen $R^{T}R = I \space y \det{R} = 1$.

✍🏽 **Grupo Ortogonal Especial:** Son todas las matrices reales 2x2 $R$ que cumplen $R^{T}R = I \space y \det{R} = 1$.

$R \in SO(2)$ se puede escribir como:

```math

    R = \begin{bmatrix} 
        r_{11} & r_{12} \\
        r_{21} & r_{22}
        \end{bmatrix}
      = \begin{bmatrix}
        \cos{\theta} & -\sin{\theta} \\
        \sin{\theta} & \cos{\theta}
        \end{bmatrix}, \theta \in [0, 2\pi)

```

⭕️ $SO(2)$ se usa para configuraciones de orientacion planares mientras que $SO(3)$  configuraciones de orientaciones espaciales.

#### Propiedades

⚫️ **Cerradura:** $AB$ debe pertenecer al conjunto.

⚫️ **Asociativa:** $(AB)C = A(BC)$.

⚫️ **Identidad:** Debe existir un elemento $I$ en el grupo que $IA =AI = A$.

⚫️ **Inverso:** $AA_{-1} = A_{-1}A = I$.

#### Usos 

1️⃣ Representar orientacion.

2️⃣ Rotar un cuerpo.

3️⃣ Cambiar el eje de referencia el cual el cuerpo esta representado

🔆 **Ejemplo**
![ejemplo](../docs/images/ejemplo.png)
El mismo espacio y el mismo punto p representado en tres ejes distintos con 3 orientaciones distintas.

Orientacion de los 3 ejes relativos a $\{s\}$ se puede escribir como
```math
    R_a= 
    \begin{matrix}
        \hat{x}_a \space \space \hat{y}_a \space \space \hat{z}_a \\ 
        \begin{bmatrix}
            1 & 0 & 0 \\
            0 & 1 & 0 \\
            0 & 0 & 1 \\
        \end{bmatrix}
    \end{matrix}, \quad
    R_b= 
    \begin{matrix}
        \hat{x}_b \quad \hat{y}_b \quad \hat{z}_b \\ 
        \begin{bmatrix}
            0 & -1 & 0 \\
            1 & 0 & 0 \\
            0 & 0 & 1 \\
        \end{bmatrix}
    \end{matrix}, \quad
    R_c= 
    \begin{matrix}
        \hat{x}_c \quad \hat{y}_c \quad \hat{z}_c \\ 
        \begin{bmatrix}
            0 & -1 & 0 \\
            0 & 0 & -1 \\
            1 & 0 & 0 \\
        \end{bmatrix}
    \end{matrix}
```

La ubicacion del punto en estos ejes seria

```math
    p_a = \begin{bmatrix}
            1 \\
            1 \\
            0 \\
           \end{bmatrix}, \quad
    p_b = \begin{bmatrix}
            1 \\
            -1 \\
            0 \\
           \end{bmatrix}, \quad
    p_b = \begin{bmatrix}
            0 \\
            -1 \\
            -1 \\
           \end{bmatrix}, \quad      

```

> ⚠️ **Nota:** $\{b\}$ se obtiene rotando $\{a\}\space 90^{\circ}$  alrededor de $\hat{z}_a$ y $\{c\}$ se obtiene rotando $\{b\}\space -90^{\circ}$  alrededor de $\hat{y}_b$

------

**Representando Orientacion:**

⚫️$R_{sc}$ Matriz de rotacion del eje $\{c\}$ relativo a $\{s\}$.

⚫️$R_{ac}R_{ca} = I$.

⚫️$R_{ac} = R_{ca}^{-1} = R_{ca}^T$.

------

**Cambiando el eje de referencia:**

⭕️ La matriz de rotacion $R_{ab}$ representa la orientacion de $\{b\}$ en $\{a\}$, $R_{bc}$ representa la orientacion de $\{c\}$ en $\{b\}$ para calcular la matriz de rotacion de $\{c\}$ en $\{a\}$ basta con.

```math

    R_{ac} = R_{ab}R_{bc}

```

⭕️ Esto mismo podemos aplicar para vectores

```math 

    R_{ab}p_{b} = p_a

```

-----

**Rotar un cuerpo:**

⭕️ $R_{sb} = R = Rot(\hat{\omega}, \theta)$

⭕️ Para rotar un punto $p_s$ es tan facil como hacer $p'_s = Rp_s$

⭕️ $R_{sb'} = RR_{sb}$; rotar por R en $\{s\}$ eje ($R_{sb}$)

⭕️ $R_{sb''} = R_{sb}R$; rotar por R en $\{b\}$ eje ($R_{sb}$)

### 3.2.2 Velocidades Angulares

⭕️ Si tenemos un marco de referencia con ejes $\{\hat{x}, \hat{y}, \hat{z}\}$ el cual esta anclado a un cuerpo rigido, si queremos determinar las derivadas con respecto al tiempo de los ejes unitarios. Tenemos que solo la direccion de los ejes varia con respecto al tiempo.

⭕️ la velocidad angular del marco de rotacion.

$\qquad$⚫️ $\dot{R}R^{-1} = \omega_s$

$\qquad$⚫️ $R^{-1}\dot{R} = \omega_b$

⭕️ La velocidad angular de un eje.

$\qquad$ ⚫️ $\omega_c = R_{cd}\omega_d$

⭕️ velocidad lineal de ejes 
$\qquad$⚫️ $\dot{r}_i = \omega_s \times r_i, \qquad i = 1,2,3$
### 3.2.3 Representacion de rotacion usando coordenadas exponenciales


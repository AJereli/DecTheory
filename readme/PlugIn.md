***Байесовские алгоритмы классификации***  
Байесовские алгоритмы классификации основаны на принципе максимума апостериорной вероятности : для классифицируемого объекта вычисляются плотности распределения ![](http://latex.codecogs.com/gif.latex?%5Cinline%20p%28x%7Cy%29%20%3D%20p_y%28x%29)  — **_функции правдоподобия_** классов, по ним вычисляются ***апостериорные вероятности*** - ![](http://latex.codecogs.com/gif.latex?P%20%5Cleft%20%5C%7By%7Cx%20%5Cright%20%5C%7D%20%3D%20P_yp_y%28x%29), где ![](http://latex.codecogs.com/gif.latex?%5Cinline%20P_y)- ***априорные вероятности*** классов. Объект относится к классу с максимальной апостериорной вероятностью.

*Задача классификации* - получить алгоритм ![](http://latex.codecogs.com/gif.latex?%5Cinline%20a%3A%5C%3B%20X%5Cto%20Y), способный классифицировать произвольный объект ![](http://latex.codecogs.com/gif.latex?%5Cinline%20x%20%5Cin%20X).  

1)  ***Построение классификатора при известных плотностях***  
![](http://latex.codecogs.com/gif.latex?%5Cinline%20%5Clambda_y) - штраф за неправильное отнесение объекта класса ***𝑦***.  
Если известны ![](http://latex.codecogs.com/gif.latex?%5Cinline%20P_y)  и ![](http://latex.codecogs.com/gif.latex?%5Cinline%20p_%7By%7D%28x%29), то минимум среднего риска ![](http://latex.codecogs.com/gif.latex?%5Cinline%20R%28a%29%20%3D%20%5Csum_%7By%5Cepsilon%20Y%7D%20%5Csum_%7Bs%5Cepsilon%20Y%7D%20%5Clambda_yP_yP%28A_s%7Cy%29), ![](http://latex.codecogs.com/gif.latex?%5Cinline%20A_s%20%3D%20%5Cbigl%5C%7Bx%20%5Cin%20X%7Ca%28x%29%3Ds%5Cbigr%5C%7D%2C)  достигается алгоритмом ![](http://latex.codecogs.com/gif.latex?%5Cinline%20a%28x%29%20%3D%20%5Carg%5Cmax%20%5Clambda_yP_yp_y%28x%29)

2) ***Восстановление плотностей по выборке***  
По подвыборке  класса *y* строим эмпирические оценки  ![](http://latex.codecogs.com/gif.latex?%5Cinline%20P_y) (доля объектов в выборке) и ![](http://latex.codecogs.com/gif.latex?%5Cinline%20p_y%28x%29).  
Три метода:  
**1)Параметрический** если плотности нормальные (гауссовские) - НДА и ЛДФ;  
**2)Непараметрический** - оценка Парзена - Розенблатта, метод парзеновского окна;   
**3)Разделение смеси** производится _ЕМ-алгоритмом_. Плотности компонент смеси (гауссовские плотности) - радиальные функции,метод радиальных базисных функций.

***NDA. Plug - in algorithm***

**Нормальный дискриминантный анализ** - один из вариантов байесовской классификации,где восстанавливаемые плотности - многомерные гауссовские: ![](http://latex.codecogs.com/gif.latex?%5Cinline%20N%28x%3B%5Cmu%20%2C%5Csum%29%20%3D%20%5Cfrac%7B1%7D%7B%5Csqrt%7B%282%5Cpi%29%5En%5Cleft%20%7C%20%5Csum%20%5Cright%20%7C%7D%7D%20%5Cexp%20%5Cleft%20%28%20%5Cfrac%7B-1%7D%7B2%7D%20%28x-%5Cmu%29%5ET%20%5Csum%5E%7B-1%7D%28x-%5Cmu%29%5Cright%20%29%2C%20x%20%5Cin%20%5Cmathbb%7BR%7D%5En), где ![](http://latex.codecogs.com/gif.latex?%5Cmu%20%5Cin%20%5Cmathbb%7BR%7D%5En) - мат. ожидание (центр), а ![](http://latex.codecogs.com/gif.latex?%5Csum%20%5Cin%20%5Cmathbb%7BR%7D%5E%7Bn%5Ctimes%20n%20%7D) - ковариационная матрица - симметричная, положительная и невырожденная.  

Находим параметры нормального распределения ![](http://latex.codecogs.com/gif.latex?%5Cinline%20%5Cmu_y%20%3D%20%5Cfrac%20%7B1%7D%7Bl_y%7D%20%5Csum_%7Bx_i%3Ay_i%20%3D%20y%7Dx_i)**(1)**, ![](http://latex.codecogs.com/gif.latex?%5Cinline%20%5Csum_y%20%3D%20%5Cfrac%7B1%7D%7Bl_y-1%7D%20%5Csum_%7Bx_i%3Ay_i%20%3D%20y%7D%28x_i%20-%20%5Cmu_y%29%28x_i-%5Cmu_y%29%5ET)**(2)**  для ![](http://latex.codecogs.com/gif.latex?%5Cinline%20y%20%5Cin%20Y) согласно **принципу максимального правдоподобия**, подставляем в формулу ***ОБРП*** и получаем ***подстановочный алгоритм***, или ***линейный дискриминант Фишера*** (если ковариационные матрицы равны для всех классов).

[src](../PlugIn.R) 

Для сгенерированных с помощью функционала библиотеки **MASS** данных (выборка с двумя классами) вычисляем центры по **(1)**:

```R
  cols <- dim(objects)[2]
  mu <- matrix(NA, 1, cols) #создаём вектор
  for (col in 1:cols)
  {
    mu[1, col] = mean(objects[,col]) #подсчёт среднего значения для каждой из компонент
  }
```
Далее, находим ковариационные матрицы классов по **(2)** :
```R
  rows <- dim(objects)[1]
  cols <- dim(objects)[2]
  sigma <- matrix(0, cols, cols) #нулевая квадратная матрица 
  for (i in 1:rows)
  {
    sigma <- sigma + (t(objects[i,] - mu) %*% # - оператор умножения матриц, t - transpose
                        (objects[i,] - mu)) / (rows - 1)
  }
```
Из теоремы задания байесовским классификатором квадратичной поверхности получим её уравнение
![](https://latex.codecogs.com/gif.latex?%5Clambda_s%20P_sp_s%28x%29%20%3D%20%5Clambda_t%20P_tp_t%28x%29%2C)  
Логарифмируя, ![](https://latex.codecogs.com/gif.latex?%5Cln%20p_s%28x%29%20-%20%5Cln%20p_t%28x%29%20%3D%20C_%7Bst%7D),![](https://latex.codecogs.com/gif.latex?C_%7Bst%7D%20%3D%20%5Cln%28%5Clambda_tP_t/%5Clambda_sP_s%29) - константа, не зависит от x.
Осталось вычислить коэффициенты квадратичного дискриминанта из двух квадратичных форм ln p(x) : ![](https://latex.codecogs.com/gif.latex?%5Cln%20p_y%28x%29%20%3D%20-%5Cfrac%7Bn%7D%7B2%7D%5Cln2%5Cpi%20-%20%5Cfrac%7B1%7D%7B2%7D%20%5Cln%20%7C%5Csum_y%7C-%5Cfrac%7B1%7D%7B2%7D%28x%20-%20%5Cmu_y%29%5ET%5Csum%5E%7B-1%7D_y%28x-%5Cmu_y%29)
```R
# кривая второго порядка: a*x1^2 + b*x1*x2 + c*x2 + d*x1 + e*x2 + f = 0
  invSigma1 <- solve(sigma1)
  invSigma2 <- solve(sigma2)
  f <- log(abs(det(sigma1))) - log(abs(det(sigma2))) +
    mu1 %*% invSigma1 %*% t(mu1) - mu2 %*% invSigma2 %*%
    t(mu2);
  alpha <- invSigma1 - invSigma2
  a <- alpha[1, 1]
  b <- 2 * alpha[1, 2]
  c <- alpha[2, 2]
  beta <- invSigma1 %*% t(mu1) - invSigma2 %*% t(mu2)
  d <- -2 * beta[1, 1]
  e <- -2 * beta[2, 1]
  ##решаем проблему возвращения из функции набора параметров передачей их map-ом
  return (c("x^2" = a, "xy" = b, "y^2" = c, "x" = d, "y" = e, "1" = f))
```

1. ДАНО: Робот находится в произвольной клетке ограниченного прямоугольного поля без 
   внутренних перегородок и маркеров.
   РЕЗУЛЬТАТ: Робот — в исходном положении в центре прямого креста из маркеров, расставленных вплоть до внешней рамки.

2. ДАНО: Робот - в произвольной клетке поля (без внутренних перегородок и маркеров)
   РЕЗУЛЬТАТ: Робот - в исходном положении, и все клетки по периметру внешней рамки промакированы

3. ДАНО: Робот - в произвольной клетке ограниченного прямоугольного поля
   РЕЗУЛЬТАТ: Робот - в исходном положении, и все клетки поля промакированы

4. ДАНО: Робот находится в произвольной клетке ограниченного прямоугольного поля без 
   внутренних перегородок и маркеров.
   РЕЗУЛЬТАТ: Робот — в исходном положении в центре косого креста из маркеров, расставленных вплоть до внешней рамки.

5. ДАНО: На ограниченном внешней прямоугольной рамкой поле имеется ровно одна внутренняя 
   перегородка в форме прямоугольника. Робот - в произвольной клетке поля между внешней и внутренней перегородками. 
   РЕЗУЛЬТАТ: Робот - в исходном положении и по всему периметру внутренней, как внутренней, так и внешней, перегородки поставлены маркеры.

#--------------------------------------------------------------------------
6. ДАНО: Робот - в произвольной клетке ограниченного прямоугольного поля, на котором  
   могут находиться также внутренние прямоугольные перегородки (все перегородки изолированы друг от друга, прямоугольники могут вырождаться в отрезки)
   РЕЗУЛЬТАТ: Робот - в исходном положении и -

      a) по всему периметру внешней рамки стоят маркеры;

      б) маркеры не во всех клетках периметра, а только в 4-х позициях - напротив исходного положения робота.

7. ДАНО: Робот - рядом с горизонтальной бесконечно продолжающейся 
   в обе стороны перегородкой (под ней), в которой имеется проход шириной в одну клетку.
   РЕЗУЛЬТАТ: Робот - в клетке под проходом

8. ДАНО: Где-то на неограниченном со всех сторон поле без внутренних перегородок 
   имеется единственный маркер. Робот - в произвольной клетке этого поля.
   РЕЗУЛЬТАТ: Робот - в клетке с маркером.

9. ДАНО: Робот - в произвольной клетке ограниченного прямоугольного поля (без внутренних 
   перегородок)
   РЕЗУЛЬТАТ: Робот - в исходном положении, на всем поле расставлены маркеры в шахматном порядке, причем так, чтобы в клетке с роботом находился маркер

10. ДАНО: Робот - в произвольной клетке ограниченного прямоугольного поля (без внутренних 
   перегородок)
   РЕЗУЛЬТАТ: Робот - в исходном положении, и на всем поле расставлены маркеры в шахматном порядке клетками размера N*N (N-параметр функции), начиная с юго-западного угла

---------------------------------------------------------------------------------
11. ДАНО: Робот - в произвольной клетке ограниченного прямоугольного поля, на поле расставлены горизонтальные перегородки различной длины (перегорки длиной в несколько клеток, считаются одной перегородкой), не касающиеся внешней рамки.
РЕЗУЛЬТАТ: Робот — в исходном положении, подсчитано и возвращено число всех перегородок на поле.

12. Отличается от предыдущей задачи тем, что если в перегородке имеются разрывы не более одной клетки каждый, то такая перегородка считается одной перегородкой.

--------------------------------------------------------------------------------------
**Прежде всего потребуется написать следующие обобщённые функции (которые потом надо будет использовать).**

1) `along!(stop_condition, robot, side)` 
      -- перемещает робота (какого-либо типа) в заданном направлении до выполнения условия останова

      -- здесь и далее предполагается, что условие останова задается функцией одного аргумента (типа `HorizonSide`) и возвращающая значение типа `Bool`

      Примеры задания условия останова (с помощью анонимных функций):

```julia
   _ -> ismarker(robot) 
   #=
   здесь формалный параметр заменен символом `_`, т.к. тут он фактически не используется (но формально требуется, чтобы  функция, выражающая условие останва, имела ровно один параметр, см. ранее использовавшиеся реализации функции `along!`)
   =#

   side -> isborder(robot, left(side))
```

2) `numsteps_along!(stop_condition, robot, side)` 
      -- делает то же самое, что и предыдущая функция, но дополнительно возвращает число фактически сделанных шагов

3) `along!(stop_condition, robot, side, max_num)` 
      -- перемещает робота (какого-либо типа) в заданном направлении до выполнения условия останова или до превышения заданного максимального числа шагов и возвращает число фактически сделанных шагов

4) `along!(robot, side)` 
      -- перемещает робота (какого-либо типа) до упора в заданном направлении

5) `numsteps_along!(robot, side)` 
      -- делает то же самое, что и предыдущая функция, но дополнительно возвращает число фактически сделанных шагов

6) `along!(robot, side, num_steps)` 
      -- перемещает робота (какого-либо типа) в заданном направлении на заданное число шагов

7) `snake!(stop_condition::Function, robot, (move_side, next_row_side)::NTuple{2,HorizonSide}=(Ost,Nord))`
      -- перемещает робота  (какого-либо типа) змейкой по ограниченному рамкой полю до выполнения заданного условия останова

8) `snake!(robot, (move_side, next_row_side)::NTuple{2,HorizonSide}=(Ost,Nord))`
      -- перемещает робота  (какого-либо типа) змейкой по ограниченному рамкой полю пока не будет пройдено ВСЁ поле

9) `spiral!(stop_condition::Function, robot)`
      -- перемещает робота (какого-либо типа) по раскручивающейся спирали до выполнения заданного условия останова

10) `shatl!(stop_condition::Function, robot)`
      -- перемещает робота

--------------------------------------------------------------------------------------------------------------
**Многие из этих функций нами ранее уже использовались, но теперь их стоит собрать все вместе в один отдельный файл, чтобы в последствии этот файл можно было бы просто "инклюдить" в свои новые программы.**
--------------------------------------------------------------------------------------------------------------


С использование этих обобщённых функций решить следующие задачи (нумерация задач продолжается).

13.  Решить задачу 9 с использованием обобщённой функции 
`snake!(robot, (move_side, next_row_side)::NTuple{2,HorizonSide} = (Ost,Nord))`

14.  Решить предыдущую задачу, но при условии наличия на поле **простых** внутренних переогородок.
    
   Под **простыми перегородками** мы понимаем **изолированные** прямолинейные или прямоугольные перегородки.

15. Решить задачу 7 с использованием обобщённой функции 
`shatl!(stop_condition::Function, robot)`

16. Решить задачу 8 с использованием обобщённой функции 
`spiral!(stop_condition::Function, robot)`

17. Решить предыдущую задачу, но при дополнительном условии:
            
    а) на поле имеются внутренние изолированные прямолинейные перегородки конечной длины (только прямолинейных,прямоугольных перегородок нет);

    б) некоторые из прямолинейных перегородок могут быть полубесконечными.

-------------------------------------------------------------------------------------------------

**Для решения задач 11-15 с использованием указанных обобщённых функций потребуется проектировать роботов специальных типов, в том числе и рассматривавшихся на лекции. В таком подходе к проектированию и состоит суть обобщённого программирования. Смысл обобщённого программирования состоит в том, чтобы в максимальной степени обеспечить возможность многократного переиспользования один раз написанного (обобщённого) кода, в том числе и в ситуациях, которые заранее не предусматривались.**
------------------------------------------------------------------------------------------------

18. Написать рекурсивную функцию, перемещающую робота до упора в заданном направлении.
    
19. Написать рекурсивную функцию, перемещающую робота до упора в заданном направлении, ставящую возле перегородки маркер и возвращающую робота в исходное положение.

20. Написать рекурсивную функцию, перемещающую робота в соседнюю клеьку в заданном направлении, при этом на пути робота может находиться изолированная прямолинейная перегородка конечной длины.

21. Написать рекурсивную функцию, перемещающую робота на расстояние вдвое большее исходного расстояния от перегородки, находящейся с заданного направления (предполагается, что размеры поля позволяют это сделать).

Дооаботать эту функию таким образом, чтобы она возвращала значение `true`, в случае, если размеры поля позволяют удвоить расстояние, или - значение `false`, в противном случае (в этом случае робот должен быть перемещен на максимально возможное расстояние).

Как при этом можно было бы сделать так, чтобы в случае невозможности переместить робота на удвоенное расстояние, в результате робот оставался бы в исходном положении?

22. Написать рекурсивную функцию, перемещающую робота в позицию, симметричную по отношению к перегородке, находящейся с заданного направления, т.е. требуется, чтобы в результате робот оказался на расстоянии от противоположной перегородки равном расстоянию до заданной перегородки.

23. Написать рекурсивную функцию, суммирующую все элементы заданного вектора (реализовать хвостовую рекурсию). 

24. Написать рекурсивную функцию, перемещающую робота на расстояние от перегородки с заданного направления вдвое меньшее исходного.

**Указание:** воспользоваться косвенной рекурсией.

25. Написать рекурсивную функцию, перемещающую робота в заданном направлении до упора и расставлящую маркеры в шахматном порядке, 
a) начиная с установки маркера;
б) начиная без установки маркера (в стартовой клетке).

**Указание:** воспользоваться косвенной рекурсией

26. Написать функцию, возвращающую значение n-го члена последовательности Фибоначчи
(1, 1, 2, 3, 5, 8, ...)

а) без использования рекурсии;
б) с использованием рекурсии;

практически убедиться, что наивная рекурсивная реализация такой функции будет крайне не эффективна в вычислительном отношении.

в) с использованием рекурсии и с мемоизацией;

убедиться, что полученный алгоритм будет достаточно эффективен в вычислительном отношении.

27. Написать функцию, расставляющие маркеры в каждой клетке внутри произвольного замкнутого лабиринта, ограниченного 

а) маркерами;

б) перегородками

и возвращающую робота в исходное положение.

Указание: воспользоваться рекурсией.

28. Написать функцию, расставляющие маркеры в шахматном порядка, начиная с исходной клетки внутри произвольного замкнутого лабиринта, ограниченного перегородками, и возвращающего робота в исходное положение.

**Указания:**
1) воспользоваться рекурсивным алгоритмом обхода поля;
2) спроектировать и использовать робота специального типа, ставящего маркеры в шахматном порядке

29. Написать функцию, расставляющие маркеры в форме косого креста с центром в исходном положении робота внутри произвольного замкнутого лабиринта, ограниченного перегородками, и возвращающего робота в исходное положение.

**Указания:**
1) воспользоваться рекурсивным алгоритмом обхода поля;
2) спроектирвать робота специального типа, ставящего маркеры только в "диагональных" клетках.

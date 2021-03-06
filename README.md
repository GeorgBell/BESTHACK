# BESTHACK
# Решение отборочного задания
## Задача внешней баллистики с учетом воздушных возмущений
### Выполнила команда GITT

### Пояснения к решению
Файл решения представлен в виде [**Hackathon.ipynb**](https://github.com/GeorgBell/BESTHACK/blob/master/Программа/Hackathon.ipynb) Jupyter Notebook-файла с пояснениями
#### 1. Алгоритм решения
![alt text](https://github.com/GeorgBell/BESTHACK/blob/master/Программа/Pics/Algorithm.png)

#### 2. Описание функционала алгоритма
Для работы алгоритма используется 5 функций: getFa, getW, getFlight, getAlpha, getDecision.

##### 2.1. getFa
Функция предназначена для определения аэродинамической силы, действующей на груз в зависимости от скорости движения. Входной переменной является скорость движения.  
Используется таблица 1, предварительно считанная с файла F.csv.  
Если значение скорости находится между его табличными значениями – используется линейная интерполяция по соседним значениям.  
Для значений скорости более 270 м/с значение аэродинамической силы принимается равным 19 931 Н.  

##### 2.2. getW
Функция предназначена для определения проекций скорости ветра на ось X и Z. Входной переменной является высота над уровнем земли.  
Используется таблица 2, предварительно считанная с файла Wind.csv.  
Для значений высоты над уровнем земли более 1400 м используется табличное значение для 1400 м.  

##### 2.3. getFlight
Функция предназначена для определения траектории движения и значений проекций скорости груза на координатные оси в зависимости от времени, прошедшего с момента сбрасывания груза.  
Принимает 7 входных параметров: начальная высота, начальная скорость, угол, шаг по времени, масса и координаты точи сброса.  
Возможность варьирования шага по времени используется для нахождения оптимального угла сбрасывания груза.  

##### 2.4. getAlpha
Функция предназначена для определения оптимального угла сбрасывания груза. В качестве критерия оптимальности принимается минимизация времени приземления груза.  
Принимает 7 входных параметров: начальная высота, начальная скорость, шаг по времени, масса, минимальное и максимальное значение угла, шаг изменения угла.  

##### 2.5. getDecision
Функция предназначена для определения всех данных, требующихся по заданию.

#### 3. Описание работы алгоритма
На первом этапе определяется оптимальный угол сброса. Для этого сначала вызывается функция getAlpha с диапазоном от -90 до 90 градусов и грубым шагом в 5 градусов и шагом по времени 0.1 с. Определяется диапазон, в котором находится оптимальный угол альфа. С этим диапазоном осуществляется повторный вызов функции getAlpha с уменьшенным шагом по углу до 1 градуса и шага по времени 0.01 с.  
Полученное значение угла передается в функцию getFlight. После его выполнения получается координата приземления груза по X и Z. Указанные координаты, взятые со знаком «-», определяют координаты точки сброса.  
Новые определенные координаты и угол альфа используются для финального запуска функции getFlight, расчета траектории груза и его проекции скоростей.  
Строятся графики. Проверяется точность попадания груза.  
Дополнительным критерии правильности является близость к 0 проекций скорости груза на оси X и Z поселение секунды падения груза. Указанное условие повышает точность попадания груза в заданную область.  

#### P.S.
Также была разработана большая часть математического апарата для сброса груза с парашюта и написаны основные блоки программы. К сожалению, на полную реализацию не хватило отведенного времени. Код представлен в файле [**Parachute.ipynb**](https://github.com/GeorgBell/BESTHACK/blob/master/Addon_parachute/Parachute.ipynb) 

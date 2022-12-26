# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Курбатова Анна Евгеньевна
- РИ210950
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Ознакомиться с основными операторами языка Python на примере реализации линейной регрессии.

## Задание 1
### Реализация перцептрона в проекте Unity.
1. Создание сцены в Unity и добавление кода перцептрона
![image](https://user-images.githubusercontent.com/86403364/209575771-e3368997-3f15-4c8b-8902-8117aacaf126.png)

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));		
	}
	
	void Update () {
		
	}
}
```

2. Значения для логического оператора OR(||)
 
![image](https://user-images.githubusercontent.com/86403364/209576491-743b0e10-67ef-42a4-86ce-ec4f0605d514.png)

3. Результаты при:

Train(1).Пока что программа не научилась работать как элемент OR

![image](https://user-images.githubusercontent.com/86403364/209577076-00d9719e-7903-42b5-a96e-f6d02504bcbc.png)

Train(2). Программа всё еще делает ошибки, но уже ближе к нужному результату

![image](https://user-images.githubusercontent.com/86403364/209577426-789cbe98-6947-4a32-9b76-7c4a336d2630.png)

Train(4).Начиная с 3й итерации, программа стала давать верный результат

![image](https://user-images.githubusercontent.com/86403364/209578390-a9ec3771-a327-43fd-8c3e-84cfc50ce1e0.png)





## Задание 2
### Изучение кода на Python
Произведена подготовка данных для работы с алгоритмом линейной регрессии.

```py
import numpy as np
import matplotlib.pyplot as plt

x = [3, 21, 22, 34, 54, 34, 55, 67, 89, 99]
x = np.array(x)
y = [2, 22, 24, 65, 79, 82, 55, 130, 150, 199]
y = np.array(y)
plt.scatter(x, y)
```

Определены связанные функции модели, потерь и оптимизации.

```py
def model(a, b, x):
    return a * x + b


def loss_function(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    return (0.5/num) * (np.square(prediction - y)).sum()


def optimize(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    da = (1.0/num) * ((prediction - y)*x).sum()
    db = (1.0/num) * ((prediction - y).sum())
    a = a - Lr * da
    b = b - Lr * db
    return a, b


def iterate(a, b, x, y, times):
    for i in range(times):
        a, b = optimize(a, b, x, y)
        return a, b
```

Выполнение шага №1.Инициализация и модель итеративной оптимизации.

```py
a = np.random.rand(1)
print(a)
b = np.random.rand(1)
print(b)
Lr = 0.000001

a, b = iterate(a, b, x, y, 1)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)
```
![image](https://user-images.githubusercontent.com/86403364/192506100-61d9f8e9-36a9-417f-8c3b-51b2ba9424a6.png)

Выполнение шага №2. Вторая итерация. Отображение значений параметров, значений потерь и эффектов визуализации после итерации.

```py
a, b = iterate(a, b, x, y, 2)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)
```

Выполнение шага №3. Третья итерация.

```py
a, b = iterate(a, b, x, y, 3)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)
```
Выполнение шага №4. Четвёртая итерация.

```py
a, b = iterate(a, b, x, y, 4)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)
```

Выполнение шага №5. Пятая итерация.

```py
a, b = iterate(a, b, x, y, 5)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)
```

Выполнение шага №6. 10 000-ая итерация.

```py
a, b = iterate(a, b, x, y, 10000)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)
```

![image](https://user-images.githubusercontent.com/86403364/192510580-59814ad1-e4c8-4371-be55-122c9b6b0903.png)


## Задание 3
### Ответы на вопросы после изучения кода на Python

- Должна ли величина loss стремиться к нулю при изменении исходных данных?

С каждым увеличением количества итераций, переменная loss уменьшалась. (третий столбец)

![image](https://user-images.githubusercontent.com/86403364/192516618-5198338c-2aa6-42c5-b7bc-475a8eb79dd2.png)

- Какова роль параметра Lr?

Оптимизация данных к наименьшей погрешности. При изменении переменной можно заметить, как при увеличении данные больше расходятся, а при уменьшении - сходятся

При 0.001

![image](https://user-images.githubusercontent.com/86403364/192519181-bc9c3937-6223-4e96-bf90-31c31a769884.png)


При 0.0001

![image](https://user-images.githubusercontent.com/86403364/192519264-5526906d-e865-416c-b2c7-3515aeb49e8f.png)

При 0.000000001

![image](https://user-images.githubusercontent.com/86403364/192519484-07c4f905-a7d0-405d-a614-3c550f5f1928.png)



## Выводы

Мне удалось примерно ознакомиться с некоторыми основными операторами языка Python на примере реализации линейной регрессии.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

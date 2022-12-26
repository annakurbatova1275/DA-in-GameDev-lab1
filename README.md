# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Курбатова Анна Евгеньевна
- РИ210950
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |

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
Ознакомиться с перцептроном.

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

3. При выполнении логического оператора AND(&) требуется больше итераций чем для оператора OR. Программа начала давать верный результат на 6й итерации.

![image](https://user-images.githubusercontent.com/86403364/209585345-764cdb00-2935-4581-ad5a-8c3d61b1bb28.png)

![image](https://user-images.githubusercontent.com/86403364/209585357-da72d669-26b3-4898-ac38-bf90a0e2d8fe.png)

4.Выполнение логического оператора XOR(^) удается только с ошибками. Первые 2 итерации делают пару-тройку ошибок, а потом ошибки идут всегда.

![image](https://user-images.githubusercontent.com/86403364/209585739-f2327883-ac97-4aee-b6be-92e11e7d9dbc.png)

![image](https://user-images.githubusercontent.com/86403364/209585765-ec1cf4c5-424a-4932-9f60-6fb225c27972.png)

5.Выполнение логическоро оператора NAND прошло также, как у оператора AND. Начиная с 6й(из 8) итераций программа давала верный результат.

![image](https://user-images.githubusercontent.com/86403364/209585897-3311c318-5dbf-444b-b745-bc26ab88d3fb.png)

![image](https://user-images.githubusercontent.com/86403364/209585909-c99699e4-e6e4-46dc-88b3-75ce1ba7fb3e.png)

## Задание 2
### Построить графики зависимости количества эпох и ошибок.


## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity.


## Выводы

Я ознакомиилась с основной работой перцептрона.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

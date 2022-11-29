# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
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
Познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity

## Задание 1
### Реализовать систему машинного обучения 
#### Работа с Anaconda
1. Создание изолированной виртуальной среды с помощью команды:

![image](https://user-images.githubusercontent.com/86403364/204545008-44194772-9ffd-4156-a0e0-81332191d410.png)

2. Активация среды с помощью команды:

![image](https://user-images.githubusercontent.com/86403364/204545610-51bd2f87-b139-4664-b6ca-cde9acfdc406.png)

3. Установка пакета mlagents:

![image](https://user-images.githubusercontent.com/86403364/204546375-5f09dc8e-934e-47a5-97a6-7e3c97e0bd62.png)

4. Установка torch:

![image](https://user-images.githubusercontent.com/86403364/204549049-fe3af7b8-0848-4078-af15-24ca7b5210f2.png)

5.Создание 3D проекта в Unity:

![image](https://user-images.githubusercontent.com/86403364/204554346-30564ba1-fda2-4a82-8eca-1f553a56da02.png)

####Unity
1. Добавление пакетов ML Agents в Unity:

![image](https://user-images.githubusercontent.com/86403364/204557577-6a07371a-240d-45fe-9cb4-87a948daaacb.png)

2. Создание сценки в Unity

![image](https://user-images.githubusercontent.com/86403364/204561593-f001e1ca-f5d7-4f8e-892b-a902550287c7.png)

3. Скрипт на C# для работы с ML Agents и перемещения фигур

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8 - 4, 0.5f, Random.value * 8 - 4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if (distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```
4. Перемещение внутрь проекта для дальнейшей работы с ним через anaconda

![image](https://user-images.githubusercontent.com/86403364/204565976-c4337795-9731-46c4-a3a0-995ba3639080.png)

5. Запуск обучения

![image](https://user-images.githubusercontent.com/86403364/204566477-b561d589-1b2e-403b-a1d0-fea7ec8df1ce.png)

По итогу фигуры начали двигаться, а конкретно шарики начали кататься в сторону кубов и двигать их

![image](https://user-images.githubusercontent.com/86403364/204591561-9931fa5c-1428-4919-9696-1dbcb3270a4a.png)


## Задание 2
### Описать не менее трех сущностей в играх и интерактивных приложениях, нуждающихся в оптимизации. 
После слияния кода этой лабораторной работы и лабораторной работы №1 получились такие данные:

![image](https://user-images.githubusercontent.com/86403364/195134158-41fa54b5-7147-4e83-bd7e-0d7b148c1cc2.png)

![image](https://user-images.githubusercontent.com/86403364/195134314-8db52b85-65b3-4f15-8b9d-5e2894b5b5bd.png)

По итогу удалось воспроизводить только плохую аудиозапись.

![image](https://user-images.githubusercontent.com/86403364/195135658-6afbe50e-bfe1-4287-8065-6403a5d0f404.png)





## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2.



## Выводы

Я познакомилась с программными средствами для организции передачи данных между инструментами google, Python и Unity

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

# Перцептрон
# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Намесников Глеб Артёмович
- РИ210912
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
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
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Ознакомиться с работой перцептрона.

## Задание 1
### Логические операции перцептрона
Ход работы:
- В своем проекте юнити реализовать перцептрон, который умеет выполнять логические операции OR, AND, XOR, NAND.

В ходе выполнения был добавлен скрипт Perceptron.cs

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
		Train(4);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));	
	}
	
	void Update () {
		
	}
}

```
Далее была запущена работа перцептрона.

Обучение перцептрона на логическую операцию OR выполнено успешно. ![image](https://user-images.githubusercontent.com/103383207/204812128-3a9a5d30-565a-4443-8ec0-9d1a960badd4.png)
Обучение перцептрона на логическую операцию AND выполнено успешно. ![image](https://user-images.githubusercontent.com/103383207/204812188-17ab5349-e2fb-4dea-bd90-8942023f3186.png)
Обучение перцептрона на логическую операцию NAND выполнено успешно, однако в данном случае на тренировку понадобилось 7 эпох, а не 4. ![image](https://user-images.githubusercontent.com/103383207/204812238-21f1d3ed-e96e-4491-837a-9ca05478ed67.png)
Обучение перцептрона на логическую операцию XOR неуспешно, поскольку, как было обьяснено в лекции, данная логическая операция из-за линейной модели перцептрона не способна разделить плоскость одной линией. ![image](https://user-images.githubusercontent.com/103383207/204812291-661ad37e-ea80-414e-a623-9a70695c7c2d.png)

## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения.

Ниже привидены графики с нужной зависимостью.

![or](https://user-images.githubusercontent.com/103383207/204812594-f6ded512-dd96-4135-9c12-e0115eac2ffc.png)
![and](https://user-images.githubusercontent.com/103383207/204812606-85bae47d-f1a5-457d-9d84-6a7273335371.png)
![nand](https://user-images.githubusercontent.com/103383207/204812616-3f3e00c4-a96f-452f-84a5-86c1d9f97c34.png)
![xor](https://user-images.githubusercontent.com/103383207/204812629-09ae9d5b-192d-43de-b7ad-194ff006bbed.png)

## Выводы

В ходе выполнения данной лабороторной работы я понял что такое перцептрон и на практике разобрался как он взаимодействует и работает с логическими операциями.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Заморзов Артём Викторовчи
- РИ-230918
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | * |
| Задание 2 | * | * |
| Задание 3 | * | * |

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
Познакомиться с одной из первых моделей нейросетей - перцептроном и реализовать эту модель в Unity

## Задание 1. В проекте Unity реализовать перцептрон, который умеет производить вычисления
Ход работы:
- Возьмем скрипт, в котором описана логика обучения и вычисления перцептрона
```C#  
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
- Для удобства можно указать кол-во эпох через инспектор)
```C#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour 
{
	public event Action EpochIsCompleted;
	[SerializeField] int epochs;
	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	public double totalError = 0;

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
			weights[i] = UnityEngine.Random.Range(-1.0f,1.0f);
		}
		bias = UnityEngine.Random.Range(-1.0f,1.0f);
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

	public double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train()
	{
		totalError = 0;
		for(int t = 0; t < ts.Length; t++)
		{
			UpdateWeights(t);
			Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
		}
		Debug.Log("TOTAL ERROR: " + totalError);
	}

	IEnumerator TrainCoroutine(int epochs) 
	{
		for (int i = 0; i < epochs; i++)
		{
            yield return new WaitForSeconds(1.0f);
            Train();
            EpochIsCompleted?.Invoke();
        }
		PrintResult();
	}
	void PrintResult()
	{
        Debug.Log("Test 0 0: " + CalcOutput(0, 0));
        Debug.Log("Test 0 1: " + CalcOutput(0, 1));
        Debug.Log("Test 1 0: " + CalcOutput(1, 0));
        Debug.Log("Test 1 1: " + CalcOutput(1, 1));
    }

	void Start () {
        InitialiseWeights();
		StartCoroutine(TrainCoroutine(epochs));
	}
}
```
- Запускаем код и видим результат (количество эпох: 5)

Проект в незапущенном виде
![image](https://github.com/shadesixsix/wss/blob/main/W4ZZ1.jpg)
Результат
![image](https://github.com/shadesixsix/wss/blob/main/W4ZZ2.jpg)

- За 5 эпох перцептрон смог обучиться вычислять выражение OR, он уже выводит правильный результат


## Задание 2. Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения
Ход работы:
- Поскольку веса и пороговое значения задаются рандомным образом, перцептрон можно запустить всего один раз, это уже передаст необходимую для анализа картину\.
- Заносим данные в таблицу и строим графики
![image](https://github.com/shadesixsix/wss/blob/main/W4z6.jpg)

- На графике видно, что чем больше количество эпох, тем точнее будет результат вычисления перцептрона, но также вероятность того что при малом кол-ве эпох обучение будет пройдено успешно не равно нулю.
## Задание 3. Построить визуальную модель работы перцептрона на сцене Unity
Ход работы:
- Идея визуализации: при каждой пройденной эпохе на платформу будет падать кубик, который цветом будет отражать обученность модели. Если кубик красный модель не обучена, и если зеленый то обученна
- Для реализации этой идеи я написал скрипт, который получает результат пройденной эпохи и на основе этих данных создает куб
```C#
using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;

public class CubeSpawner : MonoBehaviour
{
    [SerializeField] private Perceptron perceptron;
    [SerializeField] private GameObject _prefab;
    [SerializeField] private GameObject _platform;
    [SerializeField] private TextMeshProUGUI _text;
    private Vector3 lastPos;
    private int epochs = 0;

    private void OnEnable()
    {
        perceptron.EpochIsCompleted += CreateCube;
    }
    private void Start()
    {
        Vector3 platformPos = _platform.transform.position;
        var x = platformPos.x - (_platform.transform.localScale.x)/2.0f + 2;
        lastPos = new Vector3(x, platformPos.y + 10, platformPos.z);
    }
    private void CreateCube()
    {
        var obj = Instantiate(_prefab);
        obj.transform.position = CalculateThePosition(lastPos);
        lastPos = obj.transform.position;
        var color = obj.GetComponent<MeshRenderer>();
        if (perceptron.totalError > 0) 
            color.material.color = Color.red;
        else 
            color.material.color = Color.green;
        epochs++;
        _text.text = epochs.ToString();
    }

    private Vector3 CalculateThePosition(Vector3 lastPosition)
    {
        var newPos = new Vector3(lastPosition.x + _prefab.transform.localScale.x + 1, lastPosition.y, lastPosition.z);
        return newPos;
    }

}
```
- Я изменил скрипт перцептрон для взаимодействия с скриптом выше
- Результат:

Сцена с 4 эпохами:
![image](https://github.com/shadesixsix/wss/blob/main/W4z1.jpg)

1 эпоха:
![image](https://github.com/shadesixsix/wss/blob/main/W4z2.jpg)

2 эпоха:
![image](https://github.com/shadesixsix/wss/blob/main/W4z3.jpg)

3 эпоха:
![image](https://github.com/shadesixsix/wss/blob/main/W4z4.jpg)

4 эпоха:
![image](https://github.com/shadesixsix/wss/blob/main/W4z5.jpg)


- Визуализация успешно показывает состояние перцептрона на каждой стадии обучения
  
## Выводы
Я познакомился с первой моделью нейросети - перцептроном, построил модель и визуализировал работу с помощью Unity
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

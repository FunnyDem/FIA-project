#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <locale.h>

int choice_tool(int numtool, int* tools, int size)
{
	int index;
	for (int i = 0; i < size; i++)
	{
		if (tools[i] == numtool)
		{
			printf("В наличии иструмент номер %d: на месте %d\n", numtool, i);
		}
	}
	printf("Введите место нужного инструмента:\n");
	scanf_s("%d", &index);
	printf("Выбран иструмент под номером %d находящийся на месте %d", numtool, index);
	return index;
}

int application(int numtool, int* tools, int size, int use, int alltime, clock_t &startuse, clock_t &stopuse)
{
	if (numtool >= 0 && numtool < size)
	{
		if (use == 1)
		{
			startuse = clock();
		}
		if (use == 0)
		{
			stopuse = clock();
			alltime = (stopuse - startuse) / 1000;
		}
		return alltime;
	}
}

void conclusion(int alltime) 
{
	int sec, min, hour;
	sec = alltime % 60;
	min = alltime / 60;
	hour = alltime / 3600;
	printf("Время использования: %d:%d:%d", hour, min, sec);
}

int choice_tool_shortest_usage(int* timeU, int size, int use, int time, char* name, int* tools, int numtool)
{
	int index = -1;
	int min_time = INT_MAX;
	for (int i = 0; i < size; i++)
	{
		if (min_time > time)
		{
			index = i;
			min_time = timeU[i];
			printf("%d", timeU[i]);
		}
	}
	printf("%s ", name);
	printf("%d", index);
	return index;
}

int get_tool_usage_time(int* timeU, int* tools, int numtool, int index, int size, int time) 
{
	int time_use = 0;
	for (int i = 0; i < size; i++)
	{
		timeU[i] = time;
	}
	printf("Введите место нужного инструмента:\n");
	scanf_s("%d", &index);
	for (int i = 0; i < size; i++)
	{
		if (tools[i] = numtool)
			time_use = timeU[index];
	}
	return time_use;
}

int main() 
{

	setlocale(LC_ALL, "Rus");

	clock_t startuse, stopuse;
	int size;
	int count = 0;
	int choicenumtool;
	int setuse = 0;
	int alltime = 0;
	int choice;
	bool continueEnable = true;
	int continueChoice;
	int index = 0;
	int usageTime = 0;
	int time;

	printf("Введите ёмкость магазина инструментов:\n");
	scanf_s("%d", &size);

	int* tools = new int[size];
	int* timeUse = (int*)malloc(size * sizeof(int));
	char nameTools[40] = {"Название инструмента"};

	/* заполнение магазина номерами инструментов */
	for (int i = 1; i < size + 1; i++)
	{
		tools[i] = (int)(rand() % 1000);
	}

	while (continueEnable == true)
	{
		printf("\nСделайте выбор:\n 1 - Выбор инструмента \n 2 - Использовать инструмент \n 3 - Выбор инструмента с наименшим временем \n 4 - Получение времени использования инструмента \n");
		scanf_s("%d", &choice);

		switch (choice)
		{
			//Функция выбора инструмента
		case 1:
			printf("Введите номер инструмента:\n");
			scanf_s("%d", &choicenumtool);
			if (choicenumtool == 0)
			{
				printf("Место пустое\n");
				return main();
			}
			index = choice_tool(choicenumtool, tools, size);
			break;
			//Функция применения
		case 2:
			printf("Хотите ли использовать этот инструмент? ( 1 - да, 0 - нет )\n");
			scanf_s("%d", &setuse);
			application(choicenumtool, tools, size, setuse, alltime, startuse, stopuse);
			printf("Закончить (0 - да)\n");
			scanf_s("%d", &setuse);
			time = application(choicenumtool, tools, size, setuse, alltime, startuse, stopuse);
			conclusion(time);
			break;
			//Функция выбора инструмента с наименьшим временем использования
		case 3:
			printf("Выбран инструмент с минимальным временем использования:");
			choice_tool_shortest_usage( timeUse, size, setuse, time, nameTools, tools, choicenumtool);
			break;
			//Функция получения времени использования инструмента
		case 4:
			usageTime = get_tool_usage_time(timeUse, tools, choicenumtool, index, size, time);
			printf("%d", usageTime);
			break;

		}

		printf("\nХотите продолжить?(1 - да, 0 - нет)\n");
		scanf_s("%d", &continueChoice);
		if (continueChoice == 1)
		{
			continueChoice = true;
		}
		else if (continueChoice == 0)
		{
			continueEnable == false;
		}
	}
	return 0;
}

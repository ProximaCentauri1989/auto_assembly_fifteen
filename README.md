# auto_asembly_fifteen
Auto assembly of the game "Fifteen"

Автоматическая сборка игры “Пятнашки” основана на рекурсивном алгоритме обхода дерева состояний игрового поля (алгоритм Iterative Depending A*). 

Входные данные:   
  открытый список: при первом вызове функции поиска список содержит корневой узел. В дальнейшем список увеличивается по мере углубления в дерево состояний. 

  вес игрового поля: при первом вызове равен 0. Для каждого последующего развернутого узла стоимость на единицу больше, т.к. для его достижения требуется на один ход больше от предыдущего.

  порог: равен эвристической оценке корневого узла. Отсекает ранее пройденные вершины (предотвращая зацикливание алгоритма) и предотвращает развертывание узлов, стоимость которых превышает значение порога (“плохие состояния”).  

Работа алгоритма:
1.	Извлечь из открытого списка последний добавленный узел (при первом вызове это корневой узел). 
2.	Получить его стоимость. 
3.	Если стоимость узла превышает значения порога вернуть стоимость этого узла, зафиксировать её (может быть использована для нового порога, в случае, если при текущем значении порога поиск потерпит неудачу), исключить из открытого списка последний добавленный узел. 
4.	Иначе, проверить является ли игровое поле искомым. Если игровое поле является искомым (найдено частное решение) выйти из рекурсии (вернуть FOUND). При возвратах добавлять в список решений (solutionList) узлы, последовательность развертывания которых привела к частному решению.
5.	Если игровое поле не является искомым развернуть это поле (получить список дочерних состояний игрового поля из текущего) и перейти в пункту 1 (рекурсивно вызвать функцию поиска с измененными параметрами).
6.	В случае если алгоритм не нашел частного решения (искомое состояние глубоко в дереве и при текущем значении порога оно недостижимо) выполняется возврат зафиксированной минимальной стоимости узла, которая превышает порог и поиск повторяется снова с увеличенным значением порога.

Выходные данные:
Возврат FOUND из главной функции алгоритма IDA*: частное решение найдено. В результате имеется список состояний, которые привели к частному решению.
Возврат NOT_FOUND из главной функции алгоритма IDA*: частное решение НЕ найдено. 

P.S1. Т.к. при формировании игрового поля введена оценка игрового поля на собираемость (отсечение исходного состояния, которое не приведет к успешной сборке), необходимость возврата NOT_FOUND из функции Search опущена.

P.S2. Автосборка запускается 7 раз (1 раз запускает пользователь и 6 раз сборка запускается в цикле с измененными  состояниями исходного и целевого полей). Смысл таких манипуляций с функцией сборки лежит в разбиении всего этапа сборки на 7 относительно небольших частей. Это необходимо для того, чтобы при слишком запутанных состояних исключить время на ожидание поиска целевого состояния, которое может находиться глубоко в дереве. Например, за первое целевое состояние берется любое состояние поля, в котором фишка со значением "1" расположена на своем месте. Затем, сборка запускается с этим состоянием в качестве исходного, а целевым является любое состояние где фишки "1" и "2" на своих местах и т.д., пока не будет достигнуто терминальное состояние, в котором все фишки на своих местах.
Достоинство: исключены ожидания (сборка происходит "на лету").
Недостаток: сборка от исходного сгенерированного состояния до терминального, где все 15 фишек на своих местах (16-я пустая), осуществляется не за минимальное количество ходов. В среднем требуется на 20-30 ходов больше.

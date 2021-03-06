# Исследования эффекта погоды на авторегрессионную модель прогнозирования спроса 

## Задача

В данном ноутбуке решается задача прогнозирования трехнедельного спроса на __сезонные товары__ конкретных поставщиков.

В предыдущих фазах исследования эмпирическим путем было утсановлено:

- Удобно использовать пакет Darts
- Ridge-регрессия  дает оптимальные по соотношению качество-скорость результаты
- При этом регрессию лучше применять не к отдельным товарам, а к представителям некоторых кластеров
- В качестве кластеров естественно были взяты подрубрики товаров (rubr_lev2)
- В качестве признаков регресии лучше всего использовать исключительно прошлые лаги самого представителя. Никакие сезонные ковариаты оказались не нужны
- Самих представителей определяем беря сумму по всем товарам кластера и деля ее на логарифм кол-ва товаров на каждую дату
- Выгодно отбросить нерегулярности представителя, применив алгоритм STL из пакета statsmodels.
- Также выгодно "сдвинуть прогноз влево". Это означает, что модель линейной регрессии обучается на одних лагах, а вот прогноз строится с теми же лагами, сдвинутыми на 2 недели вперед. Это необходимо, чтобы опережающим образом создавать запас товара при растущем тренде, а также держать малое кол-во товара при нисходящем.
- Получив прогноз представителя, рассчитываем градиент, т.е. отношение 3-недели вперед/три недели назад, и применяем этот градиент к каждому товарному ряду, чтобы получить прогноз.

В данном ноутбуке дополнительно тестируется эффект погоды, а именно
- средней температуры за период
- суммарного кол-ва осадков за период

Нужно заметить, что поскольку мы делаем "левый сдвиг" прогноза, формальные метрики, например mae или mape, получаются хуже, чем без сдвига. Это можно объяснить тем, что мы перезапасаем товар на растущем тренде, и недозапасаем при нисходящем. В связи с этим, разумно оценивать наши результаты не только по метрикам, но и зрительно анализируя графики прогноза и реального спроса.

## Основные используемые библиотеки

*pandas*,*darts*, *matplotlib*, *numpy*, *scipy.stats*, *sklearn*, *statsmodels*|

## Вывод

Добавление погодных данных, а именно осадков и температуры (Москва, ВДНХ) если и дало мизерное улучшение, вряд ли можно его считать статистически значимым.

Возможно стоит применять более локализованные погодные данные.






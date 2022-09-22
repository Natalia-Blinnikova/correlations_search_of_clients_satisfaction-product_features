correlations_search_of_clients_satisfaction-product_features
# Analysis of correlations between the customers' estimation of the mobile network quality and technical characteristics of it.

For the Russian language verstion scroll down.

### Product aim of the study: 
to investigate whether there are any limitations in the mobile network characteristics which reduce the satisfaction level of its users. 

### Tasks: 
* to explore if the characteristics of the network are different for satisfied and non-satisfied customers;
* to explore, if there is any difference in the characteristics of the network of the customers, who are not satisfied with the quality of the service and mention some precise limitations of it. 

### To accomplish the research, I have performed:
1. exploratory data analysis;
2. comparisons of groups according to:
<br> their general estimation of the network,
<br> their mentioning the limitations of the network;
3. exploration of the features’ distribution in the groups of customers;
4. statistical analysis of differences in those features among different groups of customers according to their estimation of the network, including:
Kruskal-Wallis test,
5. bootstrap.

### Results show 
there is no statistical difference in the network features between different groups of customers according their network estimation, besides just two features and even those are statistically different only if we compare groups according to their general estimation of the network. 

# Поиск взаимосвязи между характеристиками мобильной связи Megafon и ее оценкой от пользователей. 

### Цель исследования (продуктовая): 
выяснить, существуют ли какие-либо явные недостатки в характеристиках мобильной связи, которые очевидно ухудшают впечатления и удовлетворенность клиентов от этой услуги. 

### Задачи: 
<br> выяснить, существуют ли явные различия в характеристиках связи у довольных и недовольных клиентов; 
<br> выяснить, существуют ли явные различия в уточненных характеристиках связи у клиентов, кто доволен связью не на 100%. 

### План анализа. 
` Ключевая переменная - это удовлетворенность связью, а она подразделяется, в свою очередь, на удовлетворенность отдельными элементами услуг связи.
` cначала сделаю анализ по субъективной оценке, тем более, что достаточно много людей кроме субъективной оценки больше никак не уточнили свою оценку. А потом уже сделаю более детальный анализ того, есть ли различия в пользователях, кто ответил, чем конкретно их не устраивает связь (второй вопрос). 

<br> Смотрю сначала распределение общей оценки за связь. 
![dist_estimates](https://user-images.githubusercontent.com/84775580/171584548-e05008e7-821b-4653-96d3-bb96478bfa6e.png)

Тут интересно, что больше 50% поставили оценку выше 7. Что очень неплохо. 

<b> Я разобью участников исследования на три группы: </b>
<br>те, кто поставил оценки 1-4(ужасная связь)
<br>кто поставил оценки 5-8 (средняя связь)
<br>тех, кто поставил 9-10 (отличная связь)

И посмотрю на распределения значений показателей параметров связи для разных групп.
И если будут замечены сильные различия в распределениях, уже можно проверять статистическую значимость этих различий. 

### Далее посмотрела распределение каждой метрики связи между тремя группами. 
Чисто визуально:
* У метрики 'Total Traffic(MB)' каких-то явных трендов по разным группам не вижу, все группы имеют неравномерное распределение, скошенное право.
* У метрики 'Downlink Throughput(Kbps)' есть явные различия между тремя группами - надо проверять стат.значимость.
* У метрики 'Uplink Throughput(Kbps)'тоже есть различия.
* У 'Downlink TCP Retransmission Rate(%)' есть различия между 3 группой и 1 и 2. Можно покрутить, посмотреть, значимы ли они.
* У 'Video Streaming Download Throughput(Kbps)' есть различия.
* У 'Video Streaming xKB Start Delay(ms)' есть различия в группах.
* У 'Web Page Download Throughput(Kbps)' есть различия между 3 группой и 1 и 2.
* У 'Web Average TCP RTT(ms)' есть различия между 1 группой и 2 и 3.

<b> Ставлю нулевую статистическую гипотезу </b>, что различий между тремя группами по метрикам связи не существует. 
Как ее проверить? 

Проверяю распределения для каждой метрики. Они все ненормальные, скошенные влево, согласно коэффициентам ассиметрии.
![Коэф ассиметрии](https://user-images.githubusercontent.com/84775580/171584942-2b7f97aa-8368-42bb-936d-4cc6a6b3d255.jpg)

Возьму <b> тест Краскела-Уоллиса  </b>, для более, чем двух выборок и при ненормальном распределении независимой переменной. 
![краскел_3группы](https://user-images.githubusercontent.com/84775580/171585315-e0d15230-d0a5-4407-8a9b-693533a34db3.jpg)


Результаты будто говорят, что только у двух метрик есть стат.значимые различия: 
* Web Average TCP RTT(ms) (р<0.01), 
* Downlink TCP Retransmission Rate(%)(p<0.002)
* т.е. у третьей группы (кто поставил ниже 4 баллов за качество связи), более высокие показатели по Downlink TCP Retransmission Rate(%)) и Web Average TCP RTT(ms). 

### Теперь, возможно, стоит посмотреть, зависят ли более детальные оценки пользователей от различных элементов связи? 

Т.е. это ответ на второй вопрос, который давался пользователям, только если они поставили общую оценку за связь ниже, чем 9. Они должны были выбрать из 7 вариантов, почему они не могут поставить высшую оценку за связь. 

<br> Были такие варианты: 
1 - недозвоны, обрывы связи 
2 - время ожидания гудков
3 - плохое качество связи в зданиях
4 - медленный моб.интернет
5 - медленная загрузка видео
6 - затрудняюсь ответить
7 - свой вариант


Далее решила посмотреть, есть ли какие-то яркие зависимости между тем, какую общую оценку ставят юзеры, и тем, какие элементы связи они выбирают как не очень классные.

![count_disadvantages](https://user-images.githubusercontent.com/84775580/171586370-4c004dda-894e-40cf-92ce-8240fa5f93b2.png)

Чисто визуально, как будто бы у каждой субъективной оценки связи (вопрос Q2) примерно (пропорционально) одинаковое количество юзеров, которые выбирали те или иные негативные оценки связи (вопрос Q2). 

Поэтому решила оставить прошлую группировку на 3 группы (оценки 0-4, 5-8, 9-10), но оставить только первые две (ведь я пытаюсь понять, если ли зависимость в том, какие негативные качества связи отмечают юзеры, и какие у них метрики (характеристики) связи.

### Идея провести бутстрепы по 5 группам 

<b> Цель: </b> понять, а есть ли различия в объемах и характеристиках связи между теми, кто оценивал какую-то одну метрику связи как негативную, и теми, кто не выбрал эту метрику связи как негативную (вопрос Q2). Иначе, есть ли зависимости между тем, каковы показатели этих объемов и характеристик, и тем, какие метрики связи пользователи выбирают как негативные. 

1. У нас есть оценки метрик связы 1-5 (что не нравится конкретно в связи, вопрос Q2)
2. Также у нас есть 8 метрик связи (объемы и характеристики связи). 
3. Я провожу 5х8 бутсрепов. Почему так? Смотрим. 
4. Например, я выбираю две выборки: тех, кто выбрал 1 как негативную хар-ку связи, и тех, кто не выбрал. 
5. И теперь я смотрю, есть ли статистически значимая разница между этими двумя группами в их объемах и характеристиках связи (метриках), которых 8. Т.е. для каждой из 8 метрик провожу свой бутстреп. 
6. Бутстреп проводится только с теми пользователями, кто ставил 1-8 в качестве общей оценки связи.

<b> Почему бутстреп? </b> 

Потому что распределения по каждой метрике - ненормальные. Бутстреп поможет симулировать нормальное распределение средних по множествам симулированных выборок. 

<b>Нулевая гипотеза для бутстрепов: </b>  НЕТ различий между пользователями, которые выбирают и не выбирают недостатки связи, по каждой метрике (характеристике) этой связи.

Теперь делаю пустой датафрейм, в котором индекс - это метрики связи, а потом в него буду добавлять столбцы, где каждый столбец - это негативное свойство связи, которое пользователь выбрал в вопросе Q2, значения будут вероятности, что метрики связи у тех, кто выбрал или не выбрал это негативное свойство связи, НЕ отличаются. Т.е. что две выборки принадлежат одной ГС.

![бутстреп](https://user-images.githubusercontent.com/84775580/171585981-fd32ad33-5cd6-4615-925d-c1b20254ad09.jpg)

### Выводы из бутстрепа

<br> Я не вижу вообще никакой статистической разницы в метриках связи между пользователями, которые выбирали какой-то недостаток в связи, и теми, кто не выбирал (везде вероятность, что такая и более выраженная разница, как в данном выборке, может быть в генеральной совокупности, больше, чем 0.4, это очень много). 

<br> Следовательно, принимаем нулевую гипотезу, что НЕТ различий между пользователями, которые выбирают и не выбирают недостатки связи, по метрикам (характеристикам) этой связи.

## ОБЩИЕ ВЫВОДЫ 

<br> Если мы делим пользователей на три группы по общей оценке связи (высокая, средняя и низкая), то у третьей группы (низкая оценка связи), более высокие показатели по 

* Downlink TCP Retransmission Rate(%)) и 
* Web Average TCP RTT(ms).

<br> В целом, половина пользователей оценивает связь на 7 баллов, а 75% - вообще на 10. 

<br> Пользователи, которые поставили ниже 9 и в дальнейшем более прицельно писали, какие элементы связи их не устраивают, по своим характеристикам связи статистически НЕ отличаются от тех, кто не указывал те или иные элементы связи в качестве негативных. 

<br> Т.е. нельзя сказать, что тот пользователь, которому не нравится показатель связи "недозвоны, обрывы связи" отличается по своим параметрам использования сети от пользователя, которого устраивает такой показатель. 

<br> <b> ? </b> возможно, дело в том, что пользователи в целом очень субъективно оценивают связь, т.е. для кого-то один обрыв связи критичен, а для другого - 10 обрывов не критичны. 
<br> <b> ? </b> возможно, люди просто наобум ставили цифры в вопросе Q2. 

## Предложение
Было бы круто посмотреть, отличаются ли пользователи в своим оценкам по демографическим признакам. Может быть, для пенсионеров медленный Интернет не критичен, а вот для молодежи - да. И можно даже разные тарифы предлагать этим группам. 

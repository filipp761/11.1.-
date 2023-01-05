# 11.1. «Базы данных, их типы»


# Задание 1. СУБД
___
**Кейс**
Крупная строительная компания, которая также занимается проектированием и девелопментом, решила создать правильную архитектуру для работы с данными. Ниже представлены задачи, которые необходимо решить для каждой предметной области.

Какие типы СУБД, на ваш взгляд, лучше всего подойдут для решения этих задач и почему?

1.1. Бюджетирование проектов с дальнейшим формированием финансовых аналитических отчётов и прогнозирования рисков. СУБД должна гарантировать целостность и чёткую структуру данных.

1.1.* Хеширование стало занимать длительно время, какое API можно использовать для ускорения работы?

1.2. Под каждый девелоперский проект создаётся отдельный лендинг, и все данные по лидам стекаются в CRM к маркетологам и менеджерам по продажам. Какой тип СУБД лучше использовать для лендингов и для CRM? СУБД должны быть гибкими и быстрыми.

1.2.* Можно ли эту задачу закрыть одной СУБД? И если да, то какой именно СУБД и какой реализацией?

1.3. Отдел контроля качества решил создать базу по корпоративным нормам и правилам, обучающему материалу и так далее, сформированную согласно структуре компании. СУБД должна иметь простую и понятную структуру.

1.3.* Можно ли под эту задачу использовать уже существующую СУБД из задач выше и если да, то как лучше это реализовать?

1.4. Департамент логистики нуждается в решении задач по быстрому формированию маршрутов доставки материалов по объектам и распределению курьеров по маршрутам с доставкой документов. СУБД должна уметь быстро работать со связями.

1.4.* Можно ли к этой СУБД подключить отдел закупок или для них лучше сформировать свою СУБД в связке с СУБД логистов?

1.5.* Можно ли все перечисленные выше задачи решить, используя одну СУБД? Если да, то какую именно?

Приведите ответ в свободной форме.
___
**Ответ**

1.1 Реляционная, так при работе с финансами и аналитикой нам необходима постоянное поддержание целостности данных и изоляция процессов.

1.1* СУБД типа ключ-значение Чаще всего такие СУБД используют для кэширования, т.к. они очень быстро работают, а это и не сложно, когда есть уникальный ключ, и запрос возвращает только одно значение. подойдут Redis, Memcached. md5 - это быстрый хэш. То есть он предназначен для использования больших объемов данных и вывода хэша очень, очень быстро

1.2 NoSQL – ключ-значение, так как они легко настраиваются, обладают хорошо масштабируются и быстро реагируют на запросы информации, а менеджерам по продажам можно использовать графовую, чтобы эффективно анализировать запросы и предпочтения клиентов

1.2*

1.3 NoSQL –документо-ориенированые или иерархическая, так как нам необходима база, которая имеет четкую структуру и зависимости, а также позволяла легко изменять ее и осуществлять поиск обучающего материала

1.3* У MongoDB открытый исходный код. С помощью идентификатора можно осуществлять манипуляции над объектом с высокой скоростью. При сложных операциях СУБД тоже демонстрирует весьма хорошие показатели. Это связано с тем, что ПО относится к типу NoSQL и использует объектный язык запросов, который намного легче SQL

1.4 на мой взгляд тут подойдет графовая БД, чтобы анализировать маршруты и выдавать наиболее удобные

1.4* лучше для них сформировать отдельную базу, так как там необходима целостность данных и учитывать возможность блокировки транзакций, если товар заканчивается. Так же для них актуальна больше структурированная и надежная система, такая как реляционная

1.5 Можно, но надо оценивать требования по самому слабому звену, так как для финансово-аналитических отчетов нам необходима целостность и четкая структура. За счет четкой структуры мы потеряем в скорости обработки запросов

# Задание 2. Транзакции

2.1. Пользователь пополняет баланс счёта телефона, распишите пошагово, какие действия должны произойти для того, чтобы транзакция завершилась успешно. Ориентируйтесь на шесть действий.

2.1.* Какие действия должны произойти, если пополнение счёта телефона происходило бы через автоплатёж?

Приведите ответ в свободной форме.
___
**Ответ**

2.1
1.	Выбирается услуга (пополнение счета), проверятся её доступность;
2.	Вводится данные банковской карты, пин-код, проверка корректности веденных данных, формируется транзакция, которой присваивается номер;
3.	Подтверждаем введенные данные;
4.	После подтверждения данные направляются к оператору, которые в свою очередь перенаправляет их в банк получатель;
5.	Банк-получатель сверяет полученные данные, с данными счета, и или подтверждает, или отклоняет транзакцию;
6.	Банк направляет ответ о результате выполнения транзакции оператору, а он направляет ее нам. Результат счет пополнен или выдаст транзакция отклонена.

2.1.*
Порядок точно такой же, только в базе данных оператора фиксируется заранее установленные нами данные, о порядке автоматического пополнения счета (данные карты, условия и срок пополнения) и на основании сформированного расписания, выполняет задание на пополнение счета

# Задание 3. SQL vs NoSQL

3.1. Напишите пять преимуществ SQL-систем по отношению к NoSQL.

3.1.* Какие, на ваш взгляд, преимущества у NewSQL систем перед SQL и NoSQL.

Приведите ответ в свободной форме.
___
**Ответ**

3.1. Напишите пять преимуществ SQL-систем по отношению к NoSQL.
1.	Целостность данных;
2.	Надежность;
3.	Поддержка транзакций;
4.	Строгие правила проектирования;
5.	Отображение данных в наиболее простой для пользователя форме.

3.1.* Какие, на ваш взгляд, преимущества у NewSQL систем перед SQL и NoSQL.
(NewSQL объединила преимущества SQL и NoSQL, это базы дан¬ных, которые взя¬ли новые под¬ходы рас¬пре¬делен¬ных сис¬тем от NoSQL и оста¬вили реляци¬онную модель пред¬став¬ления дан¬ных и язык зап¬росов SQL. Мно¬гие NewSQL БД — это in-memory БД. Они хра¬нят все дан-ные в опе¬ратив¬ной памяти. Если тебе нуж¬но обра¬баты¬вать дан¬ные быс¬тро, нуж¬но хра¬нить их в ОЗУ. А отно¬ситель¬но неболь¬шой объ¬ем ОЗУ на одной машине мож¬но лег¬ко ком¬пенси-ровать, соб¬рав клас¬тер из сотен узлов. Они могут масштабироваться, зачастую по запросу, не влияя на логику приложения и не нарушая модель транзакций. Гибкость и безсерверная архитектура в сочетании с высокой степенью безопасности и доступности без применения резервных систем. Наиболее популярные базы данных NewSQL: ClustrixDB, CockroachDB, NuoDB, MemSQL и VoltDB.


# Задание 4. Кластеры

Необходимо производить большое количество вычислений при работе с огромным количеством данных, под эту задачу выделено 1000 машин.

На основе какого критерия будете выбирать тип СУБД и какая модель распределённых вычислений здесь справится лучше всего и почему?

Приведите ответ в свободной форме.
___
**Ответ**

Тип БД стоит выбирать по возможности ее масштабирования и скорости обмена данных, при таком объеме машин, лучше все подойдут NoSQL базы данных. Так как данные скорее всего будут разнородные и неструктурированные.

Инструментарий, позволяющий хранить и обрабатывать данные в Data Lake:
 - Hadoop — пакет утилит и библиотек, используемый для построения систем, обрабатывающих, хранящих и анализирующих большие массивы нереляционных данных: данные датчиков, интернет-трафика, объектов JSON, файлов журналов, изображений и сообщений в соцсетях.
 - HPPC (DAS) – суперкомпьютер, способный обрабатывать данные в режиме реального времени или в «пакетном состоянии». Реализован LexisNexis Risk Solutions.
 - Storm — фреймворк Big Data, созданный для работы с информацией в режиме реального времени. Разработан на языке программирования Clojure.
 - DataLake – помимо функции хранения, включает в себя и программную платформу (например, такую как Hadoop), а также определяет источники и методы пополнения данных, кластеры узлов хранения и обработки информации, управления, инструментов обучения. DataLake при необходимости масштабируется до многих сотен узлов без прекращения работы кластера.


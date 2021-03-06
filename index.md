Д.В. Горелик – студент кафедры вычислительных систем и сетей

А.В. Гордеев (проф., докт.техн.наук) – научный руководитель

### Цель:

Построить систему для работы с удаленными виртуальными машинами.

### Основные задачи:

Определиться в каком направлении двигаться в решении вопроса предоставления выполнения лабораторных работ в дистанционном курсе;
Провести исследование в установлении использовать гипервизорную или контейнерную виртуализацию.

Виртуализация. Понятие, разновидности. 
 Виртуализация — это технология запуска одной операционной системы поверх другой. В 2001 году компания VMware выпустила серверный продукт для виртуализации на основе гипервизора, привлекший внимание корпоративных заказчиков. Практически в то же самое время компания Parallels представила решение для контейнерной виртуализации Virtuozzo, заслужившее признание у пользователей. Такое разделение сохранялось почти 12 лет: гипервизорной виртуализации не удавалось завоевать сколько-нибудь значимый кусок рынка, а контейнеры не могли проникнуть в корпоративный сегмент. Перелом наметился в 2013 году, когда новый разработчик Docker привлек внимание представителей бизнеса к преимуществам контейнерной технологии.

 
Виды виртуализации | Достоинства | Недостатки
:---: | :---: | :---: 
**Гипервизор** | жесткое разделение аппаратных ресурсов; каждая виртуальная машина полностью изолирована от соседних; можно устанавливать любую операционную систему; VPS(виртуальный выделенный сервер)-сервер можно использовать под любые нужды; жесткое разделение ресурсов исключает манипуляции в виде оверселлинга. | создание лишних копий при работе с общими ресурсами; разделением аппаратных средств занимается гипервизор, а не основная ОС.
**Контейнер** | виртуализация на уровне операционной системы; совместное использование ядра операционной системы; возможность работать с одними и теми же данными не создавая их лишние копии; поскольку на физическом сервере используется только одно ядро, существенно экономятся ресурсы процессора, дисковое пространство. | на одном сервере нельзя запускать разные гостевые операционные системы; отсутствует жесткое разделение ресурсов; процесс исполнения команд несколько замедляется. 

 
### Пути построения системы работы с удаленными виртуальными машинами
 
1. Использование непосредственно Virtual Box в операционной системе сервера. Тут необходимо решить такие вопросы как: 
    * создание системы аутентификации для пользователей, чтобы они не имели доступ к виртуальным машинам друг друга;
    * систему которая позволяла бы при не штатных ситуациях отключения одно из серверов, продолжить работу на другом без потери данных;
	
2. Использование программного обеспечения Docker для обеспечения каждого пользователя контейнером. А уже после этого запускать Virtual Box непосредственно в контейнере. В отличии от первого способа это позволит:
    * изолировать пользователей друг от друга, так как контейнеры являются полностью изолированными окружениями;
    * при непредвиденной остановке работы позволяет восстановить работу контейнера с момента на котором произошел сбой не потеряв при этом никаких данных.

![alt]({{"/Алгоритм3.png" | absolute_url}}) 

Рисунок 1 – Алгоритм экспериментов

### Эксперимент № 2:
 
1. Количество виртуальных машин на сервере увеличено до 3, они полностью аналогичны друг другу и на них одновременно проводится установка  ОС Windows 7;
2. Количество контейнеров увеличено до 3. В каждом из них одновременно запущена своя Virtual Box с одной виртуальной машиной;
3. Как и в первом эксперименте используется внутренняя память серверов. 

### Эксперимент № 3:
 
1. Аналогично второму эксперименту количество виртуальных машин увеличено до 10 штук, а количество контейнеров увеличено до 5 штук, в каждом из которых установлено по 2 виртуальные машины. 
2. Самое главное отличие – это использование внешнего хранилища данных, для работы, которого используется протокол ISCSI.

### Эксперимент № 4(семестр 3):
 
1. Аналогично третьему эксперименту количество виртуальных машин 10 штук, а количество контейнеров 5 штук, в каждом из которых установлено по 2 виртуальные машины. 
2. Самое главное отличие – это использование внутреннего HDD хранилища данных расположенного непосредственно в сервере.

![alt]({{"/График 1.jpg" | absolute_url}}) 

Рисунок 2 – Нагрузка процессора и оперативной памяти на одном контейнере/виртуальной машине
 
![alt]({{"/График 2.jpg" | absolute_url}}) 

Рисунок 3 – Нагрузка процессора и оперативной памяти на трех контейнерах/виртуальных машинах

### Результаты первых двух  экспериментов:
  
1. Виртуальные машины работают быстрее, потребляют больше аппаратных ресурсов, у одной виртуальной машины на установку операционной системы уходит порядка 550 секунд, в то время как у контейнера уходит приблизительно 700 секунд. 
2. На всем протяжении работы, контейнер потребляет в общей сложности ресурсов меньше, чем виртуальная машина, но работает дольше.
3. Если увеличить количество виртуальных машин до 3 раз, то количество потребляемых аппаратных ресурсов увеличивается в 2,5 раза. При этом время установки ОС на трех машинах одновременно удлинилось, всего на 100 секунд, по сравнению с одной виртуальной машиной.
4. При одновременной работе трех контейнеров, общее потребление ресурсов так же возросло по сравнению с работой одного контейнера, а вот время работы увеличилось на 560 секунд.

![alt]({{"/График 3.jpg" | absolute_url}}) 

Рисунок 4 – Нагрузка процессора и оперативной памяти на десяти контейнерах/виртуальных машинах и хранилище подключенном по протоколу ISCSI

### Результаты третьего эксперимента:
  
 При использовании внешнего хранилища данных мы получили почти обратный первым двум экспериментам, результат:
1. Виртуальные машины работают медленнее, потребляя такое же количество аппаратных ресурсов. 
2. На всем протяжении работы, контейнеры потребляют в общей сложности ресурсов так же, как и виртуальные машины, но работают быстрее.
3. При увеличении количества виртуальных машин до 10, то количество потребляемых аппаратных ресурсов увеличивается в 2,5 раза по сравнению со вторым экспериментом. При этом время установки ОС на 10 машинах одновременно удлинилось, в 7,5 раза и составило 4,5 тысячи секунд.
4. При одновременной работе пяти контейнеров, общее потребление ресурсов так же возросло по сравнению с работой трех контейнеров, а вот время работы увеличилось в 3,5 раза, и составило 4 тысячи секунд, что на 500 секунд меньше чему у виртуальных машин.

![alt]({{"/График 4.JPG" | absolute_url}}) 

Рисунок 5 – Нагрузка процессора и оперативной памяти на десяти контейнерах/виртуальных машинах и данные хранятся на обычных HDD

### Результаты четвертого эксперимента:

 При использовании внутренного хранилища данных мы получили почти тот же результат, что и в третьем эксперименте, результат:
1. Виртуальные машины работают медленнее, потребляя большее количество аппаратных ресурсов. 
2. На всем протяжении работы, контейнеры потребляют в общей сложности меньше ресурсов,и работают быстрее.
3. При использовании внутрисерверного HDD диска контейнеры.

	Почему же контейнеры работают быстрее когда их больше? Мы знаем, что у каждого контейнера Docker своя собственная копия операционной системы, и вполне можно предположить, что тут могла быть таже проблема, что и с виртуальными машинами — тяжеловесные образы, которые содержат одно и тоже. Но по факту мы видим что в Docker это не так. Если использовать более 1 контейнера, основанного на одном и том же образе операционной системы, то файлы этой системы будут скачаны ровно один раз. Этот эффект достигается за счёт использования union file system.
	
	UnionFS — вспомогательная файловая система, производящая каскадно-объединённое монтирование других файловых систем. Это позволяет файлам и каталогам изолированных файловых систем, известных как ветви, прозрачно перекрываться, формируя единую связанную файловую систему. Каталоги, которые имеют тот же путь в объединённых ветвях, будет совместно отображать содержимое в объединённом каталоге новой виртуальной файловой системы. Когда ветви монтируются, то указывается приоритет одной ветви над другой. Следовательно, когда обе ветви содержат файл с идентичным именем, одна ветвь будет иметь больший приоритет. Различные ветви могут одновременно находиться в режиме «только чтение» и «чтение-запись», таким образом, запись в объединённую виртуальную файловую систему будет направлена на определённую реальную файловую систему. 
	
	Union file system состоит из слоёв (layers). Слои как бы накладываются друг на друга. Все используемые нами контейнеры используют общие защищенные от записи слои, в которых находятся неизменяемые файлы операционной системы Ubuntu. А для изменяемых файлов, каждый из контейнеров будет иметь собственный слой. Естественно, Docker использует это решение не только для операционной системы, но и для любых общих частей контейнеров, которые были созданы на основе общих «предков» их образов за счет этого он и выигравает по ресурсам и скорости работы.

### Выводы:
 
  Исследование показало, что потребление аппаратных ресурсов увеличивается практически одинаково с увеличением единиц виртуальных машин и контейнеров. Время выполнения действий так же растет, но с разной зависимостью. В случае виртуальных машин время растет на 1/5, а в случае контейнеров на 5/7 при использовании 3 виртуальных машин и контейнеров работающих на внутреннем жестком диске серверов. Но если взять внешнее хранилище данных, как в третьем эксперименте, то получим, обратную зависимость время виртуальных машин выросло в 7 раз, тогда как контейнеров всего в 3,5 раза. То есть контейнеры начинают работать быстрее. Как показал четвертый эксперимент, в котором было расширено пространство внутреннего хранилища серверов при помощи установки дополнительного HDD диска, все равно выигрывает контейнерное решение. А все благодаря тому, что Docker использует Union FS. Исходя из потребностей, в прототипе будет использованно общее хранилище данных. Теперь на основе полученных данных можно завершить прототип системы.

 

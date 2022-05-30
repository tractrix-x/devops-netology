# Домашнее задание к занятию "5.1. Введение в виртуализацию. Типы и функции гипервизоров. Обзор рынка вендоров и областей применения."
Как сдавать задания
Обязательными к выполнению являются задачи без указания звездочки. Их выполнение необходимо для получения зачета и диплома о профессиональной переподготовке.

Задачи со звездочкой (*) являются дополнительными задачами и/или задачами повышенной сложности. Они не являются обязательными к выполнению, но помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Любые вопросы по решению задач задавайте в чате учебной группы.

### Задача 1
Опишите кратко, как вы поняли: в чем основное отличие полной (аппаратной) виртуализации, паравиртуализации и виртуализации на основе ОС.

	Виртуализация на основе ОС  может использовать все ресурсы хостовй машины, использет общие динамические библиотеки, общие страницы  памяти на хостовой машине, что может экономить ресурсы. Обращение к ресурсам происходит без явного слоя гипервизора: разделение ресурсов осуществляется на уровне ОС как изолированные контейнеры на хосте.

	При Паравирутализации, госетвая ОС используют только собственные ресурсы выделенные гипервизором. Т.е. при паравиртуализации имеется слой Гипервизора,отвечающий за разделение ресурсов в хостовой машине.
	
	Полнаяаппа (аратная виртуализация) сразу использует аппаратные ресурсы сервера, ей не требуется ОС для управления виртуальными машинами.


### Задача 2
Выберите один из вариантов использования организации физических серверов, в зависимости от условий использования.

Организация серверов:

физические сервера,
паравиртуализация,
виртуализация уровня ОС.
Условия использования:

Высоконагруженная база данных, чувствительная к отказу.
Различные web-приложения.
Windows системы для использования бухгалтерским отделом.
Системы, выполняющие высокопроизводительные расчеты на GPU.
Опишите, почему вы выбрали к каждому целевому использованию такую организацию.

	Высоконагруженная база данных, чувствительная к отказу. -  Физический сервер (более высокий отклик, сокращение точек отказа) или аппаратная виртуализация (выделение максимального количества ресурсов для виртуальной машины).
	
	Различные web-приложения. - Виртуализация уровня ОС (т.к. требуется меньше ресурсов, более простое масштабирование)
	
	Windows системы для использования бухгалтерским отделом. - Паравиртуализация (добавлеине ресурсов на уровне виртуальной машины, возможность клонирования)
    
	Системы, выполняющие высокопроизводительные расчеты на GPU. - Физический сервер (более высокий отклик, сокращение точек отказа) или аппаратная виртуализация (выделение максимального количества ресурсов для виртуальной машины).
	
### Задача 3
Выберите подходящую систему управления виртуализацией для предложенного сценария. Детально опишите ваш выбор.

Сценарии:

100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
Требуется наиболее производительное бесплатное open source решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.
Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows инфраструктуры.
Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.

	Выбрала бы KVM т.к бесплатно и обеспечит производительность.


### Задача 4
Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, то создавали бы вы гетерогенную среду или нет? Мотивируйте ваш ответ примерами.

	Разные среды виртуализации являются подходящими для разных задач и отличаются по сложности администрирования (разные консоли, требования к аппаратной части, навыки администрирования), развертывания (рахные методы организации и распределения ресурсов СХД и серверов), масштабирования. Также гетерогенная среда требует большей затраты ресурсов для самих систем виртуализации. 
	Гетерогенную среду виртуализации использовала бы в компании где от разных систем виртуализации могла бы получить выгоду для сопровождаемых ИС. Для уменьшения рисков разделила бы задачи между администраторами по разным средам виртуализации, чтобы администратор был более узко направлен в рабочей области. 
	Если компания с небольшим количеством однородных систем, то не стала бы применять.
	
	

	
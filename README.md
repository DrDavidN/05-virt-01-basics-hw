# «Введение в виртуализацию. Типы и функции гипервизоров. Обзор рынка вендоров и областей применения» - Дрибноход Давид

## Задача 1

Опишите кратко, в чём основное отличие полной (аппаратной) виртуализации, паравиртуализации и виртуализации на основе ОС.

Ответ:

Полная (аппаратная) виртуализация - гипервизор устанавливается непосредственно на сервер и взаимодействует с аппаратным обеспечением сервера напрямую. Гостевая ОС полностью отделяется от управления инфраструктурой хоста, она не требует никаких изменений и не подозревает, что запущена в виртуальном окружении.

Паравиртуализация - гипервизор устанавливается на хостовую ОС и взаимодействует с аппаратным обеспечением сервера через прослойку ввиде хостовой ОС. Гостевая ОС модифицируется для коммуникации с гипервизором с целью улучшения производительности и эффективности работы. В ядре гостевой ОС модифицируются невиртуализуемые инструкции на гипервызовы, которые напрямую отправляются в гипервизор.

Виртуализация на основе ОС (контейнерная виртуализация) - ядро ОС поддерживает несколько изолированных экземпляров пространств пользователя и эти экземпляры с точки зрения пользователя полностью идентичны реальному серверу.

## Задача 2

Выберите один из вариантов использования организации физических серверов в зависимости от условий использования.

Организация серверов:
- физические сервера,
- паравиртуализация,
- виртуализация уровня ОС.

Условия использования:
- высоконагруженная база данных, чувствительная к отказу;
- различные web-приложения;
- Windows-системы для использования бухгалтерским отделом;
- системы, выполняющие высокопроизводительные расчёты на GPU.

Опишите, почему вы выбрали к каждому целевому использованию такую организацию.

Ответ:

1. Высоконагруженная база данных, чувствительная к отказу: виртуализация уровня ОС - так БД можно рассматривать, как приложение (сервис), его можно запаковать в контейнер, создать несколько таких контейнеров, тем самым получить отказоустойчивый кластер, управление контейнером сводится к управлению процессом - быстро и без накладных расходов.
2. Различные web-приложения: виртуализация уровня ОС - если приложения могут и должны работать на том же ядре, что и ядро хостовой ОС. Иначе можно использовать паравиртуализация. Так как это web-приложения, то это значит, что будет достаточно много обращений к ним, как к сервису, поэтому нам в первую очередь необходимо обеспечить хорошую производительность и снизить накладные расходы. Данный способ так же позволяет утилизировать по максимуму хостовые ресурсы, управление контейнерами позволит нам упростить в принципе управление стеком web-приложений. 
3. Windows системы для использования бухгалтерским отделом: паравиртуализация - системы для бухов - по большей части, не требуют от нас супер производительности и кол-во обращение более менее прогнозируемое и поддается подсчету. Паравиртуализация более масштабируемое решение, скорее всего можно сформировать единую точку управления.
4. Системы, выполняющие высокопроизводительные расчеты на GPU: физические сервера - так как потребуется установка мощных графических процессоров для обработки данных. Физические решения для графических вычислений более производительны.


## Задача 3

Выберите подходящую систему управления виртуализацией для предложенного сценария. Детально опишите ваш выбор.

Сценарии:
1. 100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based-инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
2. Требуется наиболее производительное бесплатное open source-решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.
3. Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows-инфраструктуры.
4. Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.

Ответ: 

1. VMWare. В рамках продуктов виртуализации и управления, решения от VMWare являются наиболее сбалансированными, имеют достаточно обширный функционал, который позволит закрыть вопросы по сопровождению и администрированию среды на 100 VM Windows, Linux.
2. KVM. Решение на базе KVM позволит управлять гостевыми ОС, как Windows, так и Linux, без особых потерь в производительности, что обеспечивает решение поставленной задачи. Проект развивающийся, это несет в себе риски, но ими можно пренебречь, так как производительность нас интересует в первую очередь. 
3. XEN PV. Решение позволит поднять инфраструктуру на базе Windows OS, является open source проектом, обеспечит высокую производительность.
4. Virtual Box совместно с Vagrant. Для создания окружения для тестирования подойдет open source решение Virtual Box, с помощью Vagrant можно добиться автоматизации сборки такого окружения, при помощи Vagrantfile отдавать данное окружения всем, кто заинтересован, это позволит точно быть уверенным, что среда у всех единая.

## Задача 4

Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, создавали бы вы гетерогенную среду или нет? Мотивируйте ваш ответ примерами.

Ответ: 

Выбор среды виртуализации делается на основании предпроектного анализа, рассматривается детальное описание требований к будущей реализации, если мы можем на проекте избежать большого разнообразия из продуктов и решений, то ни к чему вводить два или более решений по управлению виртуальной средой. Однако, если же этого не избежать, специфика поставленной задачи, такова, что гетерогенная среда будет оправдана, и с точки зрения затрат, и с точки зрения управления, то тогда необходимо внедрять такое решение, но учитывать особенности - две или более точки управления виртуальными средами тянет за собой необходимость привлекать более дорогостоющий персонал, увеличивается объем выполняемых работ, требуется разработка автоматизированных решений под обе среды, возможно это наложит существенные ограничения на выбор стека технологий по мониторингу, логированию и бекапированию или наоборот увеличит кол-во продуктов. В том числе значительно усложняется вопрос по реализации прозрачного процесса управления изменениями, то есть изменения, которые будут доставляться в одну среду, могут отличаться в смысле способа и метода доставки в другой среде. Для минимизации влияния всех описаных факторов, техническое решение должно быть проработано хорошо, задача отладки процессов управления сервисов в гетерогенной среде становиться крайне важна, в том числе для страховки необходимо будет прорабатывать детальный план DRP - disaster recovery plan.

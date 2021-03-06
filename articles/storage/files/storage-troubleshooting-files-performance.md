---
title: Руководство по устранению неполадок производительности файловых ресурсов Azure
description: Устранение известных проблем с производительностью файловых ресурсов Azure. Выявлять потенциальные причины и связанные с ними обходные пути при обнаружении этих проблем.
author: gunjanj
ms.service: storage
ms.topic: troubleshooting
ms.date: 11/16/2020
ms.author: gunjanj
ms.subservice: files
ms.openlocfilehash: a49dbdace01396656c3114df0bc0d4589aff57c1
ms.sourcegitcommit: f6236e0fa28343cf0e478ab630d43e3fd78b9596
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/19/2020
ms.locfileid: "94916497"
---
# <a name="troubleshoot-azure-file-shares-performance-issues"></a>Устранение проблем с производительностью файловых ресурсов Azure

В этой статье перечислены некоторые распространенные проблемы, связанные с файловыми ресурсами Azure. Он предоставляет потенциальные причины и способы их решения при возникновении этих проблем.

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>Высокая задержка, низкая пропускная способность и общие проблемы производительности

### <a name="cause-1-share-was-throttled"></a>Причина 1: ресурс был отрегулирован

Запросы регулируются при достижении предельных значений операций ввода-вывода в секунду, входящих или исходящих данных для общей папки. Сведения об ограничениях для файловых ресурсов уровня "Стандартный" и "Премиум" см. в разделе Общие папки [и целевые объекты масштабирования файлов](./storage-files-scale-targets.md#file-share-and-file-scale-targets).

Чтобы проверить, выполняется ли регулирование общей папки, вы можете получить доступ к метрикам Azure и использовать ее на портале.

1. Войдите в свою учетную запись хранения на портале Azure.

1. В области слева в разделе **мониторинг** выберите **метрики**.

1. Выберите **файл** в качестве пространства имен метрик для своей области учетной записи хранения.

1. Выберите **транзакции** в качестве метрики.

1. Добавьте фильтр для **типа ответа**, а затем проверьте, есть ли в запросах один из следующих кодов ответа:
   * **Сукцессвиссроттлинг**: для блока сообщений сервера (SMB)
   * **ClientThrottlingError**: для RESTful

   ![Снимок экрана параметров метрик для общих файловых ресурсов уровня "Премиум" с отображением фильтра свойств "тип ответа".](media/storage-troubleshooting-premium-fileshares/metrics.png)

   > [!NOTE]
   > Чтобы получить оповещение, см. раздел ["Создание оповещения, если файловый ресурс регулируется"](#how-to-create-an-alert-if-a-file-share-is-throttled) далее в этой статье.

### <a name="solution"></a>Решение

- Если вы используете стандартный файловый ресурс, включите [большие файловые ресурсы](./storage-files-how-to-create-large-file-share.md?tabs=azure-portal) в учетной записи хранения. Большие файловые ресурсы поддерживают до 10 000 операций ввода-вывода в секунду на общую папку.
- Если вы используете файловый ресурс уровня "Премиум", увеличьте размер подготовленной общей папки, чтобы увеличить ограничение числа операций ввода-вывода в секунду. Дополнительные сведения см. в разделе "Общие сведения о подготовке для файловых ресурсов Premium" в статье о [планировании файлов Azure](./storage-files-planning.md#understanding-provisioning-for-premium-file-shares).

### <a name="cause-2-metadata-or-namespace-heavy-workload"></a>Причина 2. интенсивная рабочая нагрузка метаданных или пространства имен

Если большинство запросов ориентированы на метаданные (например, CreateFile, OpenFile, клосефиле, куеринфо или куеридиректори), задержка будет хуже, чем в операциях чтения и записи.

Чтобы определить, являются ли большинство ваших запросов ориентированными на метаданные, начните с выполнения шагов 1-4, как описано выше в разделе Причина 1. Для шага 5 вместо добавления фильтра для **типа ответа** добавьте фильтр свойств для **имени API**.

![Снимок экрана параметров метрик для общих файловых ресурсов уровня "Премиум" с отображением фильтра свойств "имя API".](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

### <a name="workaround"></a>Обходной путь

- Проверьте, можно ли изменить приложение, чтобы сократить количество операций с метаданными.
- Добавьте виртуальный жесткий диск (VHD) в общую папку и подключите VHD через SMB от клиента для выполнения файловых операций с данными. Этот подход работает для одного модуля записи и нескольких сценариев чтения и позволяет локальным операциям с метаданными. Программа установки обеспечивает производительность, аналогичную конфигурации локального непосредственно подключенного хранилища.

### <a name="cause-3-single-threaded-application"></a>Причина 3. однопотоковое приложение

Если используемое приложение является однопотоковым, эта установка может привести к значительному снижению пропускной способности операций ввода-вывода в секунду, чем максимальная возможная пропускная способность, в зависимости от размера подготовленного общего ресурса.

### <a name="solution"></a>Решение

- Увеличьте параллелизм приложений, увеличив число потоков.
- Переключитесь на приложения, где возможно параллелизм. Например, для операций копирования можно использовать AzCopy или Robocopy из клиентов Windows или **параллельную** команду из клиентов Linux.

## <a name="very-high-latency-for-requests"></a>Очень высокая задержка для запросов

### <a name="cause"></a>Причина

Виртуальная машина клиента может находиться в другом регионе, чем общая папка.

### <a name="solution"></a>Решение

- Запустите приложение из виртуальной машины, расположенной в том же регионе, что и файловый ресурс.

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>Клиенту не удалось достичь максимальной пропускной способности, поддерживаемой сетью

### <a name="cause"></a>Причина
Одной из потенциальных причин является отсутствие поддержки нескольких каналов SMB для стандартных файловых ресурсов. Сейчас служба файлов Azure поддерживает только один канал, поэтому между клиентской виртуальной машиной и сервером установлено только одно подключение. Это отдельное подключение привязано к одному ядру на клиентской виртуальной машине, поэтому максимальная пропускная способность, достижимая с виртуальной машины, ограничивается одним ядром.

### <a name="workaround"></a>Обходной путь

- Для общих файловых ресурсов уровня "Премиум" [включите многоканальный SMB в учетной записи филестораже](storage-files-enable-smb-multichannel.md).
- Получение виртуальной машины с большим количеством ядер может помочь повысить пропускную способность.
- Запуск клиентского приложения с нескольких виртуальных машин увеличит пропускную способность.
- По возможности используйте API-интерфейсы RESTFUL.

## <a name="throughput-on-linux-clients-is-significantly-lower-than-that-of-windows-clients"></a>Пропускная способность клиентов Linux значительно ниже, чем у клиентов Windows.

### <a name="cause"></a>Причина

Это известная проблема с реализацией клиента SMB в Linux.

### <a name="workaround"></a>Обходной путь

- Распределите нагрузку между несколькими виртуальными машинами.
- На той же виртуальной машине используйте несколько точек подключения с параметром **ношаресокк** и Распределите нагрузку между этими точками подключения.
- В Linux попробуйте подключить с параметром **ностриктсинк** , чтобы избежать принудительного сброса SMB при каждом вызове **фсинк** . Для файлов Azure этот параметр не влияет на согласованность данных, но может привести к созданию устаревших метаданных файла в списках каталогов (команда **ls-l** ). Непосредственное выполнение запросов к метаданным файла с помощью команды **stat** Возвращает самые актуальные метаданные файлов.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>Высокие задержки для рабочих нагрузок, интенсивно использующих метаданные, с широкими операциями открытия и закрытия

### <a name="cause"></a>Причина

Отсутствие поддержки аренды каталогов.

### <a name="workaround"></a>Обходной путь

- По возможности старайтесь не использовать чрезмерный открытый или закрывающий маркер в одном каталоге в течение короткого периода времени.
- Для виртуальных машин Linux увеличьте время ожидания кэша записи каталога, указав **актимео = \<sec>** в качестве параметра подключения. По умолчанию время ожидания равно 1 секунде, поэтому может помочь большее значение, например 3 или 5 секунд.
- Для виртуальных машин CentOS Linux или Red Hat Enterprise Linux (RHEL) обновите систему до CentOS Linux 8,2 или RHEL 8,2. Для других виртуальных машин Linux обновите ядро до 5,0 или более поздней версии.

## <a name="low-iops-on-centos-linux-or-rhel"></a>Низкие числа операций ввода-вывода в CentOS Linux или RHEL

### <a name="cause"></a>Причина

Глубина ввода-вывода больше 1 не поддерживается в CentOS Linux или RHEL.

### <a name="workaround"></a>Обходной путь

- Выполните обновление до CentOS Linux 8 или RHEL 8.
- Измените на Ubuntu.

## <a name="slow-file-copying-to-and-from-azure-file-shares-in-linux"></a>Очень большое копирование файлов и из файловых ресурсов Azure в Linux

Если вы испытываете медленное копирование файлов, ознакомьтесь с разделом "медленное копирование файлов в файловые ресурсы Azure в Linux" в разделе [руководство по устранению неполадок Linux](storage-troubleshoot-linux-file-connection-problems.md#slow-file-copying-to-and-from-azure-files-in-linux).

## <a name="jittery-or-sawtooth-pattern-for-iops"></a>Шаблон колебаний или савтус для операций ввода-вывода

### <a name="cause"></a>Причина

Клиентское приложение постоянно превышает базовые операции ввода-вывода. В настоящее время загрузка запроса на стороне службы не выполняется. Если клиент превышает базовые показатели операций ввода-вывода, он регулируется службой. Регулирование может привести к тому, что клиент будет испытывать неограниченную или савтус последовательность операций ввода-вывода. В этом случае среднее число операций ввода-вывода в секунду, достигнутое клиентом, может быть ниже, чем базовые операции ввода-вывода.

### <a name="workaround"></a>Обходной путь
- Сократите нагрузку на запрос от клиентского приложения, чтобы общая папка не была отрегулирована.
- Увеличьте квоту общей папки, чтобы она не была отрегулирована.

## <a name="excessive-directoryopendirectoryclose-calls"></a>Чрезмерные вызовы Директорйопен/Директориклосе

### <a name="cause"></a>Причина

Если число вызовов **директорйопен/директориклосе** является частью основных вызовов API и вы не предполагаете, что клиент сделает такое множество вызовов, то эта неполадка может быть вызвана антивирусной программой, установленной на виртуальной машине клиента Azure.

### <a name="workaround"></a>Обходной путь

- Исправление для этой проблемы доступно в [апреле-обновлении платформы для Windows](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform).

## <a name="file-creation-is-slower-than-expected"></a>Создание файла выполняется медленнее, чем ожидалось

### <a name="cause"></a>Причина

Рабочие нагрузки, которые полагаются на создание большого количества файлов, не увидят существенного отличия производительности между файловыми ресурсами класса Premium и стандартными файловыми ресурсами.

### <a name="workaround"></a>Обходной путь

- Нет.

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Снижение производительности Windows 8.1 или сервера 2012 R2

### <a name="cause"></a>Причина

Больше ожидаемой задержки при доступе к общим файловым ресурсам Azure для рабочих нагрузок с интенсивным вводом-выводом.

### <a name="workaround"></a>Обходной путь

- Установите доступное [исправление](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1).

## <a name="smb-multichannel-option-not-visible-under-file-share-settings"></a>Параметр многоканального SMB не отображается в параметрах общей папки. 

### <a name="cause"></a>Причина

Либо подписка не зарегистрирована для этой функции, либо тип региона и учетной записи не поддерживается.

### <a name="solution"></a>Решение

Убедитесь, что ваша подписка зарегистрирована для многоканального компонента SMB. См. раздел [Приступая к работе](storage-files-enable-smb-multichannel.md#getting-started) . Убедитесь, что учетная запись имеет тип филестораже (учетная запись Premium) на странице обзора учетной записи. 

## <a name="smb-multichannel-is-not-being-triggered"></a>Многоканальный SMB не запускается.

### <a name="cause"></a>Причина

Последние изменения параметров конфигурации многоканального SMB без повторного подключения.

### <a name="solution"></a>Решение
 
-   После внесения изменений в клиент SMB для Windows или учетной записи многоканальных параметров конфигурации SMB необходимо отключить общий ресурс, подождать 60 с и повторно подключить общий ресурс для активации многоканального подключения.
-   Для клиентской ОС Windows создание нагрузки ввода-вывода с высокой глубиной очереди скажите длина очереди = 8, например копирование файла для активации многоканального SMB.  Для серверной ОС многоканальный SMB срабатывает с длина очереди = 1, что означает, как только вы начнете ввод-вывод в общую папку.

## <a name="high-latency-on-web-sites-hosted-on-file-shares"></a>Высокая задержка на веб-сайтах, размещенных в общих файловых ресурсах 

### <a name="cause"></a>Причина  

Большое количество уведомлений об изменении файлов в файловых ресурсах может привести к значительному увеличению задержек. Обычно это происходит с веб-сайтами, размещенными в файловых ресурсах со структурой глубоко вложенных каталогов. Типичным сценарием является веб-приложение, размещенное в IIS, где для каждого каталога в конфигурации по умолчанию настроено уведомление об изменении файла. Каждое изменение ([реаддиректоричанжесв](https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-readdirectorychangesw)) в общем ресурсе, зарегистрированном клиентом SMB для отправки уведомлений об изменениях от службы файлов клиенту, которое принимает системные ресурсы и выдает усугубляется с числом изменений. Это может вызвать регулирование общего ресурса и, таким образом, привести к большей задержке на стороне клиента. 

Для подтверждения можно использовать метрики Azure на портале. 

1. Войдите в свою учетную запись хранения на портале Azure. 
1. В меню слева в разделе Мониторинг выберите метрики. 
1. Выберите файл в качестве пространства имен метрик для своей области учетной записи хранения. 
1. Выберите транзакции в качестве метрики. 
1. Добавьте фильтр для ResponseType и проверьте, есть ли в запросах код ответа Сукцессвиссроттлинг (для SMB) или ClientThrottlingError (для остальных).

### <a name="solution"></a>Решение 

- Если уведомление об изменении файла не используется, отключите уведомление об изменении файла (предпочтительно).
    - [Отключите уведомление об изменении файла](https://support.microsoft.com/help/911272/fix-asp-net-2-0-connected-applications-on-a-web-site-may-appear-to-sto) , обновив фкнмоде. 
    - Обновите интервал опроса рабочего процесса IIS (W3WP) на 0, установив `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\W3SVC\Parameters\ConfigPollMilliSeconds ` в реестре и перезапустите процесс w3wp. Дополнительные сведения об этом параметре см. в разделе [общие разделы реестра, используемые многими частями служб IIS](/troubleshoot/iis/use-registry-keys#registry-keys-that-apply-to-iis-worker-process-w3wp).
- Увеличьте частоту опроса уведомлений об изменении файла, чтобы уменьшить объем данных.
    - Обновите интервал опроса рабочего процесса W3WP на более высокое значение (например, 10mins или 30mins) в соответствии с вашими требованиями. Задайте `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\W3SVC\Parameters\ConfigPollMilliSeconds ` [в реестре](/troubleshoot/iis/use-registry-keys#registry-keys-that-apply-to-iis-worker-process-w3wp) и перезапустите процесс w3wp.
- Если сопоставленный физический каталог веб-узла имеет вложенную структуру каталогов, можно попытаться ограничить область уведомлений об изменении файлов, чтобы уменьшить объем уведомлений. По умолчанию службы IIS используют конфигурацию из файлов Web.config в физическом каталоге, с которым сопоставлен виртуальный каталог, а также во всех дочерних каталогах в этом физическом каталоге. Если вы не хотите использовать файлы Web.config в дочерних каталогах, укажите значение false для атрибута Алловсубдирконфиг в виртуальном каталоге. Дополнительные сведения можно найти [здесь](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories). 
    - Задайте для параметра "Алловсубдирконфиг" виртуального каталога IIS в Web.Config *значение false* , чтобы исключить сопоставленные физические дочерние каталоги из области.  

## <a name="how-to-create-an-alert-if-a-file-share-is-throttled"></a>Создание оповещения в случае регулирования общей папки

1. Войдите в свою учетную запись хранения на портале Azure.
1. В разделе **мониторинг** выберите **оповещения**, а затем выберите **новое правило генерации оповещений**.
1. Выберите **изменить ресурс**, выберите **тип файлового ресурса** для учетной записи хранения, а затем нажмите кнопку **Готово**. Например, если имя учетной записи хранения — *contoso*, выберите ресурс Contoso/File.
1. Щелкните **Выбрать условие** , чтобы добавить условие.
1. В списке сигналов, которые поддерживаются для учетной записи хранения, выберите метрику **транзакции** .
1. На панели **Настройка логики сигнала** в раскрывающемся списке **имя измерения** выберите **тип ответа**.
1. В раскрывающемся списке **значения измерений** выберите **СУКЦЕССВИССРОТТЛИНГ** (для SMB) или **ClientThrottlingError** (для остальных).

   > [!NOTE]
   > Если не указано ни **сукцессвиссроттлинг** , ни значение измерения **ClientThrottlingError** , это означает, что ресурс не был отрегулирован. Чтобы добавить значение измерения, рядом с раскрывающимся списком **значения измерений** выберите **Добавить настраиваемое значение**, введите **сукцессвиссроттлинг** или **ClientThrottlingError**, нажмите кнопку **ОК**, а затем повторите шаг 7.

1. В раскрывающемся списке **имя измерения** выберите Общая **Папка**.
1. В раскрывающемся списке **значения измерений** выберите общую папку или общие папки, по которым вы хотите создать оповещение.

   > [!NOTE]
   > Если общий файловый ресурс является стандартным общим ресурсом, выберите **все текущие и будущие значения**. В раскрывающемся списке значения измерений не перечислены общие папки, так как метрики для общего ресурса недоступны для стандартных файловых ресурсов. Предупреждения регулирования для стандартных файловых ресурсов запускаются, если в учетной записи хранения используется регулирование любой общей папки, и предупреждение не определяет, какой файловый ресурс был отрегулирован. Так как метрики для общего ресурса недоступны для стандартных файловых ресурсов, рекомендуется использовать одну общую папку для каждой учетной записи хранения.

1. Определите параметры оповещения, введя **пороговое значение**, **оператор**, **гранулярность статистической обработки** и **частоту оценки**, а затем нажмите кнопку **Готово**.

    > [!TIP]
    > При использовании статического порога диаграмма метрик может помочь определить разумное пороговое значение, если общая папка в данный момент регулируется. Если вы используете динамическое пороговое значение, на диаграмме метрики отображаются рассчитанные пороговые значения на основе последних данных.

1. Выберите **выбрать группу действий**, а затем добавьте группу действий (например, электронную почту или SMS) в предупреждение либо выбрав существующую группу действий, либо создав новую группу действий.
1. Введите сведения о предупреждении, такие как имя, **Описание** и **серьезность** **правила оповещения**.
1. Выберите **Создать правило генерации оповещений**, чтобы создать оповещение.

Дополнительные сведения о настройке оповещений в Azure Monitor см. в разделе [Обзор оповещений в Microsoft Azure]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="how-to-create-alerts-if-a-premium-file-share-is-trending-toward-being-throttled"></a>Создание оповещений в случае регулирования общего файлового ресурса уровня "Премиум"

1. Войдите в свою учетную запись хранения на портале Azure.
1. В разделе **мониторинг** выберите **оповещения**, а затем выберите **новое правило генерации оповещений**.
1. Выберите **изменить ресурс**, выберите **тип файлового ресурса** для учетной записи хранения, а затем нажмите кнопку **Готово**. Например, если имя учетной записи хранения — *contoso*, выберите ресурс Contoso/File.
1. Щелкните **Выбрать условие** , чтобы добавить условие.
1. В списке сигналов, которые поддерживаются для учетной записи хранения, выберите метрику исходящего **трафика** .

   > [!NOTE]
   > Необходимо создать три отдельных предупреждения, которые будут получать оповещения о превышении установленного порогового значения для входящих, исходящих данных или транзакций. Это связано с тем, что оповещение активируется только при соблюдении всех условий. Например, если все условия помещаются в одно оповещение, будет выдаваться предупреждение только в том случае, если количество входящих, исходящих и транзакций превышает пороговое значение.

1. Прокрутите страницу вниз. В раскрывающемся списке **имя измерения** выберите Общая **Папка**.
1. В раскрывающемся списке **значения измерений** выберите общую папку или общие папки, по которым вы хотите создать оповещение.
1. Определите параметры оповещения, выбрав значения в раскрывающихся списках **оператор**, **пороговое значение**, **гранулярность статистической обработки** и **частота вычисления** , а затем нажмите кнопку **Готово**.

   Метрики исходящих, входящих и транзакций выражаются в минуту, хотя вы подготавливаете исходящий трафик, входящий трафик и операции ввода-вывода в секунду. Поэтому, например, если подготовленный исходящий трафик составляет 90 &nbsp; мебибитес в секунду (MIB/s) и требуется, чтобы порог был 80 &nbsp; % от подготовленного исходящего трафика, выберите следующие параметры оповещения: 
   - Для **порогового значения**: *75497472* 
   - Для **оператора**: *больше или равно*
   - Для **типа агрегирования**: *среднее значение*
   
   В зависимости от того, насколько шумным требуется оповещение, можно также выбрать значения **гранулярности статистической обработки** и **частоту оценки**. Например, если вы хотите, чтобы оповещение пропускало среднее значение за период времени, равное 1 часу, и вы хотите, чтобы правило оповещений выполнялось каждый час, выберите следующее:
   - **Детализация статистической обработки**: *1 час*
   - Для **частоты оценки**: *1 час*

1. Выберите **выбрать группу действий**, а затем добавьте группу действий (например, электронную почту или SMS) в предупреждение либо выбрав существующую группу действий, либо создав новую.
1. Введите сведения о предупреждении, такие как имя, **Описание** и **серьезность** **правила оповещения**.
1. Выберите **Создать правило генерации оповещений**, чтобы создать оповещение.

    > [!NOTE]
    > - Чтобы получать уведомления о том, что файловый ресурс уровня "Премиум" близок к регулированию *из-за подготовленного* входящего трафика, следуйте приведенным выше инструкциям, но со следующим изменением:
    >    - **На шаге 5 Выберите метрику** входящего **трафика** вместо исходящего.
    >
    > - Чтобы получать уведомления о том, что файловый ресурс уровня "Премиум" близок к регулированию *из-за подготовленных операций ввода-вывода*, следуйте приведенным выше инструкциям, но со следующими изменениями:
    >    - На шаге 5 Выберите метрику **транзакции** **вместо исходящего.**
    >    - На шаге 10 единственным параметром для **типа агрегирования** является *Total*. Таким образом, пороговое значение зависит от выбранной степени детализации статистической обработки. Например, если вы хотите, чтобы порог был 80 &nbsp; % от подготовленных базовых операций ввода-вывода и вы выбрали *1 час* для **гранулярности статистической обработки**, то **пороговое значение** будет базовым числом операций ввода-вывода в секунду (в байтах) &times; &nbsp; 0,8 &times; &nbsp; 3600. 

Дополнительные сведения о настройке оповещений в Azure Monitor см. в разделе [Обзор оповещений в Microsoft Azure]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="see-also"></a>См. также раздел
- [Устранение неполадок службы файлов Azure в Windows](storage-troubleshoot-windows-file-connection-problems.md)  
- [Устранение неполадок службы файлов Azure в Linux](storage-troubleshoot-linux-file-connection-problems.md)  
- [Вопросы и ответы о службе "Файлы Azure"](storage-files-faq.md)

---
title: Справочник по схеме нормализации данных для маркеров Azure | Документация Майкрософт
description: В этой статье показана схема нормализации данных Sentinel в Azure.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 09/08/2020
ms.author: yelevin
ms.openlocfilehash: eb1752ea66f2cbebf6a653705b5a760e8e268240
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90937607"
---
# <a name="azure-sentinel-data-normalization-schema-reference"></a>Справочник по схеме нормализации данных для маркеров Azure

## <a name="terminology"></a>Терминология

В схемах Sentinel используется следующая терминология:

| Термин | Определение |
| ---- | ---------- |
| Устройство создания отчетов | Система, отправляющая записи в Azure Sentinel. Возможно, это не предметная система записи. |
| Record | Единица данных, отправляемых с устройства отчетов. Часто это называется «log», «Event» или «Alert», но не обязательно должен быть одним из них. |
|

## <a name="data-types-and-formats"></a>Типы данных и форматы

Значения должны быть нормализованы в соответствии с приведенными ниже рекомендациями. Это обязательно для нормализованных полей и рекомендуется для других полей. 

| Тип данных | Физический тип | Формат и значение |
| --------- | ------------- | ---------------- |
| **Дата и время** | В зависимости от возможностей метода приема, используемых в нисходящем приоритете:<ul><li>Log Analytics встроенный тип даты и времени</li><li>Целочисленное поле с использованием Log Analytics числового представления даты и времени</li><li>Строковое поле с использованием Log Analytics числового представления даты и времени</li></ul> | Log Analytics представление DateTime. <br></br>Log Analytics Дата & времени похожа на природу, но отличается от представления времени Unix. См. рекомендации по преобразованию. <br></br>Дата & времени должна быть скорректирована по часовому поясу. |
| **MAC-адрес** | Строковый тип | Colon-Hexadecimal нотация |
| **IP-адрес**; | IP-адрес | Схема не имеет отдельных адресов IPv4 и IPv6. Любое поле IP-адреса может содержать либо адрес IPv4, либо IPv6-адрес.<ul><li>IPv4 в точечно-десятичной нотации</li><li>IPv6 в 8-хекстетс нотации, что позволяет использовать короткие формы, описанные здесь.</li></ul> |
| **Пользователь** | Строковый тип | Доступны следующие три поля пользователя:<ul><li>Имя пользователя</li><li>Имя участника-пользователя</li><li>Домен пользователя</li></ul> |
| **Идентификатор пользователя** | Строковый тип | В настоящее время поддерживаются следующие 2 идентификатора пользователей:<ul><li>Идентификатор безопасности пользователя</li><li>Идентификатор Azure Active Directory</li></ul> |
| **Устройство** | Строковый тип | Поддерживаются следующие 3 столбца устройства/узла:<ul><li>ID</li><li>Имя</li><li>Полное доменное имя (FQDN)</li></ul> |
| **Страна** | Строковый тип | Строка, использующая ISO 3166-1 в соответствии с этим приоритетом:<ul><li>Коды Alpha-2 (т. е. US для США)</li><li>Коды Alpha-3 (т. е. USA для США)</li><li>короткое имя;</li></ul> |
| **Регион** | Строковый тип | Имя подразделения страны по ISO 3166-2 |
| **Город** | Строковый тип | |
| **Долгот** | Double | Представление координат ISO 6709 (десятичное число со знаком) |
| **Широта** | Double | Представление координат ISO 6709 (десятичное число со знаком) |
| **Хэш-алгоритм** | Строковый тип | Поддерживаются следующие 4 хэш-столбца:<ul><li>MD5</li><li>SHA1</li><li>SHA256</li><li>SHA512</li></ul> |
| **Тип файла** | Строковый тип | Тип файла:<ul><li>Расширение</li><li>Класс</li><li>намедтипе</li></ul> |
| 

## <a name="network-sessions-table-schema"></a>Схема таблицы сетевых сеансов

Ниже приведена схема таблицы сетевых сеансов, версия 1.0.0

| Имя поля | Тип значения | Пример | Описание | Связанные сущности ОССЕМ |
|-|-|-|-|-|
| EventType | Строковый тип | Трафик | Тип собираемого события | Событие |
| евентсубтипе | Строковый тип | Аутентификация | Дополнительное описание типа, если применимо | Событие |
| EventCount | Целое число  | 10 | Число агрегированных событий, если применимо. | Событие |
| евентендтиме | Дата и время | См. раздел «типы данных». | Время окончания события | Событие |
| EventMessage | строка |  доступ запрещен | Общее сообщение или описание, которые либо включаются в, либо создаются из записи. | Событие |
| двЦипаддр | IP-адрес |  23.21.23.34 | IP-адрес устройства, создающего запись | устройство;<br>IP-адрес |
| двкмакаддр | Строковый тип | 06:10:9f: EB: 8F: 14 | MAC-адрес сетевого интерфейса устройства отчетов, с которого было отправлено событие. | устройство;<br>Mac |
| двчостнаме | Имя устройства (строка) | syslogserver1.contoso.com | Имя устройства, создающего сообщение. | Устройство |
| евентпродукт | Строковый тип | оффицешарепоинт | Продукт, создающий событие. | Событие |
| евентпродуктверсион | строка | 9.0 |  Версия продукта, создавшего событие. | Событие |
| евентресаурцеид | ИДЕНТИФИКАТОР устройства (строка) | /Subscriptions/3c1bb38c-82e3-4f8d-A115-a7110ba70d05/resourcegroups/contoso77/providers/Микрософт.компуте/виртуалмачинес/syslogserver1 | Идентификатор ресурса устройства, создающего сообщение. | Событие |
| евентрепортурл | Строковый тип | https://192.168.1.1/repoerts/ae3-56.htm | Ссылка на полный отчет, созданный устройством создания отчетов | Событие |
| евентвендор | Строковый тип | Microsoft | Поставщик продукта, создающего событие. | Событие |
| евентресулт | Незначный: успешно, частично, сбой, [пусто] (строка) | Успех | Результат, сообщаемый для действия. Пустое значение, если оно неприменимо. | Событие |
| евентресултдетаилс | Строковый тип | Неправильный пароль | Причина или подробные сведения для результата, полученного в Евентресулт | Событие |
| евентсчемаверсион | Real | 0,1 | Версия схемы Sentinel Azure. Сейчас 0,1. | Событие |
| евентсеверити | Строковый тип | Низкий | Если полученное действие имеет влияние на безопасность, обозначает серьезность воздействия. | Событие |
| евенторигиналуид | Строковый тип | af6ae8fe-ff43-4a4c-b537-8635976a2b51 | Идентификатор записи с устройства отчетов. | Событие |
| евентстарттиме | Дата и время | См. раздел «типы данных». | Время, в которое было указано событие | Событие |
| TimeGenerated | Дата и время | См. раздел «типы данных». | Время возникновения события, сообщаемое источником отчетов. | Настраиваемое поле |
| евенттимеинжестед | Дата и время | См. раздел «типы данных». | Время приема события в метку Azure. Будет добавлен Azure Sentinel. | Событие |
| евентуид | GUID (строка) | 516a64e3-8360-4f1e-a67c-d96b3d52df54 | Уникальный идентификатор, используемый Sentinel для пометки строки. | Событие |
| нетворкаппликатионпротокол | Строковый тип | HTTPS | Протокол уровня приложения, используемый соединением или сеансом. | Сеть |
| дстбитес | INT | 32455 | Число байтов, отправленных из места назначения в источник соединения или сеанса. | Назначение |
| сркбитес | INT | 46536 | Число байтов, отправленных из источника в место назначения для соединения или сеанса. | Источник |
| нетворкбитес | INT | 78991 | Число байтов, отправленных в обоих направлениях. Если Битесрецеивед и BytesSent существуют, Битестотал должен равняться их сумме. | Сеть |
| нетворкдиректион | Множественное значение: Inbound, Outbound (строка) | Входящий | Направление соединения или сеанса в организацию или из нее. | Сеть |
| дстжеоЦити | Строковый тип | Берлингтон | Город, связанный с IP-адресом назначения | Местоназначение<br>Географические |
| дстжеокаунтри | Country (строка) | США | Страна, связанная с исходным IP-адресом | Местоназначение<br>Географические |
| дстдвчостнаме | Имя устройства (строка) |  victim_pc | Имя устройства назначения | Назначение<br>Устройство |
| дстдвкфкдн | Строковый тип | victim_pc. contoso. local | Полное доменное имя узла, на котором был создан журнал | Местоназначение<br>Устройство |
| дстдомаинхостнаме | строка | CONTOSO | Домен назначения, домен узла назначения (веб-сайт, имя домена и т. д.), например для поисков DNS или уточняющих запросов NS. | Назначение |
| дстинтерфаценаме | строка | Microsoft Hyper-V сетевой адаптер | Сетевой интерфейс, используемый устройством назначения для подключения или сеанса. | Назначение |
| дстинтерфацегуид | строка | 2BB33827-6BB6-48DB-8DE6-DB9E0B9F9C9B | Идентификатор GUID сетевого интерфейса, который использовался для запроса проверки подлинности  | Назначение |
| дстипаддр | IP-адрес | 2001: db8:: FF00:42:8329 | IP-адрес подключения или назначения сеанса, который чаще всего называется IP назначения в сетевом пакете. | Местоназначение<br>IP-адрес |
| дстдвЦипаддр | IP-адрес | 75.22.12.2 | Конечный IP-адрес устройства, не связанного непосредственно с сетевым пакетом | Местоназначение<br>устройство;<br>IP-адрес
| дстжеолатитуде | Широта (Double) | 44,475833 | Широта географической координаты, связанная с IP-адресом назначения. | Местоназначение<br>Географические |
| дстмакаддр | Строковый тип | 06:10:9f: EB: 8F: 14 | MAC-адрес сетевого интерфейса, в котором подключение или сеанс прерваны, чаще всего называемые конечным компьютером MAC в сетевом пакете. | Местоназначение<br>АДРЕС |
| дстдвкмакаддр | Строковый тип | 06:10:9f: EB: 8F: 14 | Конечный MAC-адрес устройства, которое не связано непосредственно с сетевым пакетом. | Местоназначение<br>устройство;<br>АДРЕС |
| дстдвкдомаин | Строковый тип | CONTOSO | Домен целевого устройства. | Местоназначение<br>Устройство |
| дстпортнумбер | Целочисленный тип | 443 | Конечный IP-порт. | Местоназначение<br>Порт |
| дстжеорегион | Region (строка) | Vermont | Регион в стране, связанной с IP-адресом назначения | Местоназначение<br>Географические |
| дстресаурцеид | ИДЕНТИФИКАТОР устройства (строка) |  /Subscriptions/3c1bb38c-82e3-4f8d-A115-a7110ba70d05/resourcegroups/contoso77/providers/Микрософт.компуте/виртуалмачинес/виктим | Идентификатор ресурса целевого устройства. | Назначение |
| дстнатипаддр | IP-адрес | 2::1 | Если сообщение передается промежуточным устройством NAT, например брандмауэром, то IP-адрес, используемый устройством NAT для связи с источником. | Назначение NAT,<br>IP-адрес |
| дстнатпортнумбер | INT | 443 | Если сообщается промежуточным устройством NAT, например брандмауэром, то порт, используемый устройством NAT для связи с источником. | Назначение NAT,<br>Порт |
| дстусерсид | Идентификатор безопасности пользователя |  S – 12-1445 | Идентификатор пользователя удостоверения, связанного с назначением сеанса. Как правило, удостоверение используется для проверки подлинности сервера. Дополнительные сведения см. в разделе "типы данных". | Местоназначение<br>Пользователь |
| дстусераадид | Строка (GUID) | ae92b0b4-cfba-4b42-85a0-fbd862f4df54 | Идентификатор объекта учетной записи Azure AD пользователя на конечном конце сеанса | Местоназначение<br>Пользователь |
| дстусернаме | Username (строка) | Джон | Имя пользователя удостоверения, связанного с назначением сеанса.  | Местоназначение<br>Пользователь |
| дстусерупн | строка | johnd@anon.com | Имя участника-пользователя удостоверения, связанного с назначением сеанса. | Местоназначение<br>Пользователь |
| дстусердомаин | строка | РАБОЧАЯ ГРУППА | Домен или имя компьютера учетной записи в месте назначения сеанса | Местоназначение<br>Пользователь |
| дстзоне | Строковый тип | DMZ | Зона сети назначения, определенная на устройстве отчетов. | Назначение |
| дстжеолонгитуде | Долгота (Double) | — 73,211944 | Долгота географической координаты, связанная с IP-адресом назначения | Местоназначение<br>Географические |
| двкактион | Множественное значение: allow, Deny, Drop (строка) | Allow | Если передается промежуточным устройством, например брандмауэром, действие, предпринимаемое устройством. | Устройство |
| двЦинбаундинтерфаце | Строковый тип | eth0 | Если сообщение передается промежуточным устройством, например брандмауэром, то сетевой интерфейс, используемый им для подключения к исходному устройству. | Устройство |
| двкаутбаундинтерфаце | Строковый тип  | Ethernet адаптер Ethernet 4 | Если сообщение передается промежуточным устройством, например брандмауэром, то сетевой интерфейс, используемый им для подключения к целевому устройству. | Устройство |
| нетворкдуратион | Целочисленный тип | 1500 | Время в миллисекундах, необходимое для завершения сетевого сеанса или подключения | Сеть |
| нетворкикмпкоде | Целочисленный тип | 34 | Для ICMP-сообщения введите числовое значение типа ICMP (RFC 2780 или RFC 4443). | Сеть |
| нетворкикмптипе | Строковый тип | Объект назначения недоступен | Для сообщения ICMP введите текст сообщения типа ICMP (RFC 2780 или RFC 4443). | Сеть |
| дстпаккетс | INT  | 446 | Число пакетов, отправленных с места назначения в источник соединения или сеанса. Значение пакета определяется устройством создания отчетов. | Назначение |
| сркпаккетс | INT  | 6478 | Число пакетов, отправленных из источника в место назначения для соединения или сеанса. Значение пакета определяется устройством создания отчетов. | Источник |
| нетворкпаккетс | INT  | 0 | Число пакетов, отправленных в обоих направлениях. Если Паккетсрецеивед и Паккетссент существуют, Битестотал должен равняться их сумме. | Сеть |
| хттпрекуесттиме | Целочисленный тип | 700 | Время, затраченное на отправку запроса на сервер, если применимо. | Http |
| HttpResponseTime | Целочисленный тип | 800 | Время, затраченное на получение ответа на сервере (если применимо). | Http |
| нетворкруленаме | Строковый тип | анянидроп | Имя или идентификатор правила, по которому было принято решение Девицеактион | Сеть |
| нетворкруленумбер | INT |  23 | Соответствующий номер правила  | Сеть |
| нетворксессионид | строка | 172_12_53_32_4322__123_64_207_1_80 | Идентификатор сеанса, сообщаемый устройством отчетов. Например, идентификатор сеанса на уровне 7 для конкретных приложений после проверки подлинности | Сеть |
| сркжеоЦити | Строковый тип | Берлингтон | Город, связанный с исходным IP-адресом | Источника<br>Географические |
| сркжеокаунтри | Country (строка) | США | Страна, связанная с исходным IP-адресом | Источника<br>Географические |
| сркдвчостнаме | Имя устройства (строка) |  виллаин | Имя устройства исходного устройства | Источника<br>Устройство |
| сркдвкфкдн | строка | Villain.malicious.com | Полное доменное имя узла, на котором был создан журнал | Источника<br>Устройство |
| сркдвкдомаин | строка | евилорг | Домен устройства, с которого был инициирован сеанс | Источника<br>Устройство |
| сркдвкос | Строковый тип | iOS | ОС исходного устройства | Источника<br>Устройство |
| сркдвкмоделнаме | Строковый тип | Samsung Galaxy, Примечание | Имя модели исходного устройства | Источника<br>Устройство |
| сркдвкмоделнумбер | Строковый тип | 10.0 | Номер модели исходного устройства | Источника<br>Устройство |
| сркдвктипе | Строковый тип | Мобильные службы | Тип исходного устройства | Источника<br> Устройство |
| срЦинтефаценаме | Строковый тип | eth01 | Сетевой интерфейс, используемый исходным устройством для подключения или сеанса. | Источник |
| срЦинтерфацегуид | Строковый тип | 46ad544b-eaf0-47ef-827c-266030f545a6 | Идентификатор GUID используемого сетевого интерфейса | Источник |
| срЦипаддр | IP-адрес | 77.138.103.108 | IP-адрес, с которого поступило соединение или сеанс. | Источника<br>IP-адрес |
| сркдвЦипаддр | IP-адрес | 77.138.103.108 | Исходный IP-адрес устройства, не связанного непосредственно с сетевым пакетом (собранный поставщиком или явно вычисляемым). | Источника<br>устройство;<br>IP-адрес |
| сркжеолатитуде | Широта (Double) | 44,475833 | Широта географической координаты, связанная с исходным IP-адресом | Источника<br>Географические |
| сркжеолонгитуде | Долгота (Double) | — 73,211944 | Долгота географической координаты, связанная с исходным IP-адресом | Источника<br>Географические |
| сркмакаддр | Строковый тип | 06:10:9f: EB: 8F: 14 | MAC-адрес сетевого интерфейса, из которого создан сеанс OD. | Источника<br>Mac |
| сркдвкмакаддр | Строковый тип | 06:10:9f: EB: 8F: 14 | Исходный MAC-адрес устройства, которое не связано непосредственно с сетевым пакетом. | Источника<br>устройство;<br>Mac |
| сркпортнумбер | Целочисленный тип | 2335 | IP-порт, с которого было создано подключение. Не может быть релевантным для сеанса, включающего несколько соединений. | Источника<br>Порт |
| сркжеорегион | Region (строка) | Vermont | Регион в стране, связанный с исходным IP-адресом | Источника<br>Географические |
| сркресаурцеид | Строковый тип | /Subscriptions/3c1bb38c-82e3-4f8d-A115-a7110ba70d05/resourcegroups/contoso77/providers/Микрософт.компуте/виртуалмачинес/syslogserver1 | Идентификатор ресурса устройства, создающего сообщение. | Источник |
| сркнатипаддр | IP-адрес | 4.3.2.1 | Если сообщение передается промежуточным устройством NAT, например брандмауэром, то IP-адрес, используемый устройством NAT для связи с назначением. | Источник NAT,<br>IP-адрес |
| сркнатпортнумбер | Целочисленный тип | 345 | Если сообщается промежуточным устройством NAT, например брандмауэром, то порт, используемый устройством NAT для связи с назначением. | Источник NAT,<br>Порт |
| сркусерсид | Идентификатор пользователя (строка) | S – 15-1445 | Идентификатор пользователя удостоверения, связанного с источником сеансов. Как правило, пользователь выполняет действие на клиенте. Дополнительные сведения см. в разделе "типы данных". | Источника<br>Пользователь |
| сркусераадид | Строка (GUID) | 16c8752c-7dd2-4cad-9e03-fb5d1cee5477 | Идентификатор объекта учетной записи Azure AD для пользователя в исходном конце сеанса | Источника<br>Пользователь |
| сркусернаме | Username (строка) | Сергей | Имя пользователя удостоверения, связанного с источником сеансов. Как правило, пользователь выполняет действие на клиенте. Дополнительные сведения см. в разделе "типы данных". | Источник<br>Пользователь |
| сркусерупн | строка | bob@alice.com | Имя участника-пользователя для учетной записи, запускающей сеанс | Источника<br>Пользователь |
| сркусердомаин | строка | DESKTOP | Домен для учетной записи, запускающей сеанс | Источника<br>Пользователь |
| сркзоне | Строковый тип | Касание | Зона сети источника, определенная на устройстве отчетов. | Источник |
| NetworkProtocol | Строковый тип | TCP | IP-протокол, используемый соединением или сеансом. Обычно TCP, UDP или ICMP | Сеть |
| клаудаппнаме | Строковый тип | Facebook | Имя конечного приложения для HTTP-приложения, определяемое прокси-сервером. | Cloud |
| клаудаппид | Строковый тип | 124 | Идентификатор целевого приложения для HTTP-приложения, идентифицируемого прокси-сервером. Это значение обычно относится к используемому прокси-серверу. | Cloud |
| клаудаппоператион | Строковый тип | DeleteFile | Операция, выполненная пользователем в контексте приложения назначения для HTTP-приложения, определяемое прокси-сервером. Это значение обычно относится к используемому прокси-серверу. | Cloud |
| клаудапприсклевел | Строковый тип | 3 | Уровень риска, связанный с HTTP-приложением, определяемым прокси-сервером. Это значение обычно относится к используемому прокси-серверу. | Cloud |
| FileName | Строка | ImNotMalicious.exe | Имя файла, передаваемого по сетевым подключениям для таких протоколов, как FTP и HTTP, которые предоставляют сведения об имени файла. | Файл |
| FilePath | Строковый тип | C:\Malicious\ImNotMalicious.exe | Полный путь к файлу, включая имя файла | Файл |
| FileHashMd5 | Строковый тип | 51BC68715FC7C109DCEA406B42D9D78F | Значение хэша MD5 для файла, переданного по сетевым подключениям для протоколов. | Файл |
| FileHashSha1 | Строковый тип | 491AE3... C299821476F4 | Хэш-значение SHA1 файла, переданного по сетевым подключениям для протоколов. | Файл |
| FileHashSha256 | Строковый тип | 9B8F8EDB... C129976F03 | Значение хэша SHA256 файла, переданного по сетевым подключениям для протоколов. | Файл |
| FileHashSha512 | Строковый тип | 5E127D... F69F73F01F361 | Значение хэша SHA512 файла, переданного по сетевым подключениям для протоколов. | Файл |
| FileExtension |  Строковый тип | exe | Тип файла, передаваемого по сетевым подключениям для таких протоколов, как FTP и HTTP. | Файл
| филемиметипе | Строковый тип | приложение или мсворд | Тип MIME файла, передаваемого по сетевым подключениям для таких протоколов, как FTP и HTTP. | Файл |
| FileSize | Целочисленный тип | 23500 | Размер файла в байтах, переданного по сетевым подключениям для протоколов. | Файл |
| HttpVersion | Строковый тип | 2.0 | Версия HTTP-запроса для сетевых подключений HTTP/HTTPS. | Http |
| хттпрекуестмесод | Строковый тип | GET | Метод HTTP для сетевых сеансов HTTP/HTTPS. | Http |
| HttpStatusCode | Строковый тип | 404 | Код состояния HTTP для сетевых сеансов HTTP/HTTPS. | Http |
| хттпконтенттипе | Строковый тип | multipart/form-data; граница = что-то | Заголовок типа содержимого ответа HTTP для сетевых сеансов HTTP/HTTPS. | Http |
| хттпреферрероригинал | Строковый тип | https://developer.mozilla.org/en-US/docs/Web/JavaScript | Заголовок HTTP-источника ссылки для сетевых сеансов HTTP/HTTPS. | Http |
| хттпусераженторигинал | Строковый тип | Mozilla/5.0 (Windows NT 10,0; WOW64) Апплевебкит/537.36 (КХТМЛ, например Gecko) Chrome/83.0.4103.97 Safari/537.36 | Заголовок HTTP-агента пользователя для сетевых сеансов HTTP/HTTPS. | Http |
| хттпрекуестксфф | Строковый тип | 120.12.41.1 | Заголовок HTTP X-Forwardd-for для сетевых сеансов HTTP/HTTPS. | Http |
| урлкатегори | Строковый тип | Поисковые системы | Определенная группировка URL-адреса (или, возможно, на основе домена в URL-адресе) связана с тем, что он представляет (т. е. взрослые, Новости, реклама, приостановленные домены и т. д.). | url |
| урлоригинал | Строковый тип | https://contoso.com/fo/?k=v&q = u # f | URL-адрес запроса HTTP для сетевых сеансов HTTP/HTTPS. | Url |
| урлхостнаме | Строковый тип | contoso.com | Доменная часть URL-адреса запроса HTTP для сетевых сеансов HTTP/HTTPS. | Url |
| ThreatCategory | Строковый тип | Типа | Категория угрозы, определяемая системой безопасности, например шлюзом безопасности IP-адресов и связанная с этим сетевым сеансом. | Угроза |
| среатид | Строковый тип | TR. 124 | Идентификатор угрозы, определяемый системой безопасности, например шлюзом безопасности IP-адресов и связанный с этим сетевым сеансом. | Угроза |
| ThreatName | Строковый тип | Файл теста EICAR | Имя обнаруженной угрозы или вредоносной программы | Угроза |
| аддитионалфиелдс | Dynamic (контейнер JSON) | {<br>Свойство1: "val1",<br>Свойство2: "val2"<br>} | Если соответствующий столбец в схеме не совпадает, дополнительные поля могут храниться в контейнере JSON.<br>Для синтаксического анализа во время запроса рекомендуется не использовать этот метод, так как упаковка данных в JSON снизит производительность запросов. Вместо этого рекомендуется повысить уровень дополнительных столбцов.<br>Для будущих сценариев синтаксического анализа во время приема данных в этот столбец контейнера JSON будут собраны дополнительные данные. | Настраиваемое поле |
| 

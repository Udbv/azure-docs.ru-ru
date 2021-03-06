---
title: Основные сведения об использовании виртуальных машин Azure
description: Основные сведения об использовании виртуальных машин
services: virtual-machines
documentationcenter: ''
author: mimckitt
ms.author: mimckitt
ms.service: virtual-machines-linux
ms.topic: how-to
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 07/28/2020
ms.openlocfilehash: d43f94d3555a660d6b7c8f755eebfec253d31dc2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89322988"
---
# <a name="understanding-azure-virtual-machine-usage"></a>Основные сведения об использовании виртуальных машин Azure
Анализируя сведения об использовании Azure, можно получить важные аналитические данные, которые помогут оптимизировать управление затратами и их распределение в организации. В этом документе предоставлены подробные сведения об использовании вычислительных ресурсов Azure. Дополнительные сведения об использовании Azure см. в статье об [анализе счета](../cost-management-billing/understand/review-individual-bill.md).

## <a name="download-your-usage-details"></a>Скачивание сведений об использовании
Сначала [скачайте подробные сведения об использовании](../cost-management-billing/manage/download-azure-invoice-daily-usage-date.md#download-usage-in-azure-portal). В приведенной ниже таблице представлены определения и примеры использования виртуальных машин, развернутых при помощи Azure Resource Manager. В этом документе нет подробной информации о виртуальных машинах, развернутых с помощью классической модели.


| Поле | Значение | Примеры значений | 
|---|---|---|
| Дата использования | Дата использования ресурса | `11/23/2017` |
| Meter ID | Определяет службу верхнего уровня, к которой принадлежит это использование| `Virtual Machines`|
| Подкатегория средства измерения | Идентификатор средства измерения, по которому выставляется счет. <br><br> При использовании часов вычислений для каждого сочетания размера виртуальной машины, ОС (Windows или отличной от нее) и региона предусмотрена единица измерения. <br><br> При использовании программного обеспечения категории "Премиум" единица измерения предусмотрена для каждого типа программного обеспечения. Большинство образов программного обеспечения категории "Премиум" имеют различные единицы измерения для каждого размера ядра. Дополнительные сведения см. на [странице с ценами на вычисления](https://azure.microsoft.com/pricing/details/virtual-machines/) .</li></ul>| `2005544f-659d-49c9-9094-8e0aea1be3a5`|
| Имя средства измерения| Это характерно для каждой службы в Azure. Для вычислений это всегда "Часы вычислений".| `Compute Hours`|
| Регион средства измерения| В этом столбце указывается расположение центра обработки данных для определенных служб. От этого расположения может зависеть стоимость некоторых услуг.|  `JA East`|
| Единицы| Указывает единицу тарификации службы. Для вычислительных ресурсов действует почасовая оплата.| `Hours`|
| Потреблено| Объем ресурса, потребленный за один день. Для вычислений плата выставляется за каждую минуту работы виртуальной машины в течение часа (точность — до 6 десятичных знаков).| `1, 0.5`|
| Расположение ресурса  | Определяет центр обработки данных, где выполняется ресурс.| `JA East`|
| Потребленная служба | Используемая служба платформы Azure.| `Microsoft.Compute`|
| Группа ресурсов | Группа ресурсов, в которой выполняется развернутый ресурс. Дополнительные сведения см. в [обзоре Azure Resource Manager](../azure-resource-manager/management/overview.md).|`MyRG`|
| Идентификатор экземпляра | Идентификатор ресурса. Этот идентификатор содержит имя, заданное для ресурса при его создании. Для виртуальных машин идентификатор экземпляра будет содержать SubscriptionId, ResourceGroupName и VMName (или имя масштабируемого набора для его использования).| `/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/ resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachines/MyVM1`<br><br>или<br><br>`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/ resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachineScaleSets/MyVMSS1`|
| Теги| Тег, присваиваемый ресурсу. Используйте теги, чтобы группировать записи для выставления счетов. Сведения о том, как пометить виртуальные машины с помощью [интерфейса командной строки](./linux/tag.md) или [PowerShell](./windows/tag.md) , доступны только для Диспетчер ресурсов виртуальных машин.| `{"myDepartment":"RD","myUser":"myName"}`|
| Дополнительные сведения | Метаданные определенных служб. Для виртуальных машин в поле дополнительной информации добавляются следующие сведения. <br><br> Тип образа. Определенный выполняемый образ. Полный список поддерживаемых строк представлен ниже в разделе типов образов.<br><br> Тип службы. Размер развернутой среды.<br><br> VMName. Имя виртуальной машины. Это поле доступно только для виртуальных машин с масштабируемым набором. Имя виртуальной машины в масштабируемом наборе можно найти в строке идентификатора экземпляра выше.<br><br> UsageType. Указывает представляемый тип использования.<br><br> ComputeHR — это использование часа вычислений для базовой виртуальной машины, например Standard_D1_v2.<br><br> ComputeHR_SW — это плата за программное обеспечение категории "Премиум" (если на виртуальной машине используется программное обеспечение категории "Премиум", например Microsoft R Server). | Виртуальные машины<br>`{"ImageType":"Canonical","ServiceType":"Standard_DS1_v2","VMName":"", "UsageType":"ComputeHR"}`<br><br>Масштабируемые наборы виртуальных машин<br> `{"ImageType":"Canonical","ServiceType":"Standard_DS1_v2","VMName":"myVM1", "UsageType":"ComputeHR"}`<br><br>Программное обеспечение Premium<br> `{"ImageType":"","ServiceType":"Standard_DS1_v2","VMName":"", "UsageType":"ComputeHR_SW"}` |

## <a name="image-type"></a>Тип образа
Тип некоторых образов в коллекции Azure указывается в поле дополнительной информации. Это позволяет пользователям узнать, какие компоненты развернуты на виртуальной машине, и отслеживать их. В этом поле указываются следующие значения на основе развернутого образа:
- BitRock; 
- Каноническая FreeBSD 
- Open Logic; 
- Oracle; 
- SLES для SAP 
- SQL Server 14 (предварительная версия) на базе Windows Server 2012 R2 (предварительная версия); 
- SUSE
- SUSE Premium;
- Облачное устройство StorSimple 
- Red Hat
- Red Hat для бизнес-приложений SAP;     
- Red Hat для SAP HANA; 
- BYOL клиента Windows; 
- BYOL Windows Server; 
- Windows Server (предварительная версия). 

## <a name="service-type"></a>Тип службы
Значение поля типа службы в поле дополнительной информации соответствует размеру развернутой виртуальной машины. Виртуальные машины с хранилищем класса Premium (на базе SSD) и другого класса (на базе HDD) имеют одинаковую цену. Если вы развертываете размер на основе SSD (например, Стандартный \_ DS2 \_ v2), вы увидите размер, отличный от SSD ( `Standard\_D2\_v2 VM` ), в столбце Sub-Category счетчиков, а также SSD-size ( `Standard\_DS2\_v2` ) в поле дополнительных сведений.

## <a name="region-names"></a>Имена регионов
Имя региона, указанное в поле "Расположение ресурса" в разделе информации об использовании, отличается от имени региона, используемого в Azure Resource Manager. Ниже приведены значения в Resource Manager и их аналоги в поле расположения ресурса:

| **Имя региона в Resource Manager** | **Расположение ресурса в разделе сведений об использовании** |
|---|---|
| australiaeast |Восточная Австралия|
| australiasoutheast | Юго-Восточная Австралия|
| brazilsouth | Южная Бразилия|
| CanadaCentral | Центральная Канада|
| CanadaEast | Восточная Канада|
| CentralIndia | Центральная Индия|
| centralus | Центральная часть США|
| chinaeast | Восточный Китай|
| chinanorth | Северный Китай|
| eastasia | Восточная Азия|
| eastus | Восточная часть США|
| eastus2 | восточная часть США 2|
| GermanyCentral | Центральная Германия|
| GermanyNortheast | Северо-Восточная Германия|
| japaneast | Восточная Япония|
| japanwest | Западная Япония|
| KoreaCentral | Республика Корея, центральный регион|
| KoreaSouth | Республика Корея, южный регион|
| northcentralus | Центрально-северная часть США|
| northeurope | Северная Европа|
| southcentralus | Центрально-южная часть США|
| southeastasia | Юго-Восточная Азия|
| SouthIndia | Южная Индия|
| UKNorth | Север США|
| uksouth | южная часть Соединенного Королевства|
| UKSouth2 | южная часть Соединенного Королевства 2|
| ukwest | западная часть Соединенного Королевства|
| USDoDCentral | центральный регион US DoD|
| USDoDEast | восточный регион US DoD|
| USGov (Аризона) | USGov (Аризона)|
| usgoviowa | USGov Iowa|
| USGov (Техас) | USGov (Техас)|
| usgovvirginia | USGov (Вирджиния)|
| westcentralus | Центрально-западная часть США|
| westeurope | Западная Европа|
| WestIndia | Западная Индия|
| westus | западная часть США|
| westus2 | Западная часть США 2|


## <a name="virtual-machine-usage-faq"></a>Часто задаваемые вопросы об использовании виртуальных машин
### <a name="what-resources-are-charged-when-deploying-a-vm"></a>За какие ресурсы взимается плата при развертывании виртуальной машины?    
В виртуальных машинах взимается плата за саму виртуальную машину, программное обеспечение категории "Премиум", выполняемое на виртуальной машине, учетную запись хранения или управляемый диск, связанный с виртуальной машиной, а также за передачу пропускной способности сети виртуальной машине.
### <a name="how-can-i-tell-if-a-vm-is-using-azure-hybrid-benefit-in-the-usage-csv"></a>Как определить в CSV-файле с данными об использовании, применяется ли в виртуальной машине Преимущество гибридного использования Azure?
Если развертывание выполняется с помощью [Преимущества гибридного использования Azure](https://azure.microsoft.com/pricing/hybrid-benefit/), плата взимается по тарифам для виртуальной машины не под управлением Windows, так как вы используете собственную лицензию в облаке. В своем счете вы можете определить виртуальные машины Resource Manager, для которых применяется Преимущество гибридного использования Azure, так как в столбце ImageType есть значение "Windows\_Server BYOL" или "Windows\_Client BYOL".
### <a name="how-are-basic-vs-standard-vm-types-differentiated-in-the-usage-csv"></a>Как дифференцируются типы виртуальных машин категории "Базовый" и "Стандартный" в CSV-файле с данными об использовании?
Предлагаются виртуальные машины серии А категории "Базовый" и "Стандартный". Если развернуть виртуальную машину категории "Базовый", в подкатегории индикатора у нее будет строка "Базовый". При развертывании виртуальной машины серии А уровня "Стандартный" ее размером будет "A1 VM", так как "Стандартный" — это значение по умолчанию. Дополнительные сведения о различиях категорий "Базовый"и "Стандартный" см. на [странице с ценами](https://azure.microsoft.com/pricing/details/virtual-machines/).
### <a name="what-are-extrasmall-small-medium-large-and-extralarge-sizes"></a>Что собой представляют размеры "Сверхмалый", "Малый", "Средний", "Большой" и "Сверхбольшой"?
Размеры от сверхмалого до сверхбольшого — это устаревшие названия для Standard\_A0–Standard\_A4. Такие названия могут встречаться в записях об использовании классических виртуальных машин при описании развертывания этих размеров.
### <a name="what-is-the-difference-between-meter-region-and-resource-location"></a>В чем разница между регионом учета и расположением ресурса?
Регион учета связан с единицей измерения. Для некоторых служб Azure, в которых используется одна цена для всех регионов, поле региона учета может быть пустым. Но так как для виртуальных машин назначаются цены по регионам, это поле заполняется. Расположение ресурсов для виртуальных машин — это расположение, в котором развернута виртуальная машина. Регионы Azure в обоих полях одни и те же, даже если для строки предусмотрено другое соглашение об именовании регионов.
### <a name="why-is-the-imagetype-value-blank-in-the-additional-info-field"></a>Почему значение ImageType не указано в поле дополнительной информации?
Поле ImageType заполняется только для подмножества образов. Если вы не развертывали один из образов выше, значение ImageType не указывается.
### <a name="why-is-the-vmname-blank-in-the-additional-info"></a>Почему значение VMName не указано в поле дополнительной информации?
VMName указывается в поле дополнительной информации только для виртуальных машин в масштабируемом наборе. Поле InstanceID содержит имя виртуальной машины для виртуальных машин вне масштабируемого набора.
### <a name="what-does-computehr-mean-in-the-usagetype-field-in-the-additional-info"></a>Что означает ComputeHR в поле UsageType раздела дополнительной информации?
ComputeHR — это час вычислений, который представляет событие использования для стоимости базовой инфраструктуры. Если UsageType имеет значение ComputeHR\_SW, событие использования представляет собой плату за программное обеспечение категории "Премиум" для виртуальной машины.
### <a name="how-do-i-know-if-i-am-charged-for-premium-software"></a>Как узнать, взимается ли плата за программное обеспечение категории "Премиум"?
При просмотре образа виртуальной машины, который лучше подходит для ваших нужд, обязательно ознакомьтесь с [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute). Для образа предусмотрен тариф по плану на программное обеспечение. Если используется тариф "Бесплатный", дополнительная плата за программное обеспечение не взимается. 
### <a name="what-is-the-difference-between-microsoftclassiccompute-and-microsoftcompute-in-the-consumed-service"></a>В чем разница между Microsoft.ClassicCompute и Microsoft.Compute в области используемой службы?
Microsoft.ClassicCompute представляет классические ресурсы, развернутые с помощью Azure Service Manager. При развертывании с помощью Resource Manager в области используемой службы указывается значение Microsoft.Compute. Дополнительные сведения о моделях развертывания Azure см. [здесь](../azure-resource-manager/management/deployment-models.md).
### <a name="why-is-the-instanceid-field-blank-for-my-virtual-machine-usage"></a>Почему поле InstanceID остается пустым для используемой виртуальной машины?
При развертывании с помощью классической модели развертывания строка InstanceID недоступна.
### <a name="why-are-the-tags-for-my-vms-not-flowing-to-the-usage-details"></a>Почему теги для моих виртуальных машин не передаются в сведениях об использовании?
Теги передаются в CSV-файл с данными об использовании только для виртуальных машин Resource Manager. Теги классических ресурсов в данных об использовании недоступны.
### <a name="how-can-the-consumed-quantity-be-more-than-24-hours-one-day"></a>Как использованное количество может превышать 24 часа в сутки?
В классической модели плата за ресурсы начисляется на уровне облачной службы. Если в облачной службе несколько виртуальных машин, которые используют те же тарифные единицы, данные о потреблении объединяются. Счета выставляются на уровне виртуальной машины, если она развернута через Resource Manager, поэтому объединение не применяется.
### <a name="why-is-pricing-not-available-for-dsfsgsls-sizes-on-the-pricing-page"></a>Почему на странице с ценами недоступны тарифы для размеров DS/FS/GS/LS?
Виртуальные машины с поддержкой хранилища класса "Премиум" оплачиваются по тому же тарифу, что и виртуальные машины, не поддерживающие такое хранилище. Отличаются только затраты на хранение. Дополнительные сведения см. на [странице с ценами на хранилище](https://azure.microsoft.com/pricing/details/storage/unmanaged-disks/).

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения об использовании см. в статье [Расшифровка счета за использование Microsoft Azure](../cost-management-billing/understand/review-individual-bill.md).

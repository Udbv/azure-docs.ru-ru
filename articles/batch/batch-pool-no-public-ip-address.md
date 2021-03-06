---
title: Создание пула пакетной службы Azure без общедоступных IP-адресов
description: Узнайте, как создать пул без общедоступных IP-адресов
author: pkshultz
ms.topic: how-to
ms.date: 10/08/2020
ms.author: peshultz
ms.custom: references_regions
ms.openlocfilehash: 09a5632f969117e69e68bbe0df2bfbab9a8a102b
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/18/2020
ms.locfileid: "94842141"
---
# <a name="create-an-azure-batch-pool-without-public-ip-addresses"></a>Создание пула пакетной службы Azure без общедоступных IP-адресов

При создании пула пакетной службы Azure можно подготавливать пул конфигурации виртуальных машин без общедоступного IP-адреса. В этой статье объясняется, как настроить пул пакетной службы без общедоступных IP-адресов.

## <a name="why-use-a-pool-without-public-ip-addresses"></a>Зачем использовать пул без общедоступных IP-адресов?

По умолчанию всем вычисленным узлам в пуле конфигурации виртуальной машины пакетной службы Azure назначается общедоступный IP-адрес. Этот адрес используется пакетной службой для планирования задач и обмена данными с узлами вычислений, включая исходящий доступ к Интернету.

Чтобы ограничить доступ к этим узлам и снизить возможность обнаружения этих узлов из Интернета, можно предоставить пул без общедоступных IP-адресов.

> [!IMPORTANT]
> Поддержка пулов без общедоступных IP-адресов в пакетной службе Azure в настоящее время доступна в общедоступной предварительной версии для следующих регионов: Франция Central, Восточная Азия, Западная Центральная часть США, Юго-Центральный регион США, Западная часть США 2, восток США, Северная Европа, Восточная часть США 2, Центральная часть США, Западная Европа, Восточная Австралия Северо-Запад, Западная часть США.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Предварительные требования

- **Проверка подлинности**. Чтобы использовать пул без общедоступных IP-адресов в [виртуальной сети](./batch-virtual-network.md), API клиента пакетной службы должен использовать проверку подлинности Azure Active Directory (AD). Поддержка Azure AD пакетной службой Azure описана в статье [Аутентификация решений пакетной службы с помощью Active Directory](batch-aad-auth.md). Если вы не создаете пул в виртуальной сети, можно использовать проверку подлинности Azure AD или проверку подлинности на основе ключей.

- **Виртуальная сеть Azure**. Если вы создаете пул в [виртуальной сети](batch-virtual-network.md), следуйте этим требованиям и конфигурациям. Чтобы заранее подготовить виртуальную сеть с одной или несколькими подсетями, вы можете использовать портал Azure, Azure PowerShell, интерфейс командной строки Azure (CLI) или другие методы.
  - Виртуальная сеть должна находиться в тех же подписке и регионе, что и учетная запись пакетной службы, которую вы используете для создания пула.
  - Указанная для пула подсеть должна иметь достаточное количество свободных IP-адресов для всех виртуальных машин, предназначенных для пула, то есть сумму свойств `targetDedicatedNodes` и `targetLowPriorityNodes` этого пула. Если в подсети недостаточно свободных IP-адресов, пакетная служба ограничивает выделение вычислительных узлов для пула и генерирует ошибку изменения размера.
  - Необходимо отключить политики службы частной связи и сети конечных точек. Это можно сделать с помощью Azure CLI. ```az network vnet subnet update --vnet-name <vnetname> -n <subnetname> --resouce-group <resourcegroup> --disable-private-endpoint-network-policies --disable-private-link-service-network-policies```

> [!IMPORTANT]
> Для каждого 100 выделенных или узлов с низким приоритетом Пакетная служба выделяет одну службу частной связи и одну подсистему балансировки нагрузки. Эти ресурсы ограничены [квотами ресурсов](../azure-resource-manager/management/azure-subscription-service-limits.md) в подписке. Для больших пулов может потребоваться [запросить увеличение квоты](batch-quota-limit.md#increase-a-quota) для одного или нескольких из этих ресурсов. Кроме того, не следует применять блокировки ресурсов к любому ресурсу, созданному пакетной службой, так как это предотвращает очистку ресурсов в результате действий, инициированных пользователем, таких как Удаление пула или изменение размера до нуля.

## <a name="current-limitations"></a>Текущие ограничения

1. Пулы без общедоступных IP-адресов должны использовать конфигурацию виртуальной машины, а не конфигурацию облачных служб.
1. [Конфигурация пользовательской конечной точки](pool-endpoint-configuration.md) для пакетных вычислений узлов не работает с пулами без общедоступных IP-адресов.
1. Так как нет общедоступных IP-адресов, вы не можете [использовать собственные заданные общедоступные IP-адреса](create-pool-public-ip.md) с этим типом пула.

## <a name="create-a-pool-without-public-ip-addresses-in-the-azure-portal"></a>Создание пула без общедоступных IP-адресов в портал Azure

1. Войдите в свою учетную запись пакетной службы на портале Azure.
1. В окне **Параметры** слева выберите **Пулы**.
1. В окне **Пулы** выберите **Добавить**.
1. В окне **Добавить пул** из раскрывающегося списка **Тип образа** выберите нужный вариант.
1. Выберите правильный **издатель, предложение или номер SKU** вашего образа.
1. Укажите оставшиеся обязательные параметры, включая **размер узла**, **целевые выделенные узлы** и **узлы с низким приоритетом**, а также любые необходимые дополнительные параметры.
1. При необходимости выберите виртуальную сеть и подсеть, которые вы хотите использовать. Эта виртуальная сеть должна находиться в той же группе ресурсов, что и создаваемый пул.
1. В списке **Тип подготовки IP-адреса** выберите **нопублиЦипаддрессес**.

![Снимок экрана: экран добавления пула с выбранным НопублиЦипаддрессес.](./media/batch-pool-no-public-ip-address/create-pool-without-public-ip-address.png)

## <a name="use-the-batch-rest-api-to-create-a-pool-without-public-ip-addresses"></a>Использование REST API пакетной службы для создания пула без общедоступных IP-адресов

В приведенном ниже примере показано, как использовать [пакетную службу Azure REST API](/rest/api/batchservice/pool/add) для создания пула, использующего общедоступные IP-адреса.

### <a name="rest-api-uri"></a>Универсальный код ресурса (URI) REST API

```http
POST {batchURL}/pools?api-version=2020-03-01.11.0
client-request-id: 00000000-0000-0000-0000-000000000000
```

### <a name="request-body"></a>Текст запроса

```json
"pool": {
     "id": "pool2",
     "vmSize": "standard_a1",
     "virtualMachineConfiguration": {
          "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.040-LTS"
          },
     "nodeAgentSKUId": "batch.node.ubuntu 16.04"
     }
     "networkConfiguration": {
          "subnetId": "/subscriptions/<your_subscription_id>/resourceGroups/<your_resource_group>/providers/Microsoft.Network/virtualNetworks/<your_vnet_name>/subnets/<your_subnet_name>",
          "publicIPAddressConfiguration": {
               "provision": "NoPublicIPAddresses"
          }
     },
     "resizeTimeout": "PT15M",
     "targetDedicatedNodes": 5,
     "targetLowPriorityNodes": 0,
     "taskSlotsPerNode": 3,
     "taskSchedulingPolicy": {
          "nodeFillType": "spread"
     },
     "enableAutoScale": false,
     "enableInterNodeCommunication": true,
     "metadata": [
    {
      "name": "myproperty",
      "value": "myvalue"
    }
       ]
}
```

## <a name="outbound-access-to-the-internet"></a>Исходящий доступ к Интернету

В пуле без общедоступных IP-адресов виртуальные машины не смогут получить доступ к общедоступному Интернету, если вы не настроили настройку сети соответствующим образом, например, с помощью [NAT виртуальной сети](../virtual-network/nat-overview.md). Обратите внимание, что NAT разрешает исходящий доступ к Интернету только с виртуальных машин в виртуальной сети. Созданные пакетом кластерные узлы не будут общедоступными, так как они не связаны с общедоступными IP-адресами.

Другим способом обеспечения исходящего подключения является использование определяемого пользователем маршрута (UDR). Это позволяет перенаправлять трафик на прокси-компьютер, имеющий доступ к Интернету.

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [создании пулов в виртуальной сети](batch-virtual-network.md).
- Узнайте, как [использовать частные конечные точки с учетными записями пакетной](private-connectivity.md)службы.

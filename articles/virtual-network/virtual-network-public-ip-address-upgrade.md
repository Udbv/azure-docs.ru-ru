---
title: Обновление общедоступных IP-адресов
titleSuffix: Azure Virtual Network
description: Обновите общедоступные IP-адреса с Basic на Standard.
services: virtual-network
documentationcenter: na
author: blehr
editor: ''
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.subservice: ip-services
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/07/2020
ms.author: blehr
ms.custom: references_regions , devx-track-azurecli
ms.openlocfilehash: 0c248149694c2bf66b8c94e9c0a29a8f7da9f4e4
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/18/2020
ms.locfileid: "94843746"
---
# <a name="upgrade-public-ip-addresses"></a>Обновление общедоступных IP-адресов

Общедоступные IP-адреса Azure создаются с номером SKU — Basic или Standard, который определяет аспекты их работы (включая метод распределения, использование в разных зонах доступности и ресурсы, с которыми они могут быть связаны). 

В этой статье рассматриваются следующие сценарии:
* Обновление общедоступного IP-адреса базового SKU до общедоступного IP-адреса SKU уровня "Стандартный" (с помощью портала, PowerShell или интерфейса командной строки)
* Как перенести классическую зарезервированный IP-адрес Azure на общедоступный IP-адрес Azure Resource Manager Basic SKU

## <a name="upgrade-public-ip-address-from-basic-to-standard-sku"></a>Обновление общедоступного IP-адреса с базового на стандартный SKU

>[!NOTE]
>Возможность обновления общедоступных IP-адресов с уровня "базовый" до "Стандартный" доступна не во всех регионах.  Дополнительные сведения см. в разделе [**ограничения**](#limitations) .

Чтобы обновить общедоступный IP-адрес, он не должен быть связан ни с одним ресурсом (Дополнительные сведения о том, как отменять связь с общедоступными IP, см. на [этой странице](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#view-modify-settings-for-or-delete-a-public-ip-address) ).

>[!IMPORTANT]
>Общедоступные IP-адреса, обновленные с уровня "базовый" до SKU "Стандартный", остаются без [зон доступности](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones).  Это означает, что они не могут быть связаны с ресурсом Azure, который является избыточным в виде зоны, или привязан к предварительно указанной зоне в регионах, где это предлагается.

---
# <a name="basic-to-standard---powershell"></a>[**Базовый для Standard-PowerShell**](#tab/option-upgrade-powershell)

В следующем примере предполагается, что ранее было создано общедоступный IP-адрес базового SKU с использованием примера, указанного на [этой странице](https://docs.microsoft.com/azure/virtual-network/create-public-ip-powershell?tabs=option-create-public-ip-basic) с базовым общедоступным IP-адресом **мибасикпублиЦип** в **myResourceGroup**.

Чтобы обновить IP-адрес, просто выполните приведенные ниже команды с помощью PowerShell.  Примечание. Если IP-адрес уже выделен статически, этот раздел можно пропустить.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$name = 'myBasicPublicIP'
$newsku = 'Standard'
$pubIP = Get-AzPublicIpAddress -name $name -ResourceGroupName $rg

## This section is only needed if the Basic IP is not already set to Static ##
$pubIP.PublicIpAllocationMethod = 'Static'
Set-AzPublicIpAddress -PublicIpAddress $pubIP

## This section is for conversion to Standard ##
$pubIP.Sku.Name = $newsku
Set-AzPublicIpAddress -PublicIpAddress $pubIP
```

# <a name="basic-to-standard---cli"></a>[**Базовый до Standard-CLI**](#tab/option-upgrade-cli)

В следующем примере предполагается, что ранее было создано общедоступный IP-адрес базового SKU с использованием примера, указанного на [этой странице](https://docs.microsoft.com/azure/virtual-network/create-public-ip-cli?tabs=option-create-public-ip-basic) с базовым общедоступным IP-адресом **мибасикпублиЦип** в **myResourceGroup**.

Чтобы обновить IP-адрес, просто выполните приведенные ниже команды с помощью Azure CLI.  Примечание. Если IP-адрес уже выделен статически, этот раздел можно пропустить.

```azurecli-interactive
## Variables for the command ##
$rg = 'myResourceGroup'
$name = 'myBasicPublicIP'

## This section is only needed if the Basic IP is not already set to Static ##
az network public-ip update \
--resource-group $rg \
--name $name \
--allocation-method Static 

## This section is for conversion to Standard ##
az network public-ip update \
--resource-group $rg \
--name $name \
--sku Standard
```
---

## <a name="upgrade-migrate-a-classic-reserved-ip-to-a-static-public-ip"></a>Обновление (миграция) классической зарезервированный IP-адрес на статический общедоступный IP-адрес

Чтобы воспользоваться преимуществами новых возможностей Azure Resource Manager, можно перенести существующие общедоступные статические IP-адреса, называемые зарезервированными IP-адресами, из классической модели в модель современного Azure Resource Manager.  Перенесенный общедоступный IP-адрес будет иметь тип SKU "базовый".


---

# <a name="reserved-to-basic---powershell"></a>[**Зарезервировано в Basic-PowerShell**](#tab/option-migrate-powershell)

В следующем примере предполагается, что предыдущее создание классической службы Azure зарезервированный IP-адрес **myReservedIP** в **myResourceGroup**. Еще одним необходимым условием для миграции является проверка того, зарегистрирована Azure Resource Manager подписка на миграцию. Это подробно описано в шагах 3 и 4 [этой страницы](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-ps).

Чтобы перенести зарезервированный IP-адрес, выполните приведенные ниже команды с помощью PowerShell.  Примечание. Если IP-адрес не связан ни с одной службой (ниже находится служба с именем **myService**), этот шаг можно пропустить.

```azurepowershell-interactive
## Variables for the command ##
$name = 'myReservedIP'

## This section is only needed if the Reserved IP is not already disassociated from any Cloud Services ##
$serviceName = 'myService'
Remove-AzureReservedIPAssociation -ReservedIPName $name -ServiceName $service

$validate = Move-AzureReservedIP -ReservedIPName $name -Validate
$validate.ValidationMessages
```
Предыдущая команда отображает все предупреждения и ошибки, которые блокируют миграцию. Если проверка прошла успешно, можно выполнить следующие шаги для подготовки и фиксации миграции.
```azurepowershell-interactive
Move-AzureReservedIP -ReservedIPName $name -Prepare
Move-AzureReservedIP -ReservedIPName $name -Commit
```
Новая группа ресурсов в Azure Resource Manager создается с использованием имени перенесенного зарезервированный IP-адрес (в приведенном выше примере это будет группа ресурсов **myReservedIP — перенесена**).

# <a name="reserved-to-basic---cli"></a>[**Зарезервировано для Basic-CLI**](#tab/option-migrate-cli)

В следующем примере предполагается, что предыдущее создание классической службы Azure зарезервированный IP-адрес **myReservedIP** в **myResourceGroup**. Еще одним необходимым условием для миграции является проверка того, зарегистрирована Azure Resource Manager подписка на миграцию. Это подробно описано в шагах 3 и 4 [этой страницы](https://docs.microsoft.com/azure/virtual-machines/linux/migration-classic-resource-manager-cli).

Чтобы перенести зарезервированный IP-адрес, выполните приведенные ниже команды с помощью Azure CLI.  Примечание. Если IP-адрес не связан ни с одной службой (ниже находится служба с именем **myService** и развертывание **развертывании mydeployment**), этот шаг можно пропустить.

```azurecli-interactive
## Variables for the command ##
$name = 'myReservedIP'

## This section is only needed if the Reserved IP is not already disassociated from any Cloud Services ##
$service = 'myService'
$deployment = 'myDeployment'
azure network reserved-ip disassociate $name $service $deployment

azure network reserved-ip validate-migration $name
```
Предыдущая команда отображает все предупреждения и ошибки, которые блокируют миграцию. Если проверка прошла успешно, можно выполнить следующие шаги для подготовки и фиксации миграции.
```azurecli-interactive
azure network reserved-ip prepare-migration $name
azure network reserved-ip commit-migration $name
```
Новая группа ресурсов в Azure Resource Manager создается с использованием имени перенесенного зарезервированный IP-адрес (в приведенном выше примере это будет группа ресурсов **myReservedIP — перенесена**).

---

## <a name="limitations"></a>Ограничения

* В настоящее время эта возможность недоступна в следующих регионах:<br>
US Gov (Вирджиния)<br>
восточный регион US DoD<br>
центральный регион US DoD<br>
Восточный Китай<br>
Восточный Китай 2<br>
Северный Китай<br>
Северный Китай 2

* Чтобы обновить базовый общедоступный IP-адрес, он не может быть связан ни с одним ресурсом Azure.  Дополнительные сведения о том, как разорвать связь с общедоступными IP-адресами, см. на [этой странице](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#view-modify-settings-for-or-delete-a-public-ip-address) .  Аналогично, чтобы перенести зарезервированный IP-адрес, он не может быть связан с какой-либо облачной службой.  Дополнительные сведения о том, как отсоединить зарезервированные IP-адреса, см. на [этой странице](https://docs.microsoft.com/azure/virtual-network/remove-public-ip-address-vm) .  
* Общедоступные IP-адреса, обновленные с уровня "базовый" до SKU "Стандартный", не будут иметь [зон доступности](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones) и поэтому не могут быть связаны с ресурсом Azure, который является избыточным в виде зоны или зональные.  Обратите внимание, что это относится только к регионам, предлагающим зоны доступности.

## <a name="next-steps"></a>Next Steps

- Узнайте больше об [общедоступных IP-адресах](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) в Azure, включая разницу между типами SKU, а также [параметрами общедоступного IP-адреса](virtual-network-public-ip-address.md#create-a-public-ip-address).
- Узнайте, как [Обновить общедоступные подсистемы балансировки нагрузки Azure с уровня "базовый" на "Стандартный](https://docs.microsoft.com/azure/load-balancer/upgrade-basic-standard)".
- Ознакомьтесь с [классическими зарезервированными IP](https://docs.microsoft.com/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip) -адресами Azure и [переносом классических ресурсов в Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

---
title: Скрипт PowerShell для получения ключа и строки подключения для учетной записи Azure Cosmos DB
description: Пример скрипта Azure PowerShell. Операции с ключом учетной записи и строкой подключения для учетной записи Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: a6d15517e10a36a4b951d8b5afe47697086f11a2
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93099523"
---
# <a name="connection-string-and-account-key-operations-for-an-azure-cosmos-db-account-using-powershell"></a>Операции со строкой подключения и ключом для учетной записи Azure Cosmos с использованием PowerShell
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Пример скрипта

> [!NOTE]
> В этом примере демонстрируется использование учетной записи API SQL. Чтобы использовать этот пример с другими API, соответствующие свойства необходимо скопировать в скрипт для такого API.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-keys-connection-strings.ps1 "Connection strings and account keys for Azure Cosmos account")]

## <a name="clean-up-deployment"></a>Очистка развертывания

После выполнения примера сценария можно удалить группу ресурсов и все связанные с ней ресурсы, выполнив следующую команду.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Get-Help | Примечания |
|---|---|
|**Azure Cosmos DB**| |
| [Get-AzCosmosDBAccountKey](/powershell/module/az.cosmosdb/get-azcosmosdbaccountkey) | Возвращает строку подключения или ключ (только для чтения и записи или только для чтения) учетной записи Cosmos DB. |
| [New-AzCosmosDBAccountKey](/powershell/module/az.cosmosdb/new-azcosmosdbaccountkey) | Повторно создает определенный ключ для учетной записи Cosmos DB. |
|**Группы ресурсов Azure**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Удаляет группу ресурсов со всеми вложенными ресурсами. |
|||

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о Azure PowerShell см. в [документации по Azure PowerShell](/powershell/).
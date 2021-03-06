---
title: Включение ведения журнала Azure Key Vault
description: Как включить ведение журнала для Azure Key Vault, которое сохраняет сведения в предоставленной учетной записи хранения Azure.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 10/01/2020
ms.author: mbaldwin
ms.openlocfilehash: 5e0007f3b0dad8a68e9d81cebbe9fe24b5a7db3c
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/03/2020
ms.locfileid: "93285645"
---
# <a name="how-to-enable-key-vault-logging"></a>Включение ведения журнала Key Vault

Создав одно или несколько хранилищ ключей вы, вероятно, захотите отслеживать, кто, как и когда осуществлял доступ к этим хранилищам. Дополнительные сведения о функции см. в разделе [ведение журнала Key Vault](logging.md).

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим учебником необходимо наличие следующих компонентов.

* Существующее хранилище ключей, которое вы используете.  
* Интерфейс командной строки Azure или Azure PowerShell.
* Достаточный объем хранилища в Azure для журналов хранилищ ключей.

Если вы решили установить и использовать CLI локально, потребуется Azure CLI версии 2.0.4 или более поздней. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI](/cli/azure/install-azure-cli). Чтобы войти в Azure с помощью CLI, введите следующее:

```azurecli-interactive
az login
```

Если вы решили установить и использовать PowerShell локально, потребуется Azure PowerShell модуль версии 1.0.0 или более поздней. Чтобы узнать версию, введите `$PSVersionTable.PSVersion`. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-az-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzAccount`, чтобы создать подключение к Azure.

```powershell-interactive
Connect-AzAccount
```

## <a name="connect-to-your-key-vault-subscription"></a>Подключение к подписке Key Vault

Первым шагом при настройке ведения журнала ключей является подключение к подписке, содержащей хранилище ключей. Это особенно важно, если у вас есть несколько подписок, связанных с вашей учетной записью.

С Azure CLI можно просмотреть все подписки с помощью команды [AZ Account List](/cli/azure/account?view=azure-cli-latest#az_account_list) , а затем подключиться к ней, используя команду [AZ Account Set](/cli/azure/account?view=azure-cli-latest#az_account_set):

```azurecli-interactive
az account list

az account set --subscription "<subscriptionID>"
```

С Azure PowerShell можно сначала вывести список подписок с помощью командлета [Get-азсубскриптион](/powershell/module/az.accounts/get-azsubscription?view=azps-4.7.0) , а затем подключиться к нему с помощью командлета [Set-азконтекст](/powershell/module/az.accounts/set-azcontext?view=azps-4.7.0) : 

```powershell-interactive
Get-AzSubscription

Set-AzContext -SubscriptionId "<subscriptionID>"
```

## <a name="create-a-storage-account-for-your-logs"></a>Создание учетной записи хранения для журналов

Хотя для журналов можно использовать существующую учетную запись хранения, мы создадим новую учетную запись хранения, выделенную для Key Vault журналов. 

Чтобы упростить управление, мы также будем использовать ту же группу ресурсов, которая содержит хранилище ключей. В кратком руководстве по [Azure CLI](quick-create-cli.md) и [Azure PowerShell](quick-create-powershell.md)эта группа ресурсов называется **myResourceGroup** , а расположение — *eastus*. Замените эти значения собственными, как применимые. 

Кроме того, необходимо указать имя учетной записи хранения. Имена учетных записей хранения должны быть уникальными длиной от 3 до 24 символов и использовать только цифры и буквы в нижнем регистре.  Наконец, мы создадим учетную запись хранения SKU "Standard_LRS".

В Azure CLI используйте команду [AZ Storage Account Create](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create) .

```azurecli-interactive
az storage account create --name "<your-unique-storage-account-name>" -g "myResourceGroup" --sku "Standard_LRS"
```

С Azure PowerShell используйте командлет [New-азсторажеаккаунт](/powershell/module/az.storage/new-azstorageaccount?view=azps-4.7.0) . Необходимо указать расположение, соответствующее группе ресурсов.

```powershell
 New-AzStorageAccount -ResourceGroupName myResourceGroup -Name "<your-unique-storage-account-name>" -Type "Standard_LRS" -Location "eastus"
```

В любом случае обратите внимание на идентификатор учетной записи хранения. Операция Azure CLI возвращает идентификатор "ID" в выходных данных. Чтобы получить "ID" с Azure PowerShell, используйте [Get-азсторажеаккаунт](/powershell/module/az.storage/get-azstorageaccount?view=azps-4.7.0) и присвоить выходные данные переменной $SA. Затем можно просмотреть учетную запись хранения с $sa. ID. ($Sa. Свойство Context также будет использоваться далее в этой статье.)

```powershell-interactive
$sa = Get-AzStorageAccount -Name "<your-unique-storage-account-name>" -ResourceGroup "myResourceGroup"
$sa.id
```

Идентификатор учетной записи хранения будет иметь формат "/Subscriptions/<a-Subscription-ID>/Ресаурцеграупс/миресаурцеграуп/провидерс/Микрософт.стораже/сторажеаккаунтс/<a-Unique-Storage-Account-Name>".

> [!NOTE]
> Если вы решили использовать существующую учетную запись хранения, она должна использовать ту же подписку, что и хранилище ключей, и должна использовать модель развертывания Azure Resource Manager, а не классическая модель развертывания.

## <a name="obtain-your-key-vault-resource-id"></a>Получение идентификатора ресурса хранилища ключей

В кратком руководстве по интерфейсу [командной строки](quick-create-cli.md) и [PowerShell](quick-create-powershell.md)вы создали ключ с уникальным именем.  Используйте это имя еще раз в следующих шагах.  Если вы не можете вспомнить имя хранилища ключей, можно воспользоваться командой Azure CLI [AZ keyvault List](/cli/azure/keyvault?view=azure-cli-latest#az_keyvault_list) или командлетом Azure PowerShell [Get-азкэйваулт](/powershell/module/az.keyvault/get-azkeyvault?view=azps-4.7.0) для их перечисления.

Используйте имя хранилища ключей, чтобы найти его идентификатор ресурса.  С Azure CLI используйте команду [AZ keyvault показывать](/cli/azure/keyvault?view=azure-cli-latest#az_keyvault_show) .

```azurecli-interactive
az keyvault show --name "<your-unique-keyvault-name>"
```

С Azure PowerShell используйте командлет [Get-азкэйваулт](/powershell/module/az.keyvault/get-azkeyvault?view=azps-4.7.0) .

```powershell-interactive
Get-AzKeyVault -VaultName "<your-unique-keyvault-name>"
```

Идентификатор ресурса для хранилища ключей будет иметь формат "/Subscriptions/<a-Subscription-ID>/Ресаурцеграупс/миресаурцеграуп/провидерс/Микрософт.кэйваулт/ваултс/<a-Unique-KeyVault-Name>". Обратите внимание на следующее действие.

## <a name="enable-logging-using-azure-powershell"></a>Включение ведения журнала с помощью Azure PowerShell

Чтобы включить ведение журнала для Key Vault, мы будем использовать команду Azure CLI [AZ Monitor диагностики-Settings Create](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest) или командлет [Set-АЗДИАГНОСТИКСЕТТИНГ](/powershell/module/az.monitor/set-azdiagnosticsetting?view=azps-4.7.0) вместе с идентификатором учетной записи хранения и идентификатором ресурса хранилища ключей.

```azurecli-interactive
az monitor diagnostic-settings create --storage-account "<storage-account-id>" --resource "<key-vault-resource-id>" --name "Key vault logs" --logs '[{"category": "AuditEvent","enabled": true}]' --metrics '[{"category": "AllMetrics","enabled": true}]'
```

С Azure PowerShell мы будем использовать командлет [Set-аздиагностиксеттинг](/powershell/module/az.monitor/set-azdiagnosticsetting?view=azps-4.7.0) с флагом **-Enabled** , установленным в **$true** , а категория имеет значение `AuditEvent` (единственная категория для Key Vault ведения журнала):

```powershell-interactive
Set-AzDiagnosticSetting -ResourceId "<key-vault-resource-id>" -StorageAccountId $sa.id -Enabled $true -Category "AuditEvent"
```

При необходимости можно задать политику хранения для журналов, чтобы старые журналы автоматически удалялись по истечении указанного промежутка времени. Например, можно задать политику хранения, которая автоматически удаляет журналы старше 90 дней.

<!-- With the Azure CLI, use the [az monitor diagnostic-settings update](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az_monitor_diagnostic_settings_update) command. 

```azurecli-interactive
az monitor diagnostic-settings update 
```
-->

С Azure PowerShell используйте командлет [Set-аздиагностиксеттинг](/powershell/module/az.monitor/set-azdiagnosticsetting?view=azps-4.7.0) . 

```powershell-interactive
Set-AzDiagnosticSetting "<key-vault-resource-id>" -StorageAccountId $sa.id -Enabled $true -Category AuditEvent -RetentionEnabled $true -RetentionInDays 90
```

Журнал:

* все прошедшие проверку подлинности запросы REST API, включая запросы, ставшие неудачными из-за определенных разрешений на доступ, системных ошибок или неправильных запросов;
* операции с хранилищем ключей, включая создание, удаление и настройку политик доступа к нему, а также обновление таких его атрибутов, как теги;
* операции с ключами и секретами в хранилище ключей, в том числе:
  * создание, изменение или удаление ключей или секретов;
  * подписывание, проверка, шифрование, расшифровка, упаковка и распаковка ключей, получение секретов и вывод списка ключей и секретов (и их версий);
* непроверенные запросы, которые приводят к появлению ответа 401 (например, запросы без токена носителя, с недействительным токеном, а также запросы неправильного формата или просроченные запросы).  
* Изменились события уведомлений Сетки событий, связанные с приближением истечения срока действия, истечением срока действия и политикой доступа к хранилищу (событие новой версии не регистрируются). События заносятся в журнал независимо от наличия подписки на события, созданной в хранилище ключей. См. статью [Использование Azure Key Vault в качестве источника Сетки событий](../../event-grid/event-schema-key-vault.md).

## <a name="access-your-logs"></a>Доступ к журналам

Журналы Key Vault хранятся в контейнере "Insights-Logs-auditevent" в указанной учетной записи хранения. Чтобы просмотреть эти журналы, скачайте большие двоичные объекты.

Сначала перечислите все большие двоичные объекты в контейнере.  В Azure CLI используйте команду [AZ Storage BLOB List](/cli/azure/storage/blob?view=azure-cli-latest#az_storage_blob_list) .

```azurecli-interactive
az storage blob list --account-name "<your-unique-storage-account-name>" --container-name "insights-logs-auditevent"
```

Используя Azure PowerShell, используйте список [Get-азсторажеблоб](/powershell/module/az.storage/get-azstorageblob?view=azps-4.7.0) , содержащий все большие двоичные объекты в этом контейнере, и введите:

```powershell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

Как видно из выходных данных команды Azure CLI или командлета Azure PowerShell, имена больших двоичных объектов имеют формат `resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json` . Для значений даты и времени используется время в формате UTC.

Поскольку одну учетную запись можно использовать для сбора журналов нескольких ресурсов, для просмотра и скачивания нужных BLOB-объектов желательно указывать полный идентификатор ресурса в имени BLOB-объекта. Но сначала мы рассмотрим загрузку всех BLOB-объектов.

В Azure CLI используйте команду [AZ Storage BLOB download](/cli/azure/storage/blob?view=azure-cli-latest#az_storage_blob_download) , передайте ей имена больших двоичных объектов и путь к файлу, в котором нужно сохранить результаты.

```azurecli-interactive
az storage blob download --container-name "insights-logs-auditevent" --file <path-to-file> --name "<blob-name>" --account-name "<your-unique-storage-account-name>"
```

Используя Azure PowerShell, используйте командлет [gt-азсторажеблобс](/powershell/module/az.storage/get-azstorageblob?view=azps-4.7.0) , чтобы получить список больших двоичных объектов, затем передайте его в командлет [Get-азсторажеблобконтент](/powershell/module/az.storage/get-azstorageblobcontent?view=azps-4.7.0) , чтобы скачать журналы по выбранному пути.

```powershell-interactive
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context | Get-AzStorageBlobContent -Destination "<path-to-file>"
```

При запуске второго командлета в PowerShell **/** разделитель в именах больших двоичных объектов создает полную структуру папок в папке назначения. Эта структура будет использоваться для скачивания и хранения больших двоичных объектов в виде файлов.

Для выборочной загрузки BLOB-объектов используйте подстановочные знаки. Пример:

* Если у вас есть несколько хранилищ ключей, но вы хотите загрузить журналы только для одного хранилища с именем CONTOSOKEYVAULT3.

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
  ```

* Если у вас есть несколько групп ресурсов, но вы хотите загрузить журналы только для одной из них, используйте `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
  ```

* Если вы хотите скачать все журналы за январь 2019 г., используйте `-Blob '*/year=2019/m=01/*'`:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'
  ```

Теперь можно переходить к анализу содержимого журналов. Но прежде чем мы перейдем к этому, вам нужно изучить еще две команды:

Дополнительные сведения о том, как читать журналы, см [. в разделе Key Vault Logging: интерпретация журналов Key Vault](logging.md#interpret-your-key-vault-logs)

## <a name="use-azure-monitor-logs"></a>Использование журналов Azure Monitor

Решение Key Vault в журналах Azure Monitor позволяет просматривать журналы `AuditEvent` для Key Vault. В журналах Azure Monitor запросы по журналам используются для анализа данных и получения необходимых сведений.

Дополнительные сведения, включая инструкции по настройке, см. в разделе [Мониторинг службы Key Vault с помощью Azure Monitor для Key Vault (предварительная версия)](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="next-steps"></a>Дальнейшие действия

- Основные сведения, в том числе анализ журналов Key Vault, см. в разделе [ведение журнала Key Vault](logging.md)
- Руководство по использованию Azure Key Vault в веб-приложении .NET см. в [этой статье](tutorial-net-create-vault-azure-web-app.md).
- Справочные материалы по программированию см. в статье [Руководство разработчика хранилища ключей Azure](developers-guide.md).
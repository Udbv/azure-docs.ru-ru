---
title: Создание моментального снимка виртуального жесткого диска с помощью Azure CLI
description: Узнайте, как создать копию виртуального жесткого диска (VHD) в Azure в качестве резервной копии или для устранения неполадок.
author: roygara
manager: twooley
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 07/11/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 6374108247b9bfb950c42495b13b501ded8a02d2
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96015967"
---
# <a name="create-a-snapshot-using-the-portal-or-azure-cli"></a>Создание моментального снимка с помощью портала или Azure CLI

Создайте моментальный снимок операционной системы или диска данных для резервного копирования или устранения неполадок с виртуальной машиной. Моментальный снимок — это полная копия виртуального жесткого диска, доступная только для чтения. 

## <a name="use-azure-cli"></a>Использование Azure CLI 

В следующем примере необходимо использовать [Cloud Shell](https://shell.azure.com/bash) или установить Azure CLI.

Ниже показано, как создать моментальный снимок, выполнив команду **az snapshot create** с параметром **--source-disk**. В приведенном ниже примере предполагается, что в группе ресурсов *myResourceGroup* существует виртуальная машина *myVM*.

Получите идентификатор диска с помощью команды [az vm show](/cli/azure/vm#az-vm-show).

```azurecli-interactive
osDiskId=$(az vm show \
   -g myResourceGroup \
   -n myVM \
   --query "storageProfile.osDisk.managedDisk.id" \
   -o tsv)
```

Создайте моментальный снимок с именем *osDisk-backup* с помощью команды [az snapshot create](/cli/azure/snapshot#az-snapshot-create).

```azurecli-interactive
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDisk-backup
```

> [!NOTE]
> Перед сохранением моментального снимка в хранилище, отказоустойчивом в пределах зоны, моментальный снимок необходимо создать в регионе, который поддерживает [зоны доступности](../../availability-zones/az-overview.md) и в котором включен параметр **--sku Standard_ZRS**.

Список моментальных снимков можно просмотреть с помощью команды [az snapshot list](/cli/azure/snapshot#az-snapshot-list).

```azurecli-interactive
az snapshot list \
   -g myResourceGroup \
   - table
```

## <a name="use-azure-portal"></a>Использование портала Azure 

1. Войдите на [портал Azure](https://portal.azure.com).
2. В левом верхнем углу щелкните **Создать ресурс** и выполните поиск фразы **моментальный снимок**. В результатах поиска выберите **Моментальный снимок**.
3. В колонке **Моментальный снимок** щелкните **Создать**.
4. Заполните поле **Имя** для моментального снимка.
5. Выберите существующую группу ресурсов или укажите имя, чтобы создать новую. 
7. В поле **Исходный диск** выберите управляемый диск, моментальный снимок которого необходимо создать.
8. Выберите **тип учетной записи**, которая будет использоваться для хранения моментального снимка. Используйте **диск HDD ценовой категории "Стандартный"**, если вам не нужно хранить моментальный снимок на высокопроизводительном диске SSD.
9. Нажмите кнопку **Создать**.


## <a name="next-steps"></a>Следующие шаги

 Создайте виртуальную машину из моментального снимка, создав из него управляемый диск, а затем подключив этот диск как диск ОС. См. дополнительные сведения о скрипте для [создания виртуальной машины из моментального снимка](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json).


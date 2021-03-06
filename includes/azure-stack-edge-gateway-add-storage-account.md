---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 08/30/2020
ms.author: alkohli
ms.openlocfilehash: 0b6a6cbf51ef2ff1f1ef53b53a2b84c7a4f9510d
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "96185556"
---
1. На [портале Azure](https://portal.azure.com/) выберите ресурс Azure Stack Edge и перейдите в раздел **Обзор**. Устройство должно быть подключено к сети.

2. На панели команд для устройства выберите **+ Добавить учетную запись хранения**. 

   ![добавление учетной записи хранения;](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-1.png)

3. На панели **Добавить учетную запись хранения Edge** укажите следующие параметры:

    а. Имя учетной записи хранения Edge (уникальное в пределах устройства). Имена учетных записей хранения могут содержать только цифры и буквы в нижнем регистре. Использовать специальные знаки не допускается. Имя учетной записи хранения должно быть уникальным в пределах устройства (не нескольких устройств).

    b. Необязательное описание данных, хранящихся в учетной записи хранения.  
    
    c. По умолчанию учетная запись хранения Edge сопоставляется с учетной записью хранения Azure в облаке, и данные из учетной записи хранения автоматически отправляются в облако. Укажите учетную запись хранения Azure, с которой будет сопоставлена учетная запись хранения Edge.  

    г. Создайте новый контейнер или выберите существующий в учетной записи хранения Azure. Все данные на устройстве, записываемые в учетную запись хранения Edge, автоматически передаются в выбранный контейнер хранилища в сопоставленной учетной записи хранения Azure.

    <!--![Add a storage account](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-2.png)-->

    Д. Указав все параметры учетной записи хранения, выберите **Добавить**, чтобы создать учетную запись хранения Edge. Когда учетная запись хранения Edge будет создана, вы получите уведомление. Новая учетная запись хранения Edge появится в списке учетных записей хранения на портале Azure. 

    
4. Если выбрать эту новую учетную запись хранения и перейти к разделу **Ключи доступа**, вы найдете конечную точку службы BLOB-объектов и соответствующее имя учетной записи хранения. Скопируйте эти значения, так как они и ключи доступа понадобятся при подключении к учетной записи хранения Edge.

    ![Добавление учетной записи хранения (2)](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-4.png)

    Чтобы получить ключи доступа, [подключитесь к локальным API устройства с помощью Azure Resource Manager](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md). 

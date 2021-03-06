---
title: Подрезать видео при кодировании с помощью служб мультимедиа Azure (ЧАСТное)
description: В этом разделе описывается, как подрезать видео при кодировании со службами мультимедиа Azure с помощью функции RESTFUL.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: how-to
ms.date: 06/10/2019
ms.author: inhenkel
ms.openlocfilehash: 2a78e8c8c204e5cedeb8bdd2a25cff40a3e5cc44
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89296555"
---
# <a name="subclip-a-video-when-encoding-with-media-services---rest"></a>Подрезать видео при кодировании со службами мультимедиа — остальное

Вы можете обрезать или подрезать видео при его кодировании с помощью [задания](/rest/api/media/jobs). Эта функция работает с любыми [преобразованиями](/rest/api/media/transforms), созданными с помощью предустановок [BuiltInStandardEncoderPreset](/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) или [StandardEncoderPreset](/rest/api/media/transforms/createorupdate#standardencoderpreset). 

Пример RESTFUL в этом разделе создает задание, которое обрезает видео по мере отправки задания кодирования. 

## <a name="prerequisites"></a>Предварительные требования

Чтобы выполнить действия, описанные в этом разделе, необходимо сделать следующее:

- [Создайте учетную запись служб мультимедиа Azure](./create-account-howto.md).
- [Настройте Postman для вызовов REST API Служб мультимедиа Azure](media-rest-apis-with-postman.md).
    
    Не забудьте выполнить последний шаг, указанный в разделе [Получение токена Azure AD](media-rest-apis-with-postman.md#get-azure-ad-token). 
- Создание преобразования и выходных ресурсов. Вы можете увидеть, как создать преобразование и выходные ресурсы в [кодировке удаленного файла на основе URL-адреса и потока руководства по](stream-files-tutorial-with-rest.md) работе с видео.
- Ознакомьтесь с разделом [концепция кодирования](encoding-concept.md) .

## <a name="create-a-subclipping-job"></a>Создание задания подрезки

1. В скачанной коллекции после этого выберите **преобразования и задания**  ->  **создать задание с вложенной обрезки**.
    
    Запрос на **Размещение** выглядит следующим образом:
    
    ```
    https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
    ```
1. Обновите значение переменной среды "Трансформнаме", указав имя преобразования. 
1. Перейдите на вкладку **текст** и измените "мйоутпутассет" на имя выходного ресурса.

    ```
    {
      "properties": {
        "description": "A Job with transform cb9599fb-03b3-40eb-a2ff-7ea909f53735 and single clip.",
       
        "input": {
          "@odata.type": "#Microsoft.Media.JobInputHttp",
          "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
          "files": [
                "Ignite-short.mp4"
            ],
          "start": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT10S"
          },
          "end": {
            "@odata.type": "#Microsoft.Media.AbsoluteClipTime",
            "time": "PT40S"
          }
        },
      
        "outputs": [
          {
            "@odata.type": "#Microsoft.Media.JobOutputAsset",
            "assetName": "myOutputAsset"
          }
        ],
        "priority": "Normal"
      }
    }
    ```
1. Нажмите кнопку **Отправить**.

    Вы увидите **ответ** с информацией о созданном и отправленном задании, а также о состоянии задания. 

## <a name="next-steps"></a>Дальнейшие шаги

[Кодирование с помощью пользовательского преобразования](custom-preset-rest-howto.md) 

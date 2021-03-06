---
title: Защита данных в Azure Stream Analytics
description: В этой статье объясняется, как зашифровать личные данные, используемые заданием Azure Stream Analytics.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 09/23/2020
ms.openlocfilehash: 72566987068729efef4310ce145c30584c4895b0
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011410"
---
# <a name="data-protection-in-azure-stream-analytics"></a>Защита данных в Azure Stream Analytics 

Azure Stream Analytics — это полностью управляемая платформа как услуга, которая позволяет создавать конвейеры аналитики в режиме реального времени. Вся тяжелая работа, такая как подготовка кластера, масштабирование узлов в соответствии с использованием и управление внутренними контрольными точками, осуществляется за кулисами.

## <a name="private-data-assets-that-are-stored"></a>Хранящиеся закрытые ресурсы данных

Azure Stream Analytics сохраняет следующие метаданные и данные для запуска: 

* Определение запроса и связанная с ним конфигурация  

* Определяемые пользователем функции или агрегаты  

* Контрольные точки, необходимые для среды выполнения Stream Analytics

* Моментальные снимки ссылочных данных 

* Сведения о подключении ресурсов, используемых заданием Stream Analytics

Чтобы помочь вам удовлетворить обязательства по обеспечению соответствия требованиям в любой регулируемой отрасли или среде, вы можете ознакомиться с дополнительными предложениями по обеспечению [соответствия требованиям корпорации Майкрософт](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942). 

## <a name="in-region-data-residency"></a>Местонахождение данных In-Region
Azure Stream Analytics хранит данные клиента и другие метаданные, описанные выше. По умолчанию данные клиента хранятся по Azure Stream Analytics в одном регионе, поэтому эта служба автоматически удовлетворяет требованиям местонахождение данных региона, включая те, которые указаны в [центре управления безопасностью](https://azuredatacentermap.azurewebsites.net/).
Кроме того, вы можете хранить все ресурсы данных (данные клиента и другие метаданные), связанные с заданием Stream Analytics, в одном регионе, шифруя их в учетной записи хранения по своему усмотрению.

## <a name="encrypt-your-data"></a>Шифрование данных

Stream Analytics автоматически применяет оптимальные стандарты шифрования в рамках инфраструктуры для шифрования и защиты данных. Вы можете просто доверять Stream Analytics для безопасного хранения всех данных, чтобы не беспокоиться об управлении инфраструктурой.

Если вы хотите использовать ключи, управляемые клиентом (CMK), для шифрования данных, можно использовать собственную учетную запись хранения (общее назначение версии v1 или v2) для хранения частных ресурсов данных, необходимых для среды выполнения Stream Analytics. Учетную запись хранения можно зашифровать по мере необходимости. Ни один из личных ресурсов данных не будет постоянно храниться в инфраструктуре Stream Analytics. 

Этот параметр должен быть настроен во время создания задания Stream Analytics и не может быть изменен в течение всего жизненного цикла задания. Не рекомендуется изменять или удалять хранилище, используемое Stream Analytics. При удалении учетной записи хранения все закрытые ресурсы данных будут безвозвратно удалены, что приведет к сбою задания. 

Обновление или вращение ключей в учетной записи хранения невозможно с помощью портала Stream Analytics. Ключи можно обновить с помощью интерфейсов API.


### <a name="configure-storage-account-for-private-data"></a>Настройка учетной записи хранения для частных данных 


Зашифруйте учетную запись хранения, чтобы защитить все данные и явно выбрать расположение личных данных. 

Чтобы помочь вам удовлетворить обязательства по обеспечению соответствия требованиям в любой регулируемой отрасли или среде, вы можете ознакомиться с дополнительными предложениями по обеспечению [соответствия требованиям корпорации Майкрософт](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942). 



Выполните следующие действия, чтобы настроить учетную запись хранения для закрытых ресурсов данных. Эта конфигурация выполняется из задания Stream Analytics, а не из учетной записи хранения.

1. Войдите на [портал Azure](https://portal.azure.com/).

1. Щелкните **Создать ресурс** в верхнем левом углу окна портала Azure. 

1. Выберите **Analytics**   >  **Stream Analytics задание**   из списка результатов. 

1. Заполните страницу задания Stream Analytics, указав необходимые сведения, такие как имя, регион и масштаб. 

1. Установите флажок *защитить все закрытые ресурсы данных, необходимые для этого задания в учетной записи хранения*.

1. Выберите учетную запись хранения из подписки. Обратите внимание, что этот параметр нельзя изменить в течение всего жизненного цикла задания. 

   ![Параметры учетной записи хранения личных данных](./media/data-protection/storage-account-create.png)

## <a name="private-data-assets-that-are-stored-by-stream-analytics"></a>Закрытые ресурсы данных, которые хранятся Stream Analytics

Все закрытые данные, которые должны сохраняться Stream Analytics, хранятся в учетной записи хранения. Ниже приведены примеры закрытых ресурсов данных. 

* Созданные запросы и связанные с ними конфигурации  

* Определяемые пользователем функции 

* Контрольные точки, необходимые для среды выполнения Stream Analytics

* Моментальные снимки ссылочных данных 

Также сохраняются сведения о подключении ресурсов, которые используются в Stream Analyticsном задании. Зашифруйте учетную запись хранения, чтобы защитить все ваши данные. 

Чтобы помочь вам удовлетворить обязательства по обеспечению соответствия требованиям в любой регулируемой отрасли или среде, вы можете ознакомиться с дополнительными предложениями по обеспечению [соответствия требованиям корпорации Майкрософт](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942). 

## <a name="enables-data-residency"></a>Включает местонахождение данных 
Вы можете использовать эту функцию, чтобы применить любые требования к местонахождение данных, предоставив учетную запись хранения соответствующим образом.

## <a name="known-issues"></a>Известные проблемы
Существует известная проблема, при которой при использовании управляемого удостоверения для проверки подлинности во всех входных и выходных данных задание, использующее управляемый пользователем ключ, выполняет сбои. 

## <a name="next-steps"></a>Следующие шаги

* [Создайте учетную запись хранения Azure](../storage/common/storage-account-create.md)
* [Общие сведения о входных данных Azure Stream Analytics](stream-analytics-add-inputs.md)
* [Концепции контрольных точек и воспроизведения в Azure Stream Analytics](stream-analytics-concepts-checkpoint-replay.md)
* [Использование эталонных данных для уточняющих запросов в Stream Analytics](stream-analytics-use-reference-data.md)

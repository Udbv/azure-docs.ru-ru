---
title: Настройка сеттингсфромо приложений для извлечения с помощью Azure Pipelines
description: Сведения об использовании Azure Pipelines для извлечения значений ключей в хранилище конфигураций приложений
services: azure-app-configuration
author: drewbatgit
ms.service: azure-app-configuration
ms.topic: how-to
ms.date: 11/17/2020
ms.author: drewbat
ms.openlocfilehash: 15810e65873c685565ccaad6c2dcdc1707713f2c
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "96182654"
---
# <a name="pull-settings-to-app-configuration-with-azure-pipelines"></a>Извлечение параметров в конфигурацию приложения с помощью Azure Pipelines

Задача [настройки приложения Azure](https://marketplace.visualstudio.com/items?itemName=AzureAppConfiguration.azure-app-configuration-task) извлекает значения ключа из хранилища конфигурации приложений и задает их в качестве переменных конвейера Azure, которые могут использоваться последующими задачами. Эта задача дополняет задачу [принудительной настройки приложения Azure](https://marketplace.visualstudio.com/items?itemName=AzureAppConfiguration.azure-app-configuration-task-push) , которая передает значения ключа из файла конфигурации в хранилище конфигурации приложения. Дополнительные сведения см. в статье [Push Settings to App Configuration with Azure pipelines](push-kv-devops-pipeline.md).

## <a name="prerequisites"></a>Предварительные требования

- Подписка Azure — [создайте бесплатную учетную запись](https://azure.microsoft.com/free/).
- Хранилище конфигурации приложений — создайте его бесплатно в [портал Azure](https://portal.azure.com).
- Проект Azure DevOps. [создайте его бесплатно](https://go.microsoft.com/fwlink/?LinkId=2014881)
- Задача настройки приложения Azure. Скачайте бесплатную версию из [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AzureAppConfiguration.azure-app-configuration-task#:~:text=Navigate%20to%20the%20Tasks%20tab,the%20Azure%20App%20Configuration%20instance.).  

## <a name="create-a-service-connection"></a>Создание подключения к службе

Подключение к службе позволяет получить доступ к ресурсам в подписке Azure из проекта Azure DevOps.

1. В Azure DevOps перейдите к проекту, содержащему целевой конвейер, и откройте **Параметры проекта** внизу слева.
1. В разделе **конвейеры** выберите **подключения к службе**.
1. Если у вас нет существующих подключений к службам, нажмите кнопку **создать подключение к службе** в середине экрана. В противном случае щелкните **новое подключение к службе** в правом верхнем углу страницы.
1. Выберите **Azure Resource Manager**.
1. Выберите **субъект-служба (автоматически)**.
1. Заполните подписку и ресурс. Задайте имя для подключения к службе.

Теперь, когда подключение к службе создано, найдите имя субъекта-службы, назначенного ей. На следующем шаге мы добавим новое назначение роли этому субъекту-службе.

1. Последовательно выберите **Параметры проекта**  >  **подключения к службе**.
1. Выберите подключение службы, созданное в предыдущем разделе.
1. Выберите **Управление субъектом-службой**.
1. Обратите внимание на указанное **Отображаемое имя** .

## <a name="add-role-assignment"></a>Добавление назначения роли

Назначьте подходящую роль конфигурации приложения для подключения к службе, которое используется в задаче, чтобы задача могла получить доступ к хранилищу конфигураций приложений.

1. Перейдите к целевому хранилищу конфигураций приложений. Пошаговое руководство по настройке хранилища конфигураций приложений см. в разделе [Создание хранилища конфигурации приложений](/azure/azure-app-configuration/quickstart-dotnet-core-app#create-an-app-configuration-store) в одном из кратких руководств по настройке приложений Azure.
1. Слева выберите **Управление доступом (IAM)**.
1. В верхней части щелкните **+ Добавить** и выберите **добавить назначение ролей**.
1. В разделе **роль** выберите **модуль чтения данных конфигурации приложения**. Эта роль позволяет задаче считывать сведения из хранилища конфигураций приложений. 
1. Выберите субъект-службу, связанный с подключением службы, созданным в предыдущем разделе.

> [!NOTE]
> Для разрешения Azure Key Vault ссылок в конфигурации приложения для подключения службы также должно быть предоставлено разрешение на чтение секретов в упоминаемых хранилищах ключей Azure.
  
## <a name="use-in-builds"></a>Использование в сборках

В этом разделе рассматривается использование задачи настройки приложения Azure в конвейере сборки Azure DevOps.

1. Перейдите на **страницу конвейер сборки, щелкнув конвейер**  >  **конвейеры**. Документацию по конвейеру сборки см.  [в статье Создание первого конвейера](/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=net%2Ctfs-2018-2%2Cbrowser).
      - Если вы создаете новый конвейер сборки, щелкните **создать конвейер** и выберите репозиторий для конвейера. В правой части конвейера выберите пункт **Показывать помощник** и найдите задачу **настройки приложения Azure** .
      - Если вы используете существующий конвейер сборки, выберите **изменить** , чтобы изменить конвейер. На вкладке **задачи** найдите задачу **Настройка приложения Azure** .
1. Настройте необходимые параметры для задачи, чтобы извлечь значения ключа из хранилища конфигурации приложения. Описания параметров доступны в разделе **параметров** ниже и в подсказках рядом с каждым параметром.
      - Задайте параметру **подписки Azure** имя подключения службы, созданного на предыдущем шаге.
      - В качестве **имени конфигурации приложения** задайте имя ресурса для хранилища конфигурации приложения.
      - Оставьте значения по умолчанию для остальных параметров.
1. Сохранение и постановка в очередь сборки. В журнале сборки отобразятся все ошибки, произошедшие во время выполнения задачи.

## <a name="use-in-releases"></a>Использовать в выпусках

В этом разделе рассматривается использование задачи настройки приложений Azure в конвейере выпуска Azure DevOps.

1. Перейдите на страницу конвейера выпуска, выбрав пункт **конвейеры**  >  **выпуски**. Документацию по конвейеру выпуска см. в разделе [конвейеры выпуска](/azure/devops/pipelines/release?view=azure-devops).
1. Выберите существующий конвейер выпуска. Если у вас ее нет, щелкните **Новый конвейер** , чтобы создать новый.
1. Нажмите кнопку **изменить** в правом верхнем углу, чтобы изменить конвейер выпуска.
1. Выберите **этап** для добавления задачи. Дополнительные сведения о стадиях см. в разделе [Добавление этапов, зависимостей, & условий](/azure/devops/pipelines/release/environments?view=azure-devops).
1. Щелкните **+** для параметра "запустить в агенте", а затем добавьте задачу " **Настройка приложения Azure** " на вкладке " **Добавление задач** ".
1. Настройте необходимые параметры в задаче, чтобы извлечь значения ключа из хранилища конфигурации приложения. Описания параметров доступны в разделе **параметров** ниже и в подсказках рядом с каждым параметром.
      - Задайте параметру **подписки Azure** имя подключения службы, созданного на предыдущем шаге.
      - В качестве **имени конфигурации приложения** задайте имя ресурса для хранилища конфигурации приложения.
      - Оставьте значения по умолчанию для остальных параметров.
1. Сохранение и постановка в очередь выпуска. В журнале выпусков отобразятся все ошибки, обнаруженные во время выполнения задачи.

## <a name="parameters"></a>Параметры

В задаче настройки приложения Azure используются следующие параметры:

- **Подписка Azure**. раскрывающийся список, содержащий доступные подключения к службам Azure. Чтобы обновить и обновить список доступных подключений к службам Azure, нажмите кнопку **обновить подписку Azure** справа от текстового поля.
- **Имя конфигурации приложения**: раскрывающийся список, который загружает доступные хранилища конфигураций в выбранной подписке. Чтобы обновить и обновить список доступных хранилищ конфигураций, нажмите кнопку **Обновить имя конфигурации приложения** справа от текстового поля.
- **Фильтр ключей**. фильтр можно использовать для выбора значений ключей, запрашиваемых из конфигурации приложения Azure. Значение * выберет все значения ключа. Дополнительные сведения о см. в разделе [значения ключа запроса](concept-key-value.md#query-key-values).
- **Метка**: указывает, какую метку следует использовать при выборе значений ключа из хранилища конфигурации приложения. Если метка не указана, будут извлечены значения ключа с надписью без метки. Следующие символы не допускаются:, *.
- **Префикс ключа монтажа**: указывает один или несколько префиксов, которые должны быть обрезаны из ключей конфигурации приложения перед их заданием в качестве переменных. Несколько префиксов можно разделить символом новой строки.

## <a name="use-key-values-in-subsequent-tasks"></a>Использование значений ключа в последующих задачах

Значения ключа, полученные из конфигурации приложения, устанавливаются в виде переменных конвейера, которые доступны в виде переменных среды. Ключом переменной среды является ключ ключевого значения, который извлекается из конфигурации приложения после усечения префикса, если он указан.

Например, если в последующей задаче выполняется сценарий PowerShell, он может использовать ключевое значение с ключом "Мибуилдсеттинг" следующим образом:
```powershell
echo "$env:myBuildSetting"
```
Значение будет напечатано на консоли.

## <a name="troubleshooting"></a>Устранение неполадок

При возникновении непредвиденной ошибки можно включить журналы отладки, задав для переменной конвейера `system.debug` значение `true` .

## <a name="faq"></a>ВОПРОСЫ И ОТВЕТЫ

**Разделы справки создать конфигурацию из нескольких ключей и меток?**

Иногда может потребоваться, чтобы конфигурация состояла из нескольких меток, например по умолчанию и разработки. Для реализации этого сценария можно использовать несколько задач конфигурации приложения в одном конвейере. Значения ключа, полученные задачей на более позднем этапе, будут заменять все значения, указанные в предыдущих шагах. В приведенном выше примере задача может использоваться для выбора значений ключа с меткой по умолчанию, а вторая задача может выбирать значения ключа с меткой разработки. Ключи с меткой разработки будут переопределять те же ключи с меткой по умолчанию.

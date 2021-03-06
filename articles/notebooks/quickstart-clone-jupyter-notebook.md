---
title: Клонирование записной книжки Jupyter из GitHub с помощью службы "Записные книжки Azure" (предварительная версия)
description: Быстро клонируйте записную книжку Jupyter из репозитория GitHub и запускайте ее в своей учетной записи Записных книжек Azure.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 267e79e7d4bf108ac3b2c72d64cee5a07ba638be
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "87424483"
---
# <a name="quickstart-clone-a-notebook-in-azure-notebooks-preview"></a>Краткое руководство. Клонирование записной книжки в службе "Записные книжки Azure" (предварительная версия)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

В этом кратком руководстве вы скопируете записную книжку Jupyter, хранящуюся в GitHub, в учетную запись Записных книжек Azure. 

Репозитории GitHub предоставляют хранилище и управление версиями для записных книжек Jupyter. Участники совместной работы хранят локальные копии репозиториев и запускают из них записные книжки. Во время клонирования записной книжки Jupyter из GitHub в учетную запись Записных книжек Azure создается независимая копия записной книжки. Изменения сохраняются только в учетной записи Записных книжек Azure и не влияют на исходный репозиторий GitHub. 

Так как клон Записных книжек Azure находится в облаке, вы можете поделиться им с другими участниками совместной работы, которым не нужно вносить какие-либо локальные копии или даже устанавливать Jupyter на компьютерах. Вы также можете клонировать записную книжку в качестве отправной точки для собственного проекта или для получения файлов данных. 

## <a name="prerequisites"></a>Предварительные требования
Нет.

## <a name="clone-azure-cognitive-services-notebooks"></a>Клонирование записных книжек Azure Cognitive Services

1. Перейдите в службу [Записные книжки Azure](https://notebooks.azure.com) и выполните вход. Дополнительные сведения см. в [кратком руководстве по входу в Записные книжки Azure](quickstart-sign-in-azure-notebooks.md).

1. Вверху на общедоступной странице профиля щелкните **Мои проекты**:

    ![Ссылка "Мои проекты" в верхней части окна браузера](media/quickstarts/my-projects-link.png)

1. На странице **My Projects** (Мои проекты) нажмите кнопку со стрелкой вверх (сочетание клавиш: U; для этой кнопки отображается название **Upload GitHub Repo** (Отправить репозиторий GitHub), когда окно браузера имеет достаточную ширину):

    ![Команда отправки репозитория GitHub на странице My Projects (Мои проекты)](media/quickstarts/upload-github-repo-command.png)

1. В появившемся окне **Upload GitHub Repository** (Отправить репозиторий GitHub) введите или укажите приведенные ниже сведения, а затем нажмите кнопку **Импорт**:

   - **GitHub repository** (Репозиторий GitHub): Microsoft/cognitive-services-notebooks (репозиторий с этим именем клонирует записные книжки Jupyter для Azure Cognitive Services, расположенные по адресу [https://github.com/Microsoft/cognitive-services-notebooks](https://github.com/Microsoft/cognitive-services-notebooks)).
   - **Clone recursively** (Клонировать рекурсивно): флажок снят.
   - **Имя проекта**: Cognitive Services Clone.
   - **Идентификатор проекта**: cognitive-services-clone.
   - **Общедоступный**: флажок снят.

     ![Всплывающее окно отправки репозитория GitHub для сбора сведений о репозитории](media/quickstarts/upload-github-repo-popup.png)

1. Подождите, пока процесс завершится. Клонирование репозитория может занять несколько минут.

1. После завершения клонирования в службе "Записные книжки Azure" откроется новый проект, где вы увидите копии всех файлов.

    :::image type="content" source="media/quickstarts/completed-clone.png" alt-text="Представление завершенного клона." lightbox="media/quickstarts/completed-clone.png":::

## <a name="share-a-notebook"></a>Совместное использование записной книжки

1. Для предоставления общего доступа к копии клонированного проекта используйте элемент управления **Общий доступ** или получите ссылку, получите код HTML или Markdown, содержащий ссылку, либо создайте сообщение электронной почты со ссылкой:

    ![Команда предоставления общего доступа к проекту](media/quickstarts/share-project-command.png)

1. Так как флажок **Общедоступный** был снят при клонировании проекта, клон является частным. Чтобы сделать свою копию общедоступной, выберите **Параметры проекта**, задайте вариант **Public project** (Открытый проект) во всплывающем окне, а затем нажмите кнопку **Сохранить**.

1. Выберите в проекте записную книжку, чтобы запустить ее. Каждая записная книжка в репозитории Azure Cognitive Services, к примеру, имеет свое собственное автономное краткое руководство. На следующем рисунке показан результат использования записной книжки BingImageSearchAPI после добавления ключа подписки API Cognitive Services и изменения термина поиска "puppies" на "bunnies":

    ![Запуск записной книжки Jupyter, клонированной из GitHub](media/quickstarts/clone-notebook-result.png)

1. По завершении работы с записной книжкой выберите **File** (Файл) > **Close and halt** (Закрыть и остановить), чтобы закрыть записную книжку и окно браузера.

1. Для совместного использования отдельной записной книжки в проекте щелкните ее правой кнопкой мыши и выберите **Copy link** (Копировать ссылку) (сочетание клавиш: Y):

    ![Команда контекстного меню, чтобы скопировать ссылку на отдельную записную книжку](media/quickstarts/copy-link-to-individual-notebook.png)

1. Чтобы изменить файлы, не являющиеся записными книжками, щелкните правой кнопкой мыши файл в проекте и выберите **Edit file** (Изменить файл) (сочетание клавиш: I). Действие по умолчанию, **Запустить** (сочетание клавиш: r), позволяет только просматривать содержимое файла без редактирования

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Руководство. Создание и запуск записной книжки Jupyter Notebook для линейной регрессии](tutorial-create-run-jupyter-notebook.md)

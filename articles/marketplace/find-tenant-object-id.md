---
title: Поиск идентификатора клиента, идентификатора объекта и сведений о ассоциации партнера в Azure Marketplace
description: Как найти идентификатор клиента, идентификатор объекта и сведения о ассоциации партнера по ИДЕНТИФИКАТОРу подписки в Azure Marketplace.
ms.service: marketplace
ms.topic: article
author: keferna
ms.author: keferna
ms.date: 10/09/2020
ms.openlocfilehash: db09943085cb7934bca5d7f2dc24ba692613ee19
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2020
ms.locfileid: "92426894"
---
# <a name="find-tenant-id-object-id-and-partner-association-details"></a>Поиск идентификатора клиента, идентификатора объекта и сведений о сопоставлении партнера

В этой статье описывается, как найти идентификатор клиента, идентификатор объекта и сведения о ассоциации партнера вместе с соответствующими идентификаторами подписки.

Если необходимо получить снимки экрана этих элементов в Azure Cloud Shell для использования при отладке, перейдите к разделу [Поиск клиента, объекта и идентификатора партнера для отладки](#find-ids-for-debugging).

>[!Note]
> Права на выполнение этих действий имеют только владелец подписки.

## <a name="find-tenant-id"></a>Поиск идентификатора клиента

1. Перейдите на [портал Azure](https://ms.portal.azure.com/).
2. Выберите **Azure Active Directory**.

    :::image type="content" source="media/tenant-and-object-id/icon-azure-ad.png" alt-text="Значок Azure Active Directory в портал Azure.":::

3. Выберите **Группы**. Идентификатор клиента находится в поле **сведения о клиенте** .

    :::image type="content" source="media/tenant-and-object-id/select-groups-1.png" alt-text="Значок Azure Active Directory в портал Azure.":::

## <a name="find-subscriptions-and-roles"></a>Поиск подписок и ролей

1. Перейдите к портал Azure и выберите **Azure Active Directory** , как указано в шагах 1 и 2 выше.
2. Выберите **Подписки**.

    :::image type="content" source="media/tenant-and-object-id/icon-azure-subscriptions-1.png" alt-text="Значок Azure Active Directory в портал Azure.":::

3. Просмотр подписок и ролей.

    :::image type="content" source="media/tenant-and-object-id/subscriptions-screen-1.png" alt-text="Значок Azure Active Directory в портал Azure.":::

## <a name="find-partner-id"></a>Найти идентификатор партнера

1. Перейдите на страницу подписки, как описано в предыдущем разделе.
2. Выберите подписку.
3. В разделе **выставление счетов**выберите **сведения о партнере**.

    :::image type="content" source="media/tenant-and-object-id/menu-partner-information.png" alt-text="Значок Azure Active Directory в портал Azure.":::

## <a name="find-user-object-id"></a>Поиск пользователя (идентификатор объекта)

1. Войдите на [портал администрирования Office 365](https://portal.office.com/adminportal/home) , используя учетную запись в нужном клиенте с соответствующими правами администратора.
2. В меню слева разверните раздел **центры администрирования** внизу и выберите параметр Azure Active Directory, чтобы запустить консоль администрирования в новом окне браузера.
3. Выберите **Пользователи**.
4. Перейдите к нужному пользователю или найдите его, а затем выберите имя учетной записи, чтобы просмотреть сведения о профиле учетной записи пользователя.
5. Идентификатор объекта находится в разделе удостоверение справа.

    :::image type="content" source="media/tenant-and-object-id/azure-ad-admin-center.png" alt-text="Значок Azure Active Directory в портал Azure.":::

6. Найдите **назначения ролей** , выбрав **Управление доступом (IAM)** в меню слева, а затем **назначения ролей**.

    :::image type="content" source="https://docs.microsoft.com/azure/role-based-access-control/media/role-assignments-portal/role-assignments.png" alt-text="Значок Azure Active Directory в портал Azure.":::

## <a name="find-ids-for-debugging"></a>Поиск идентификаторов для отладки

В этом разделе описывается, как найти клиент, объект и связь с ИДЕНТИФИКАТОРом партнера в целях отладки.

1. Перейдите на [портал Azure](https://ms.portal.azure.com/).
2. Откройте Azure Cloud Shell, щелкнув значок PowerShell в правом верхнем углу.

    :::image type="content" source="media/tenant-and-object-id/icon-azure-cloud-shell-1.png" alt-text="Значок Azure Active Directory в портал Azure.":::

3. Выберите **PowerShell**.

    :::image type="content" source="media/tenant-and-object-id/icon-powershell.png" alt-text="Значок Azure Active Directory в портал Azure.":::

4. Выберите поле **Подписка** , чтобы выбрать тот, с которым связан партнер, а затем **Создайте хранилище**. Это одноразовое действие; Если хранилище уже настроено, перейдите к следующему шагу.

    :::image type="content" source="media/tenant-and-object-id/create-storage-window.png" alt-text="Значок Azure Active Directory в портал Azure.":::

5. Создание (или уже имеющееся) хранилища открывает окно Azure Cloud Shell.

    :::image type="content" source="media/tenant-and-object-id/cloud-shell-window-1.png" alt-text="Значок Azure Active Directory в портал Azure.":::

6. Для сведений о партнерских ассоциациях установите расширение с помощью следующей команды:<br>`az extension add --name managementpartner`.<br>Azure Cloud Shell Обратите внимание, если расширение уже установлено:

    :::image type="content" source="media/tenant-and-object-id/cloud-shell-window-2.png" alt-text="Значок Azure Active Directory в портал Azure.":::

7. Проверьте сведения о партнере с помощью следующей команды:<br>`az managementpartner show --partner-id xxxxxx`<br>Например, `az managementpartner show --partner-id 4760962`.<br>Прикрепить снимок экрана результатов команды, который будет выглядеть примерно так:

    :::image type="content" source="media/tenant-and-object-id/partner-id-sample-screenshot.png" alt-text="Значок Azure Active Directory в портал Azure.":::

>[!NOTE]
>Если для нескольких подписок требуется снимок экрана, используйте эту команду для переключения между подписками:<br>`az account set --subscription "My Demos"`

## <a name="next-steps"></a>Дальнейшие действия

- [Поддерживаемые страны и регионы](sell-from-countries.md)
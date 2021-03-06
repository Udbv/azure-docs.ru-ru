---
title: Учебник. Настройка перебора для автоматической подготовки пользователей с помощью Azure Active Directory | Документация Майкрософт
description: Узнайте, как настроить Azure Active Directory для автоматической инициализации и отзыва учетных записей пользователей для перебора.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/12/2019
ms.author: Zhchia
ms.openlocfilehash: 6ef4558cc0cbbacb372fc4a4c2b52859517a2635
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2020
ms.locfileid: "94353532"
---
# <a name="tutorial-configure-robin-for-automatic-user-provisioning"></a>Учебник. Настройка перебора для автоматической подготовки пользователей

Цель этого учебника — продемонстрировать шаги, которые необходимо выполнить в процессе перебора и Azure Active Directory (Azure AD), чтобы настроить Azure AD для автоматической инициализации и отзыва пользователей и (или) групп для перебора.

> [!NOTE]
> В этом руководстве рассматривается соединитель, созданный на базе службы подготовки пользователей Azure AD. Подробные сведения о том, что делает эта служба, как она работает, и часто задаваемые вопросы см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Сейчас этот соединитель предоставляется в общедоступной предварительной версии. Дополнительные сведения об общих условиях использования продуктов в предварительной версии см. в документе [Дополнительные условия использования Предварительных версий Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Предварительные требования

В сценарии, описанном в этом руководстве, предполагается, что у вас уже имеется:

* клиент Azure AD;
* [Переданный клиент](https://robinpowered.com/pricing/)
* Учетная запись пользователя в перебора с разрешениями администратора.

## <a name="assigning-users-to-robin"></a>Назначение пользователей для перебора

В Azure Active Directory для определения того, какие пользователи должны получать доступ к выбранным приложениям, используется концепция, называемая *назначением*. В контексте автоматической подготовки учетных записей пользователей синхронизируются только пользователи и группы, назначенные приложению в Azure AD.

Перед настройкой и включением автоматической подготовки пользователей следует решить, каким пользователям и (или) группам в Azure AD требуется доступ для перебора. После принятия решения вы можете назначить этих пользователей и (или) группы для перебора, следуя приведенным ниже инструкциям.
* [Назначение корпоративному приложению пользователя или группы](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-robin"></a>Важные советы по назначению пользователей для перебора

* Для проверки конфигурации автоматической подготовки пользователей рекомендуется назначить одного пользователя Azure AD. Дополнительные пользователи и/или группы можно назначить позднее.

* При назначении пользователя для перебора необходимо выбрать в диалоговом окне назначения любую допустимую роль конкретного приложения (если она доступна). Пользователи с ролью **Доступ по умолчанию** исключаются из подготовки.

## <a name="set-up-robin-for-provisioning"></a>Настройка перебора для подготовки

1. Войдите в свою [административную консоль администрирования](https://dashboard.robinpowered.com/login). Перейдите к разделу **Управление интеграцией > > SCIM > управление**.

    ![Консоль администрирования с включенной поддержкой](media/robin-provisioning-tutorial/robin-admin.png)

2.  Создание нового маркера Организации. Если вы потеряли этот маркер, всегда можно создать новый, не затрагивая существующих пользователей.

    ![Добавление SCIM для перебора](media/robin-provisioning-tutorial/robin-token.png)

3.  Скопируйте **маркер проверки подлинности scim**. Это значение будет указано в поле Секретный токен на вкладке Подготовка переданного приложения в портал Azure.



## <a name="add-robin-from-the-gallery"></a>Добавление перебора из коллекции

Перед настройкой перебора для автоматической подготовки пользователей с помощью Azure AD необходимо добавить в коллекцию приложений Azure AD перебора в список управляемых приложений SaaS.

**Чтобы добавить переработку из коллекции приложений Azure AD, выполните следующие действия.**

1. В **[портал Azure](https://portal.azure.com)** на панели навигации слева выберите **Azure Active Directory**.

    ![Кнопка Azure Active Directory](common/select-azuread.png)

2. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

3. Чтобы добавить новое приложение, нажмите кнопку **новое приложение** в верхней части области.

    ![Кнопка "Создать приложение"](common/add-new-app.png)

4. В поле поиска введите "перераспределение", **выберите "** переустановить" на панели результатов и нажмите кнопку " **добавить** **", чтобы** добавить это приложение.

    ![Перераспределение в списке результатов](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-robin"></a>Настройка автоматической подготовки пользователей для перебора 

В этом разделе описано, как настроить службу подготовки Azure AD для создания, обновления и отключения пользователей и (или) групп в зависимости от назначения пользователей и групп в Azure AD.

> [!TIP]
> Вы также можете включить единый вход на основе SAML для перебора, следуя инструкциям, приведенным в [руководстве по использованию единого входа](./robin-tutorial.md). Единый вход можно настроить независимо от автоматической подготовки пользователей, хотя эти две функции дополняют друг друга.

### <a name="to-configure-automatic-user-provisioning-for-robin-in-azure-ad"></a>Чтобы настроить автоматическую подготовку пользователей для перебора в Azure AD, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com). Выберите **Корпоративные приложения** , а затем **Все приложения**.

    ![Колонка "Корпоративные приложения"](common/enterprise-applications.png)

2. В списке приложений выберите **Robin**.

    ![Ссылка для перебора в списке приложений](common/all-applications.png)

3. Выберите вкладку **Подготовка**.

    ![Снимок экрана параметров управления с вызываемым параметром подготовки.](common/provisioning.png)

4. Для параметра **Режим подготовки к работе** выберите значение **Automatic** (Автоматически).

    ![Снимок экрана: раскрывающийся список режима подготовки с вызываемым автоматическим параметром.](common/provisioning-automatic.png)

5. В разделе **учетные данные администратора** введите `https://api.robinpowered.com/v1.0/scim-2` **URL-адрес клиента**. Введите значение **маркера проверки подлинности scim** , полученное ранее в **маркере секрета**. Нажмите кнопку **проверить подключение** , чтобы убедиться, что Azure AD может подключиться для перебора. В случае сбоя подключения убедитесь, что у учетной записи есть права администратора, и повторите попытку.

    ![URL-адрес клиента + токен](common/provisioning-testconnection-tenanturltoken.png)

6. В поле **Почтовое уведомление** введите адрес электронной почты пользователя или группы, которые должны получать уведомления об ошибках подготовки, а также установите флажок **Send an email notification when a failure occurs** (Отправить уведомление по электронной почте при сбое).

    ![Почтовое уведомление](common/provisioning-notification-email.png)

7. Выберите команду **Сохранить**.

8. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory пользователей для перебора**.

    ![Пользовательские сопоставления для перебора](media/robin-provisioning-tutorial/robin-user-mapping.png)

9. Проверьте атрибуты пользователя, которые синхронизированы из Azure AD для перебора в разделе **сопоставление атрибутов** . Атрибуты, выбранные как свойства **Matching** , используются для сопоставления учетных записей пользователей в процессе перебора операций обновления. Нажмите кнопку **Сохранить** , чтобы зафиксировать все изменения.

    ![Перемещаемые атрибуты пользователя](media/robin-provisioning-tutorial/robin-user-attribute-mapping.png)

10. В разделе **сопоставления** выберите **синхронизировать Azure Active Directory группы для перебора**.

    ![Сопоставления групп для перебора](media/robin-provisioning-tutorial/robin-group-mapping.png)

11. Проверьте атрибуты группы, которые синхронизированы из Azure AD для перебора в разделе **сопоставление атрибутов** . Атрибуты, выбранные как свойства **Matching** , используются для сопоставления групп в процессе перебора для операций обновления. Нажмите кнопку **Сохранить** , чтобы зафиксировать все изменения.

    ![Атрибуты группы для перебора](media/robin-provisioning-tutorial/robin-group-attribute-mapping.png)

12. Чтобы настроить фильтры области, ознакомьтесь со следующими инструкциями, предоставленными в [руководстве по фильтрам области](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Чтобы включить службу подготовки Azure AD для перебора, измените значение параметра **состояние подготовки** на **включено** в разделе **Параметры** .

    ![Состояние подготовки "Включено"](common/provisioning-toggle-on.png)

14. Определите пользователей и (или) группы, которые вы хотите подготавливать для перебора, выбрав нужные значения в **области** в разделе **Параметры** .

    ![Область действия подготовки](common/provisioning-scope.png)

15. Когда будете готовы выполнить подготовку, нажмите кнопку **Сохранить**.

    ![Сохранение конфигурации подготовки](common/provisioning-configuration-save.png)

После этого начнется начальная синхронизация пользователей и (или) групп, определенных в поле **Область** раздела **Параметры**. Начальная синхронизация занимает больше времени, чем последующие операции синхронизации. Если служба запущена, они выполняются примерно каждые 40 минут. В разделе **сведения о синхронизации** можно отслеживать ход выполнения и переходить по ссылкам для просмотра отчетов по подготовке, в которых описаны все действия, выполняемые службой подготовки Azure AD в процессе перебора.

Дополнительные сведения о чтении журналов подготовки Azure AD см. в руководстве по [отчетам об автоматической подготовке учетных записей](../app-provisioning/check-status-user-account-provisioning.md).



## <a name="additional-resources"></a>Дополнительные ресурсы

* [Управление подготовкой учетных записей пользователей для корпоративных приложений](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Дальнейшие действия

* [Сведения о просмотре журналов и получении отчетов о действиях по подготовке](../app-provisioning/check-status-user-account-provisioning.md)
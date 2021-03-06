---
title: Руководство по интеграции Azure Active Directory с приложением "Проверка подлинности SAML RStudio Connect" | Документация Майкрософт
description: Из этой статьи вы узнаете, как настроить единый вход Azure Active Directory в приложении "Проверка подлинности SAML RStudio Connect".
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/21/2020
ms.author: jeedes
ms.openlocfilehash: 23ad4347dc898f713066ea1ff061490d3eefb55b
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93080487"
---
# <a name="tutorial-azure-active-directory-integration-with-rstudio-connect-saml-authentication"></a>Руководство по интеграции Azure Active Directory с приложением "Проверка подлинности SAML RStudio Connect"

В этом учебнике описано, как интегрировать приложение "Проверка подлинности SAML RStudio Connect" с Azure Active Directory (Azure AD).
Интеграция Azure AD с приложением "Проверка подлинности SAML RStudio Connect" обеспечивает указанные ниже преимущества.

* Контроль доступа к приложению "Проверка подлинности SAML RStudio Connect" с помощью Azure AD.
* Автоматический вход пользователей в приложение "Проверка подлинности SAML RStudio Connect"(единый вход) с помощью учетной записи Azure AD.
* Вы можете управлять учетными записями централизованно на портале Azure.

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с приложением "Проверка подлинности SAML RStudio Connect", вам потребуется:

* подписка Azure AD; (если у вас нет среды Azure AD, вы можете получить [бесплатную учетную запись](https://azure.microsoft.com/free/));
* Проверка подлинности SAML RStudio Connect. Есть возможность [бесплатного ознакомительного использования в течение 45 дней.](https://www.rstudio.com/products/connect/)

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства вы настроите и проверите единый вход Azure AD в тестовой среде.

* Приложение "Проверка подлинности SAML RStudio Connect" поддерживает единый вход, инициируемый **поставщиком услуг и поставщиком удостоверений**.

* Приложение "Проверка подлинности SAML RStudio Connect" поддерживает **JIT-подготовку** пользователей.

## <a name="adding-rstudio-connect-saml-authentication-from-the-gallery"></a>Добавление приложения "Проверка подлинности SAML RStudio Connect" из коллекции

Чтобы настроить интеграцию приложения "Проверка подлинности SAML RStudio Connect" с Azure AD, необходимо добавить это приложение из коллекции в список управляемых приложений SaaS.

1. Войдите на портал Azure с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. В области навигации слева выберите службу **Azure Active Directory**.
1. Перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.
1. Чтобы добавить новое приложение, выберите **Новое приложение**.
1. В разделе **Добавление из коллекции** в поле поиска введите **Проверка подлинности SAML RStudio Connect**.
1. Выберите **Проверка подлинности SAML RStudio Connect** в области результатов и добавьте это приложение. Подождите несколько секунд, пока приложение не будет добавлено в ваш клиент.


## <a name="configure-and-test-azure-ad-sso-for-rstudio-connect-saml-authentication"></a>Настройка и проверка единого входа Azure AD для приложения "Проверка подлинности SAML RStudio Connect"

Настройте и проверьте единый вход Azure AD в приложении "Проверка подлинности SAML RStudio Connect" с помощью тестового пользователя **B. Simon**. Для обеспечения работы единого входа необходимо установить связь между пользователем Azure AD и соответствующим пользователем в приложении "Проверка подлинности SAML RStudio Connect".

Чтобы настроить и проверить единый вход Azure AD в приложение "Проверка подлинности SAML RStudio Connect" , выполните действия из указанных ниже стандартных блоков.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-sso)** необходима, чтобы пользователи могли использовать эту функцию.
    * **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
    * **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы разрешить пользователю Britta Simon использовать единый вход Azure AD.
2. **[Настройка единого входа в приложение "Проверка подлинности SAML RStudio Connect"](#configure-rstudio-connect-saml-authentication-sso)** необходима, чтобы настроить параметры единого входа на стороне приложения.
    * **[Создание тестового пользователя приложения "Проверка подлинности SAML RStudio Connect"](#create-rstudio-connect-saml-authentication-test-user)** требуется для того, чтобы в приложении "Проверка подлинности SAML RStudio Connect" существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
3. **[Проверка единого входа](#test-sso)** позволяет убедиться в правильности конфигурации.

## <a name="configure-azure-ad-sso"></a>Настройка единого входа Azure AD

Выполните следующие действия, чтобы включить единый вход Azure AD на портале Azure.

1. На портале Azure на странице интеграции с приложением **Проверка подлинности SAML RStudio Connect** найдите раздел **Управление** и выберите **Единый вход**.
1. На странице **Выбрать метод единого входа** выберите **SAML**.
1. На странице **Настройка единого входа с помощью SAML** щелкните значок "Изменить" (значок пера), чтобы открыть диалоговое окно **Базовая конфигурация SAML** и изменить параметры.

   ![Изменение базовой конфигурации SAML](common/edit-urls.png)

1. Если вы хотите настроить приложение в режиме, инициируемом **поставщиком удостоверений**, в разделе **Базовая конфигурация SAML** выполните следующие действия, заменив `<example.com>` адресом и портом сервера приложения "Проверка подлинности SAML RStudio Connect":

    ![Сведения о домене и URL-адресах единого входа для приложения "Проверка подлинности SAML RStudio Connect"](common/idp-intiated.png)

    а. В текстовом поле **Идентификатор** введите URL-адрес в формате `https://<example.com>/__login__/saml`.

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в формате `https://<example.com>/__login__/saml/acs`.

5. Чтобы настроить приложение для работы в режиме, инициируемом **поставщиком услуг**, щелкните **Задать дополнительные URL-адреса** и выполните следующие действия.

    ![Отправка метаданных приложения "Проверка подлинности SAML RStudio Connect"](common/metadata-upload-additional-signon.png)

    В текстовом поле **URL-адрес входа** введите URL-адрес в формате `https://<example.com>/`.

    > [!NOTE]
    > Эти значения приведены для примера. Замените их фактическими значениями идентификатора, URL-адреса ответа и URL-адреса входа. Они определяются с помощью адреса сервера приложения "Проверка подлинности SAML RStudio Connect" (`https://example.com` в приведенных выше примерах). Если у вас возникли проблемы, обратитесь в [группу поддержки приложения "Проверка подлинности SAML RStudio Connect"](mailto:support@rstudio.com). Можно также посмотреть шаблоны в разделе **Базовая конфигурация SAML** на портале Azure.

6. Для приложения "Проверка подлинности SAML RStudio Connect" утверждения SAML должны иметь определенный формат. Для этого необходимо добавить настраиваемые сопоставления атрибутов в вашу конфигурацию атрибутов токена SAML. На следующем снимке экрана показан список атрибутов по умолчанию, в котором **nameidentifier** сопоставляется с **user.userprincipalname**. Проверка подлинности SAML RStudio Connect ожидает, что **nameidentifier** будет сопоставляться с **user.mail**, поэтому щелкните значок **Изменить** и измените сопоставление атрибутов.

    ![Изображение](common/edit-attribute.png)

7. На странице **Настройка единого входа с помощью SAML** в разделе **Сертификат подписи SAML** нажмите кнопку "Копировать", чтобы копировать **URL-адрес метаданных федерации приложений** и сохранить его на компьютере.

    ![Ссылка для скачивания сертификата](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

В этом разделе описано, как на портале Azure создать тестового пользователя с именем B.Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.
1. В верхней части экрана выберите **Новый пользователь**.
1. В разделе **Свойства пользователя** выполните следующие действия.
   1. В поле **Имя** введите `B.Simon`.  
   1. В поле **Имя пользователя** введите username@companydomain.extension. Например, `B.Simon@contoso.com`.
   1. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.
   1. Нажмите кнопку **Создать**.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как включить единый вход Azure для пользователя B. Simon, предоставив этому пользователю доступ к приложению "Проверка подлинности SAML RStudio Connect".

1. На портале Azure выберите **Корпоративные приложения**, а затем — **Все приложения**.
1. В списке приложений выберите **Проверка подлинности SAML RStudio Connect**.
1. На странице "Обзор" приложения найдите раздел **Управление** и выберите **Пользователи и группы**.
1. Выберите **Добавить пользователя**, а в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.
1. В диалоговом окне **Пользователи и группы** выберите **B.Simon** в списке пользователей, а затем в нижней части экрана нажмите кнопку **Выбрать**.
1. Если пользователям необходимо назначить роль, вы можете выбрать ее из раскрывающегося списка **Выберите роль**. Если для этого приложения не настроена ни одна роль, будет выбрана роль "Доступ по умолчанию".
1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

## <a name="configure-rstudio-connect-saml-authentication-sso"></a>Настройка единого входа в приложении "Проверка подлинности SAML RStudio Connect"

Чтобы настроить единый вход для приложения **Проверка подлинности SAML RStudio Connect**, необходимо указать **URL-адрес метаданных федерации приложений** и **адрес сервера**, которые использовались выше. Это можно реализовать в файле конфигурации приложения "Проверка подлинности SAML RStudio Connect" по пути `/etc/rstudio-connect.rstudio-connect.gcfg`.

Вот пример файла конфигурации:

```
[Server]
SenderEmail =

; Important! The user-facing URL of your RStudio Connect SAML Authentication server.
Address = 

[Http]
Listen = :3939

[Authentication]
Provider = saml

[SAML]
Logging = true

; Important! The URL where your IdP hosts the SAML metadata or the path to a local copy of it placed in the RStudio Connect SAML Authentication server.
IdPMetaData = 

IdPAttributeProfile = azure
SSOInitiated = IdPAndSP
```

Укажите свой **адрес сервера** в значении `Server.Address`, а **URL-адрес метаданных федерации приложений** в значении `SAML.IdPMetaData`. Обратите внимание, что в этом примере конфигурации используется незашифрованное HTTP-подключение, в то время как Azure AD требует использования зашифрованного HTTPS-подключения. Вы можете использовать [обратный прокси-сервер](https://docs.rstudio.com/connect/admin/proxy/) перед приложением "Проверка подлинности SAML RStudio Connect" или настроить приложение "Проверка подлинности SAML RStudio Connect" для [использования HTTPS напрямую](https://docs.rstudio.com/connect/admin/appendix/configuration/#HTTPS). 

Если у вас возникли проблемы с конфигурацией, ознакомьтесь с [руководством по приложению "Проверка подлинности SAML RStudio Connect" для администраторов](https://docs.rstudio.com/connect/admin/authentication/saml/) или отправьте сообщение электронной почты в [группу поддержки RStudio](mailto:support@rstudio.com), чтобы получить помощь.

### <a name="create-rstudio-connect-saml-authentication-test-user"></a>Создание тестового пользователя приложения "Проверка подлинности SAML RStudio Connect"

Используя инструкции этого раздела, вы создадите в приложении "Проверка подлинности SAML RStudio Connect" пользователя Britta Simon. Приложение "Проверка подлинности SAML RStudio Connect" поддерживает JIT-подготовку, и эта функция включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Если пользователь еще не существует в приложении "Проверка подлинности SAML RStudio Connect", он создается при попытке доступа к приложению "Проверка подлинности SAML RStudio Connect".

## <a name="test-sso"></a>Проверка единого входа 

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью указанных ниже способов. 

#### <a name="sp-initiated"></a>Инициация поставщиком услуг:

* Выберите **Тестировать приложение** на портале Azure. Вы будете перенаправлены по URL-адресу для входа в приложение "Проверка подлинности SAML RStudio Connect", где можно инициировать поток входа.  

* Перейдите по URL-адресу для входа в приложение "Проверка подлинности SAML RStudio Connect" и инициируйте поток входа.

#### <a name="idp-initiated"></a>Вход, инициированный поставщиком удостоверений

* На портале Azure выберите **Тестировать приложение**, и вы автоматически войдете в приложение "Проверка подлинности SAML RStudio Connect", для которого настроен единый вход. 

Вы можете также использовать Панель доступа корпорации Майкрософт для тестирования приложения в любом режиме. Щелкнув плитку приложения "Проверка подлинности SAML RStudio Connect" на Панели доступа, вы будете перенаправлены на страницу входа приложения, если выполнены настройки для использования в режиме поставщика услуг, или автоматически войдете в приложение "Проверка подлинности SAML RStudio Connect", для которого настроен единый вход, если выполнены настройки для использования в режиме поставщика удостоверений. См. дополнительные сведения о [панели доступа](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="next-steps"></a>Дальнейшие действия

После настройки приложения "Проверка подлинности SAML RStudio Connect" вы можете применить управление сеансами, которое защищает от хищения конфиденциальных данных вашей организации и несанкционированного доступа к ним в реальном времени. Управление сеансом является расширением функции условного доступа. [Узнайте, как применять управление сеансами с помощью Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


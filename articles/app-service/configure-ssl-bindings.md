---
title: Защита пользовательского DNS-имени с помощью привязки TLS/SSL
description: Обеспечьте защищенный доступ по протоколу HTTPS к пользовательскому домену, создав привязку TLS/SSL с сертификатом. Повышение безопасности веб-сайта за счет применения протокола HTTPS или TLS 1.2.
tags: buy-ssl-certificates
ms.topic: tutorial
ms.date: 04/30/2020
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: f7301809b3befc41110a32062d6e478c412fa56e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90981126"
---
# <a name="secure-a-custom-dns-name-with-a-tlsssl-binding-in-azure-app-service"></a>Защита пользовательского DNS-имени с помощью привязки TLS/SSL в Службе приложений Azure

В этой статье показано, как защитить [личный домен](app-service-web-tutorial-custom-domain.md) в [приложении Службы приложений](./index.yml) или [приложении-функции](../azure-functions/index.yml) путем создания привязки к сертификату. По завершении работы с этой статьей вы сможете получить доступ к приложению Службы приложений в конечной точке `https://` для настраиваемого DNS-имени (например, `https://www.contoso.com`). 

![Веб-приложение с настраиваемым TLS/SSL-сертификатом](./media/configure-ssl-bindings/app-with-custom-ssl.png)

Обеспечение защиты [личного домена](app-service-web-tutorial-custom-domain.md) с использованием сертификата состоит из двух этапов.

- [Добавление в Службу приложений закрытого сертификата](configure-ssl-certificate.md), который отвечает всем [требованиям к закрытым сертификатам](configure-ssl-certificate.md#private-certificate-requirements).
-  Создание привязки TLS к соответствующему личному домену. В этой статье рассматривается второй этап.

В этом руководстве описано следующее:

> [!div class="checklist"]
> * Выбор более высокой ценовой категории.
> * Защита личного домена с помощью сертификата.
> * Принудительное использование HTTPS
> * Принудительное применение TLS 1.1/1.2
> * автоматизация управления TLS с помощью скриптов.

## <a name="prerequisites"></a>Предварительные требования

Ознакомьтесь со следующими статьями:

- [Создано приложение службы приложений](./index.yml).
- [Руководство. Сопоставление существующего настраиваемого DNS-имени со Службой приложений Azure](app-service-web-tutorial-custom-domain.md) или [Приобретение личного доменного имени в Службе приложений Azure](manage-custom-dns-buy-domain.md).
- [Добавление закрытого сертификата в приложение](configure-ssl-certificate.md)

> [!NOTE]
> Проще всего добавить закрытый сертификат, [создав бесплатный управляемый сертификат Службы приложений](configure-ssl-certificate.md#create-a-free-certificate-preview) (предварительная версия).

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

<a name="upload"></a>

## <a name="secure-a-custom-domain"></a>Защита личного домена

Выполните следующие шаги:

На <a href="https://portal.azure.com" target="_blank">портале Azure</a> в меню слева выберите **Службы приложений** >  **\<app-name>** .

В левой области навигации приложения запустите диалоговое окно **Привязка TLS/SSL**, выполнив одно из приведенных ниже действий.

- Выберите **Личные домены** > **Добавить привязку**.
- Выберите **Параметры TLS/SSL** > **Добавить привязку TLS/SSL**.

![Добавка привязки к домену](./media/configure-ssl-bindings/secure-domain-launch.png)

В области **Личный домен** выберите личный домен, для которого необходимо добавить привязку.

Если у приложения уже есть сертификат для выбранного личного домена, перейдите напрямую к разделу [Создание привязки](#create-binding). В противном случае продолжайте работу.

### <a name="add-a-certificate-for-custom-domain"></a>Добавление сертификата для личного домена

Если у приложения нет сертификата для выбранного личного домена, у вас есть два варианта:

- **Отправить сертификат PFX**. Для этого выполните инструкции из раздела [Upload a private certificate](configure-ssl-certificate.md#upload-a-private-certificate) (Передача закрытого сертификата), а затем выберите этот вариант.
- **Импортировать сертификат Службы приложений**. Для этого выполните инструкции из раздела [Import an App Service Certificate](configure-ssl-certificate.md#import-an-app-service-certificate) (Импорт сертификата Службы приложений), а затем выберите этот вариант.

> [!NOTE]
> Вы также можете [создать бесплатный сертификат](configure-ssl-certificate.md#create-a-free-certificate-preview) (предварительная версия) или [импортировать сертификат Key Vault](configure-ssl-certificate.md#import-a-certificate-from-key-vault), но это нужно сделать отдельно, а затем вернуться в диалоговое окно **Привязка TLS/SSL**.

### <a name="create-binding"></a>Создание привязки

Чтобы настроить привязку TLS в диалоговом окне **Привязка TLS/SSL**, воспользуйтесь сведениями из следующей таблицы и щелкните **Добавить привязку**.

| Параметр | Описание |
|-|-|
| Личный домен | Имя домена, для которого будет добавлена привязка TLS/SSL. |
| Отпечаток закрытого сертификата | Привязка сертификата. |
| Тип TLS/SSL | <ul><li>**[SSL на основе SNI](https://en.wikipedia.org/wiki/Server_Name_Indication)** позволяет добавить несколько привязок SSL на основе SNI. Этот параметр позволяет указать несколько TLS/SSL-сертификатов для защиты нескольких доменов с одним IP-адресом. Большинство современных браузеров (включая Internet Explorer, Chrome, Firefox и Opera) поддерживает SNI (дополнительные сведения см. в статье [Server Name Indication](https://wikipedia.org/wiki/Server_Name_Indication)).</li><li>**SSL на основе IP** позволяет добавить только одну привязку SSL на основе IP. Этот параметр позволяет указать только один TLS/SSL-сертификат для защиты выделенного общедоступного IP-адреса. После настройки привязки воспользуйтесь сведениями из раздела [Переназначение записи для SSL на основе IP-адреса](#remap-records-for-ip-ssl).<br/>SSL на основе IP-адреса поддерживается только на уровне **"Стандартный"** или выше. </li></ul> |

После завершения операции состояние TLS/SSL личного домена меняется на **Безопасный**.

![Удачно выполненная привязка TLS/SSL](./media/configure-ssl-bindings/secure-domain-finished.png)

> [!NOTE]
> Состояние **Защищено** **личного домена** означает, что он защищен с помощью сертификата, но Служба приложений не проверяет, например, является ли сертификат самозаверяющим или действительным, что также может привести к отображению ошибки или предупреждения в браузере.

## <a name="remap-records-for-ip-ssl"></a>Повторное сопоставление записей для SSL на основе IP-адресов

Если вы не используете SSL на основе IP в приложении, перейдите к разделу [Тестирование HTTPS](#test-https).

Возможно, вам потребуется внести два изменения:

- По умолчанию приложение использует общий общедоступный IP-адрес. После привязки сертификата с помощью SSL на основе IP Служба приложений создает выделенный IP-адрес для вашего приложения. Если с приложением сопоставлена запись A, обновите реестр домена, указав этот новый выделенный IP-адрес.

    Он появится на странице **Личный домен** вашего приложения. [Скопируйте этот IP-адрес](app-service-web-tutorial-custom-domain.md#info), затем [переназначьте его записи A](app-service-web-tutorial-custom-domain.md#map-an-a-record).

- При наличии привязки SSL на основе SNI к `<app-name>.azurewebsites.net` [повторно сопоставьте все сопоставления CNAME](app-service-web-tutorial-custom-domain.md#map-a-cname-record), чтобы они указывали на `sni.<app-name>.azurewebsites.net` (добавьте префикс `sni`).

## <a name="test-https"></a>Тестирование HTTPS

В разных браузерах перейдите по адресу `https://<your.custom.domain>`, чтобы проверить, как он обслуживает ваше приложение.

:::image type="content" source="./media/configure-ssl-bindings/app-with-custom-ssl.png" alt-text="Снимок экрана: пример перехода к странице личного домена с выделенным URL-адресом contoso.com.":::

Код приложения может проверить протокол с помощью заголовка x-appservice-proto. Заголовок будет иметь значение `http` или `https`. 

> [!NOTE]
> Если приложение выдает ошибки проверки сертификата, вероятно, вы используете самозаверяющий сертификат.
>
> Если это не так, возможно, вы пропустили промежуточные сертификаты при экспорте сертификата в PFX-файл.

## <a name="prevent-ip-changes"></a>Предотвращение изменений IP-адреса

Входящий IP-адрес может измениться после удаления привязки, даже если привязка сделана на базе SSL на основе IP. Это особенно важно при обновлении сертификата, который уже связан с привязкой SSL на основе IP. Чтобы избежать изменения IP-адреса приложения, выполните по очереди следующие действия:

1. Передайте новый сертификат.
2. Привяжите новый сертификат к пользовательскому домену, не удаляя старый сертификат. Таким образом вы замените привязку, а не удалите старый сертификат.
3. Удалите старый сертификат. 

## <a name="enforce-https"></a>Принудительное использование HTTPS

По умолчанию любой пользователь по-прежнему может получить доступ к вашему приложению с помощью HTTP. Вы можете перенаправить все HTTP-запросы на HTTPS-порт.

На странице приложения в области слева выберите **Параметры SSL**. Затем в окне **Только HTTPS**выберите **ВКЛ**.

![Принудительное использование HTTPS](./media/configure-ssl-bindings/enforce-https.png)

По завершении операции перейдите по любому из URL-адресов HTTP, которые указывают на ваше приложение. Пример:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="enforce-tls-versions"></a>Принудительное применение версий TLS

Приложение по умолчанию разрешает применение [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) версии 1.2, которая является рекомендуемой в соответствии с такими отраслевыми стандартами, как [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). Чтобы принудительно применить другую версию TLS сделайте следующее:

На странице приложения в области слева выберите **Параметры SSL**. Затем в разделе **версии TLS** выберите минимальную требуемую версию TLS. Этот параметр определяет только входящие вызовы. 

![Принудительное использование TLS 1.1 или 1.2](./media/configure-ssl-bindings/enforce-tls1-2.png)

После этой операции приложение отклоняет все подключения с более ранними версиями TLS.

## <a name="handle-tls-termination"></a>Обработка терминирования TLS/SSL

В Службе приложений [терминирование TLS](https://wikipedia.org/wiki/TLS_termination_proxy) происходит в подсистеме балансировки нагрузки сети, поэтому все HTTPS-запросы достигают вашего приложения в виде незашифрованных HTTP-запросов. Если логика вашего приложения проверяет, зашифрованы ли пользовательские запросы, проверяйте заголовок `X-Forwarded-Proto`.

Руководства по настройке для конкретного языка, например [руководство по настройке Node.js в Linux](configure-language-nodejs.md#detect-https-session), описывают возможность обнаруживать сеанс HTTPS в коде приложения.

## <a name="automate-with-scripts"></a>Автоматизация с помощью сценариев

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom TLS/SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom TLS/SSL certificate to a web app")]

## <a name="more-resources"></a>Дополнительные ресурсы

* [Использование TLS/SSL-сертификата в коде в Службе приложений Azure](configure-ssl-certificate-in-code.md)
* [FAQ: Configuration and management FAQs for Web Apps in Azure](./faq-configuration-and-management.md) (Часто задаваемые вопросы о настройке и управлении для функции "Веб-приложения" в Azure)
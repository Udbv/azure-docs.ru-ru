---
title: Предварительные требования для облачной подготовки Azure AD Connect в Azure AD
description: В этой статье описываются необходимые условия и требования к оборудованию для облачной подготовки.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/16/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8eb8de2424012d12f216f154eb077028a8f82d76
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "96173708"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-provisioning"></a>Предварительные требования для облачной подготовки Azure AD Connect
В этой статье описано, как выбрать и применить облачную подготовку Azure Active Directory (Azure AD) Connect в качестве решения для идентификации.

## <a name="cloud-provisioning-agent-requirements"></a>Требования к агенту облачной подготовки
Для использования облачной подготовки Azure AD Connect вам потребуются следующие ресурсы.

- Учетные данные администратора домена или администратора предприятия для создания Azure AD Connect Cloud Sync gMSA (групповая управляемая учетная запись службы) для запуска службы агента. 
- Учетная запись гибридного администратора удостоверений для клиента Azure AD, который не является гостевым пользователем.
- Локальный сервер для агента подготовки с Windows 2012 R2 или более поздней версии.  Этот сервер должен быть сервером уровня 0 на основе [модели административного уровня Active Directory](/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material).
- Конфигурации для локального брандмауэра.

## <a name="group-managed-service-accounts"></a>Групповые управляемые учетные записи служб
Групповая управляемая учетная запись службы — это управляемая Доменная учетная запись, которая обеспечивает автоматическое управление паролями, упрощенное управление именами участников-служб, возможность делегировать управление другим администраторам, а также расширяет эту функциональность на нескольких серверах.  Azure AD Connect Cloud Sync поддерживает и использует gMSA для запуска агента.  Для создания этой учетной записи вам будет предложено ввести учетные данные администратора во время установки.  Учетная запись будет отображаться как (Домаин\проважентгмса $).  Дополнительные сведения о gMSA см. в разделе [групповые управляемые учетные записи служб](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) . 

### <a name="prerequisites-for-gmsa"></a>Необходимые компоненты для gMSA:
1.  Схема Active Directory в лесу домена gMSA должна быть обновлена до Windows Server 2012
2.  [Модули POWERSHELL RSAT](/windows-server/remote/remote-server-administration-tools) на контроллере домена
3.  По крайней мере один контроллер домена в домене должен работать под Windows Server 2012.
4.  Сервер, присоединенный к домену, на котором устанавливается агент, должен быть либо Windows Server 2012, либо более поздней версии.

Инструкции по обновлению существующего агента для использования учетной записи gMSA см. в разделе [групповые управляемые учетные записи служб](how-to-install.md#group-managed-service-accounts).

### <a name="in-the-azure-active-directory-admin-center"></a>В центре администрирования Azure Active Directory

1. Создайте облачную учетную запись администратора гибридной идентификации в клиенте Azure AD. Таким образом, вы сможете управлять конфигурацией клиента, если работа локальных служб завершится сбоем или они станут недоступными. Узнайте [, как добавить облачную учетную запись администратора гибридной идентификации](../fundamentals/add-users-azure-active-directory.md). Выполнение этого шага очень важно, чтобы не потерять доступ к клиенту.
1. Добавьте одно [имя личного домена](../fundamentals/add-custom-domain.md) (или несколько) в клиент Azure AD. Пользователи могут выполнить вход с помощью одного из этих доменных имен.

### <a name="in-your-directory-in-active-directory"></a>В каталоге Active Directory

Запустите [средство IdFix](/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix), чтобы подготовить атрибуты каталога для синхронизации.

### <a name="in-your-on-premises-environment"></a>В локальной среде

1. Выберите присоединенный к домену сервер узла под управлением Windows Server 2012 R2 или более поздней версии, на котором есть не менее 4 ГБ ОЗУ и среда выполнения .NET 4.7.1 или более поздней версии.

1. Политика выполнения PowerShell на локальном сервере должна иметь значение Undefined или RemoteSigned.

1. Если между серверами и Azure AD присутствует брандмауэр, необходимо настроить указанные ниже элементы.
   - Убедитесь, что агенты могут передавать *исходящие* запросы в Azure AD через приведенные ниже порты.

        | Номер порта | Как он используется |
        | --- | --- |
        | **80** | Скачивание списков отзыва сертификатов при проверке TLS/SSL-сертификата.  |
        | **443** | Обработка всего исходящего трафика для службы. |
        |**8082**|Требуется для установки, если требуется настроить API администрирования.  Этот порт можно удалить после установки агента, если вы не планируете использовать API.   |
        | **8080** (необязательно) | Агенты передают данные о своем состоянии каждые 10 минут через порт 8080, если порт 443 недоступен. Эти данные о состоянии отображаются на портале Azure AD. |
   
     
   - Если брандмауэр применяет правила в соответствии с отправляющими трафик пользователями, откройте эти порты для трафика, поступающего от служб Windows, которые работают как сетевая служба.
   - Если брандмауэр или прокси-сервер позволяет указать надежные суффиксы, можно добавить подключения к \*.msappproxy.net и \*.servicebus.windows.net. Если нет, разрешите доступ к [диапазонам IP-адресов центра обработки данных Azure](https://www.microsoft.com/download/details.aspx?id=41653). Список диапазонов IP-адресов обновляется еженедельно.
   - Агентам требуется доступ к адресам login.windows.net и login.microsoftonline.com для первоначальной регистрации. Откройте эти URL-адреса в брандмауэре.
   - Для проверки сертификатов разблокируйте следующие URL-адреса: mscrl.microsoft.com:80, crl.microsoft.com:80, ocsp.msocsp.com:80 и www\.microsoft.com:80. Эти же URL-адреса используются для проверки сертификатов в других продуктах Майкрософт, так что они могут быть уже разблокированы.

>[!NOTE]
> Установка агента облачной подготовки на Windows Server Core не поддерживается.




### <a name="additional-requirements"></a>Дополнительные требования
- [Microsoft .NET Framework 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>Требования к TLS

>[!NOTE]
>Протокол TLS обеспечивает безопасность обмена данными. Изменение параметров TLS влияет на весь лес. Дополнительные сведения см. в статье [Обновление, предусматривающее использование TLS 1.1 и TLS 1.2 в качестве безопасных протоколов по умолчанию в WinHTTP в Windows](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi).

Перед установкой на сервере Windows, на котором размещен агент облачной подготовки Azure AD Connect, должен быть настроен протокол TLS 1.2.

Чтобы включить TLS 1.2, сделайте следующее:

1. Настройте следующие разделы реестра:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. Перезапустите сервер.




## <a name="next-steps"></a>Дальнейшие действия 

- [Что собой представляет подготовка?](what-is-provisioning.md)
- [Что такое облачная подготовка Azure AD Connect?](what-is-cloud-provisioning.md)
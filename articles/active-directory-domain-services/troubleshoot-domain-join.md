---
title: Устранение неполадок при присоединении домена к доменным службам Azure AD | Документация Майкрософт
description: Узнайте, как устранять распространенные проблемы при попытке присоединения к домену виртуальной машины или подключении приложения к Azure Active Directory доменным службам, а также не удается подключиться к управляемому домену или выполнить проверку подлинности.
services: active-directory-ds
author: MicrosoftGuyJFlo
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/06/2020
ms.author: joflore
ms.openlocfilehash: ee60b684d64ef49fbb669de8c98203e2df5268bf
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "91967517"
---
# <a name="troubleshoot-domain-join-problems-with-an-azure-active-directory-domain-services-managed-domain"></a>Устранение неполадок при присоединении к домену с помощью управляемого домена доменных служб Azure Active Directory

При попытке присоединить виртуальную машину или подключить приложение к управляемому домену доменных служб Azure Active Directory (Azure AD DS) может появиться сообщение об ошибке, которое не удается выполнить. Чтобы устранить проблемы с присоединением к домену, просмотрите следующие моменты:

* Если вы не получаете запрос проверки подлинности, виртуальная машина или приложение не сможет подключиться к управляемому домену Azure AD DS.
    * Начните устранять неполадки [подключения к домену](#connectivity-issues-for-domain-join).
* При возникновении ошибки во время проверки подлинности подключение к управляемому домену будет успешным.
    * Начните устранять неполадки, [связанные с учетными данными, во время присоединение к домену](#credentials-related-issues-during-domain-join).

## <a name="connectivity-issues-for-domain-join"></a>Проблемы с подключением для присоединения к домену

Если виртуальная машина не может найти управляемый домен, обычно это связано с сетевым подключением или конфигурацией. Чтобы найти и устранить проблему, ознакомьтесь со следующими шагами по устранению неполадок.

1. Убедитесь, что виртуальная машина подключена к тому же или к одноранговой виртуальной сети, что и управляемый домен. В противном случае виртуальная машина не сможет найти домен и подключиться к нему для присоединения.
    * Если виртуальная машина не подключена к той же виртуальной сети, убедитесь, что пиринг виртуальных сетей или VPN-подключение *активны* или *подключены* , чтобы трафик правильно перенаправлялся.
1. Попробуйте проверить связь с доменом, используя доменное имя управляемого домена, например `ping aaddscontoso.com` .
    * Если ответ на запрос проверки связи не удается устранить, попробуйте проверить связь с IP-адресами домена, отображаемого на странице обзора на портале для управляемого домена, например `ping 10.0.0.4` .
    * Если вы можете успешно проверить связь с IP-адресом, но не с доменом, служба DNS может быть неправильно настроена. Убедитесь, что [для виртуальной сети настроены DNS-серверы управляемого домена][configure-dns].
1. Попробуйте очистить кэш сопоставителя DNS на виртуальной машине, например `ipconfig /flushdns` .

### <a name="network-security-group-nsg-configuration"></a>Конфигурация группы безопасности сети (NSG)

При создании управляемого домена также создается группа безопасности сети с соответствующими правилами для успешного выполнения операции домена. Если вы изменяете или создаете дополнительные правила группы безопасности сети, вы можете непреднамеренно блокировать порты, необходимые для Azure AD DS для предоставления служб подключения и проверки подлинности. Эти правила группы безопасности сети могут вызывать такие проблемы, как синхронизация паролей, которые не могут быть завершены, пользователи не смогут войти в систему или проблемы с подключением к домену.

Если проблемы с подключением продолжают возникать, ознакомьтесь со следующими действиями по устранению неполадок.

1. Проверьте состояние работоспособности управляемого домена в портал Azure. Если у вас есть оповещение для *AADDS001*, правило группы безопасности сети блокирует доступ.
1. Проверьте [необходимые порты и правила группы безопасности сети][network-ports]. Убедитесь, что ни один из правил группы безопасности сети, которые не применяются к виртуальной машине или виртуальной сети, из которых вы подключаетесь, не блокирует эти сетевые порты.
1. После устранения проблем с конфигурацией группы безопасности сети предупреждение *AADDS001* исчезнет со страницы работоспособности примерно через 2 часа. Теперь, когда сетевое подключение уже доступно, попробуйте присоединить виртуальную машину к домену еще раз.

## <a name="credentials-related-issues-during-domain-join"></a>Проблемы с учетными данными во время присоединение к домену

Если появится диалоговое окно с запросом учетных данных для присоединения к управляемому домену, виртуальная машина сможет подключиться к домену с помощью виртуальной сети Azure. Процесс присоединение к домену завершается сбоем при проверке подлинности в домене или авторизации для завершения процесса присоединение к домену с использованием учетных данных.

Чтобы устранить неполадки, связанные с учетными данными, ознакомьтесь со следующими действиями по устранению неполадок.

1. Для указания учетных данных рекомендуется использовать формат имени участника-пользователя, например `dee@contoso.onmicrosoft.com`. Убедитесь, что имя участника-пользователя правильно настроено в Azure AD.
    * *SamAccountName* для вашей учетной записи может быть автоматически создан, если в клиенте есть несколько пользователей с одним префиксом имени участника-пользователя или если префикс UPN слишком длинный. Поэтому формат *SamAccountName* для вашей учетной записи может отличаться от предполагаемого или используемого в локальном домене.
1. Попробуйте использовать учетные данные для учетной записи пользователя, которая является частью управляемого домена, чтобы присоединить виртуальные машины к управляемому домену.
1. Убедитесь, что вы [включили синхронизацию паролей][enable-password-sync] и ожидались достаточно долго для завершения начальной синхронизации паролей.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о Active Directory процессах в рамках операции присоединение к домену см. в разделе [проблемы с соединением и проверкой подлинности][join-authentication-issues].

Если у вас по-прежнему возникли проблемы при присоединении виртуальной машины к управляемому домену, [найдите справку и отправьте запрос в службу поддержки для Azure Active Directory][azure-ad-support].

<!-- INTERNAL LINKS -->
[enable-password-sync]: tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[network-ports]: network-considerations.md#network-security-groups-and-required-ports
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[configure-dns]: tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network

<!-- EXTERNAL LINKS -->
[join-authentication-issues]: /previous-versions/windows/it-pro/windows-2000-server/cc961817(v=technet.10)

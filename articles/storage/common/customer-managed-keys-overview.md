---
title: Ключи, управляемые клиентом для шифрования учетной записи
titleSuffix: Azure Storage
description: Для защиты данных в учетной записи хранения можно использовать собственный ключ шифрования. Указанный ключ CMK используется для защиты доступа к ключу, который шифрует данные, и управления этим доступом. Ключи CMK обеспечивают большую гибкость при администрировании операций управления доступом.
services: storage
author: tamram
ms.service: storage
ms.date: 09/15/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 2b474ae184374a2c91dcba15517048556686ec35
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2020
ms.locfileid: "92782235"
---
# <a name="customer-managed-keys-for-azure-storage-encryption"></a>Управляемые клиентом ключи для шифрования службы хранилища Azure

Для защиты данных в учетной записи хранения можно использовать собственный ключ шифрования. Указанный ключ CMK используется для защиты доступа к ключу, который шифрует данные, и управления этим доступом. Ключи CMK обеспечивают большую гибкость при администрировании операций управления доступом.

Для хранения ключей, управляемых клиентом, необходимо использовать либо Azure Key Vault, либо Azure Key Vault управляемую аппаратную модель безопасности (HSM) (Предварительная версия). Вы можете создать собственные ключи и сохранить их в хранилище ключей или в управляемом модуле HSM. также можно использовать Azure Key Vault интерфейсы API для создания ключей. Учетная запись хранения и хранилище ключей или управляемый модуль HSM должны находиться в одном и том же регионе и в одном клиенте Azure Active Directory (Azure AD), но они могут находиться в разных подписках.

Дополнительные сведения о Azure Key Vault см. в разделе [что такое Azure Key Vault?](../../key-vault/general/overview.md).

> [!NOTE]
> Управляемый модуль HSM Azure Key Vault и Azure Key Vault поддерживают те же интерфейсы API и интерфейсы управления для конфигурации.

## <a name="about-customer-managed-keys"></a>Сведения об управляемых клиентом ключах

На следующей схеме показано, как служба хранилища Azure использует Azure Active Directory, хранилище ключей или управляемый модуль HSM для выполнения запросов с помощью управляемого клиентом ключа:

![Схема работы ключей, управляемых клиентом, в службе хранилища Azure](media/customer-managed-keys-overview/encryption-customer-managed-keys-diagram.png)

В следующем списке описаны шаги, пронумерованные на схеме.

1. Администратор Azure Key Vault предоставляет разрешения на ключи шифрования для управляемого удостоверения, связанного с учетной записью хранения.
2. Администратор службы хранилища Azure настраивает шифрование с помощью управляемого клиентом ключа для учетной записи хранения.
3. Служба хранилища Azure использует управляемое удостоверение, связанное с учетной записью хранения, для проверки подлинности доступа к Azure Key Vault через Azure Active Directory.
4. Служба хранилища Azure заключает ключ шифрования учетной записи в ключ клиента в Azure Key Vault.
5. Для операций чтения и записи служба хранилища Azure отправляет запросы в Azure Key Vault для распаковки ключа шифрования учетной записи для выполнения операций шифрования и расшифровки.

## <a name="customer-managed-keys-for-queues-and-tables"></a>Ключи, управляемые клиентом для очередей и таблиц

Данные, хранящиеся в хранилище очередей и таблиц, не защищаются автоматически с помощью ключа, управляемого клиентом, если для учетной записи хранения включены ключи, управляемые клиентом. При необходимости эти службы можно настроить для включения в эту защиту во время создания учетной записи хранения.

Дополнительные сведения о создании учетной записи хранения, поддерживающей управляемые клиентом ключи для очередей и таблиц, см. в разделе [Создание учетной записи, поддерживающей управляемые клиентом ключи для таблиц и очередей](account-encryption-key-create.md).

Данные в хранилище BLOB-объектов и файлах Azure всегда защищаются ключами, управляемыми клиентом, если для учетной записи хранения настроены ключи, управляемые клиентом.

## <a name="enable-customer-managed-keys-for-a-storage-account"></a>Включение управляемых клиентом ключей для учетной записи хранения

При настройке ключа, управляемого клиентом, служба хранилища Azure заключает корневой ключ шифрования данных для учетной записи с управляемым клиентом ключом в связанное хранилище ключей или управляемый модуль HSM. Включение управляемых клиентом ключей не влияет на производительность и вступает в силу немедленно.

Если вы включаете или отключаете управляемые ключи клиента или изменяете ключ или версию ключа, Защита корневого ключа шифрования изменяется, но данные в учетной записи хранения Azure не требуют повторного шифрования.

Ключи, управляемые клиентом, могут быть включены только в существующих учетных записях хранения. Хранилище ключей или управляемый модуль HSM должны быть настроены для предоставления разрешений управляемому удостоверению, связанному с учетной записью хранения. Управляемое удостоверение доступно только после создания учетной записи хранения.

Вы можете в любое время переключаться между управляемыми клиентом ключами и ключами, управляемыми корпорацией Майкрософт. Дополнительные сведения о ключах, управляемых корпорацией Майкрософт, см. в разделе [сведения об управлении ключами шифрования](storage-service-encryption.md#about-encryption-key-management).

Сведения о настройке шифрования службы хранилища Azure с помощью управляемых клиентом ключей в хранилище ключей см. в статье [Настройка шифрования с помощью управляемых клиентом ключей, хранящихся в Azure Key Vault](customer-managed-keys-configure-key-vault.md). Сведения о настройке управляемых пользователем ключей в управляемом модуле HSM см. в статье [Настройка шифрования с помощью управляемых клиентом ключей, хранящихся в Azure Key Vault управляемый HSM (Предварительная версия)](customer-managed-keys-configure-key-vault-hsm.md).

> [!IMPORTANT]
> Управляемые клиентом ключи основываются на управляемых удостоверениях для ресурсов Azure — функции Azure AD. Сейчас управляемые удостоверения не поддерживаются в сценариях работы с разными каталогами. При настройке ключей, управляемых клиентом, в портал Azure управляемое удостоверение автоматически назначается вашей учетной записи хранения, как описано в этой статье. Если впоследствии вы перемещаете подписку, группу ресурсов или учетную запись хранения из одного каталога Azure AD в другой, управляемое удостоверение, связанное с учетной записью хранения, не передается в новый клиент, поэтому управляемые клиентом ключи могут перестать работать. Дополнительные сведения см. в статье **Передача подписки между каталогами Azure AD** в [часто задаваемых вопросах и известных проблемах с управляемыми удостоверениями для ресурсов Azure](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).  

Шифрование службы хранилища Azure поддерживает ключи RSA и RSA-HSM размером 2048, 3072 и 4096. Дополнительные сведения о ключах см. в разделе [о ключах](../../key-vault/keys/about-keys.md).

Использование хранилища ключей или управляемого модуля HSM связано с затратами. Дополнительные сведения см. в разделе [цены на Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="update-the-key-version"></a>Обновление версии ключа

При настройке шифрования с помощью управляемых клиентом ключей у вас есть два варианта обновления версии ключа:

- **Автоматическое обновление версии ключа:** Чтобы автоматически обновить ключ, управляемый клиентом, когда доступна новая версия, не указывайте версию ключа при включении шифрования с помощью управляемых клиентом ключей для учетной записи хранения. Если версия ключа не указана, служба хранилища Azure проверяет хранилище ключей или управляемый модуль HSM ежедневно для новой версии ключа, управляемого клиентом. Служба хранилища Azure автоматически использует последнюю версию ключа.
- **Обновите версию ключа вручную:** Чтобы использовать определенную версию ключа для шифрования службы хранилища Azure, укажите эту версию ключа при включении шифрования с помощью управляемых клиентом ключей для учетной записи хранения. Если вы укажете версию ключа, служба хранилища Azure использует эту версию для шифрования до тех пор, пока вы не обновите версию ключа вручную.

    При явном указании версии ключа необходимо вручную обновить учетную запись хранения, чтобы она использовала новый универсальный код ресурса (URI) новой версии при создании новой версии. Сведения о том, как обновить учетную запись хранения для использования новой версии ключа, см. в разделе [Настройка шифрования с помощью управляемых клиентом ключей, хранящихся в Azure Key Vault](customer-managed-keys-configure-key-vault.md) или [Настройка шифрования с помощью управляемых клиентом ключей, хранящихся в Azure Key Vault управляемый HSM (Предварительная версия)](customer-managed-keys-configure-key-vault-hsm.md).

Обновление ключа для управляемого клиентом ключа не вызывает повторного шифрования данных в учетной записи хранения. От пользователя не требуется предпринимать никаких действий.

> [!NOTE]
> Чтобы повернуть ключ, создайте новую версию ключа в хранилище ключей или управляемом модуле HSM в соответствии с политиками соответствия требованиям. Вы можете вручную сменить ключ или создать функцию, чтобы ее можно было вращать по расписанию.

## <a name="revoke-access-to-customer-managed-keys"></a>Отозвать доступ к ключам, управляемым клиентом

Вы можете в любое время отозвать доступ учетной записи хранения к ключу, управляемому клиентом. После отмены доступа к ключам, управляемым клиентом, или после отключения или удаления ключа клиенты не могут вызывать операции, считывающие или записывающие в большой двоичный объект или его метаданные. Попытки вызова любой из следующих операций завершатся ошибкой с кодом ошибки 403 (запрещено) для всех пользователей:

- [Вывод списка больших двоичных объектов](/rest/api/storageservices/list-blobs)при вызове с `include=metadata` параметром в URI запроса
- [Get BLOB (Получение BLOB-объекта)](/rest/api/storageservices/get-blob)
- [Получение свойств большого двоичного объекта](/rest/api/storageservices/get-blob-properties)
- [Get BLOB Metadata (Получение метаданных BLOB-объекта)](/rest/api/storageservices/get-blob-metadata)
- [Set BLOB Metadata (Задание метаданных BLOB-объекта)](/rest/api/storageservices/set-blob-metadata)
- [Большой двоичный объект моментальных снимков](/rest/api/storageservices/snapshot-blob)при вызове с `x-ms-meta-name` заголовком запроса
- [Копирование BLOB-объекта](/rest/api/storageservices/copy-blob)
- [Копировать большой двоичный объект из URL-адреса](/rest/api/storageservices/copy-blob-from-url)
- [Установка уровня большого двоичного объекта](/rest/api/storageservices/set-blob-tier)
- [Put Block (Вставка блокировки)](/rest/api/storageservices/put-block)
- [Разместить блок по URL-адресу](/rest/api/storageservices/put-block-from-url)
- [Append Block](/rest/api/storageservices/append-block)
- [Добавить блок из URL-адреса](/rest/api/storageservices/append-block-from-url)
- [Put BLOB (Вставка BLOB-объекта)](/rest/api/storageservices/put-blob)
- [Put Page](/rest/api/storageservices/put-page)
- [Размещение страницы по URL-адресу](/rest/api/storageservices/put-page-from-url)
- [Incremental Copy Blob](/rest/api/storageservices/incremental-copy-blob) (инкрементная копия Blob);

Чтобы снова вызвать эти операции, восстановите доступ к ключу, управляемому клиентом.

Все операции с данными, которые не перечислены в этом разделе, могут продолжаться после отзыва ключей, управляемых клиентом, или отключения или удаления ключа.

Чтобы отозвать доступ к ключам, управляемым клиентом, используйте [PowerShell](./customer-managed-keys-configure-key-vault.md#revoke-customer-managed-keys) или [Azure CLI](./customer-managed-keys-configure-key-vault.md#revoke-customer-managed-keys).

## <a name="customer-managed-keys-for-azure-managed-disks"></a>Управляемые клиентом ключи для управляемых дисков Azure

Ключи, управляемые клиентом, также доступны для управления шифрованием управляемых дисков Azure. Управляемые клиентом ключи ведут себя иначе для управляемых дисков, чем для ресурсов службы хранилища Azure. Дополнительные сведения см. в статье [Шифрование управляемых дисков Azure](../../virtual-machines/windows/disk-encryption.md) на стороне сервера для Windows или [Шифрование на стороне сервера для управляемых дисков Azure](../../virtual-machines/linux/disk-encryption.md) для Linux.

## <a name="next-steps"></a>Дальнейшие действия

- [Шифрование службы хранилища Azure для неактивных данных](storage-service-encryption.md)
- [Настройка шифрования с помощью управляемых клиентом ключей, хранящихся в Azure Key Vault](customer-managed-keys-configure-key-vault.md)
- [Настройка шифрования с помощью управляемых клиентом ключей, хранящихся в Azure Key Vault управляемом HSM (Предварительная версия)](customer-managed-keys-configure-key-vault-hsm.md)
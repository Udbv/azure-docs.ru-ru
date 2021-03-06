---
title: Обновление основной версии — портал Azure — база данных Azure для MySQL — один сервер
description: В этой статье описывается, как можно обновить основную версию базы данных Azure для MySQL — одиночный сервер с помощью портал Azure
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 11/16/2020
ms.openlocfilehash: 080d7c09b180d5943216793119718eb6a2add79e
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95753057"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server-using-the-azure-portal"></a>Обновление основной версии в базе данных Azure для MySQL на одном сервере с помощью портал Azure

> [!IMPORTANT]
> Обновление основной версии для одного сервера базы данных Azure для MySQL доступно в общедоступной предварительной версии.

В этой статье описывается, как можно обновить основную версию MySQL на месте в базе данных Azure для MySQL Single Server.

Эта функция позволит клиентам выполнять обновление на месте серверов MySQL 5,6 до MySQL 5,7 с помощью кнопки без перемещения данных или изменения строки подключения приложения.

> [!Note]
> * Обновление основной версии доступно только для обновления основной версии с MySQL 5,6 до MySQL 5,7<br>
> * Обновление основной версии на сервере реплики пока не поддерживается.
> * Сервер будет недоступен во время операции обновления. Поэтому рекомендуется выполнять обновления во время планового периода обслуживания.

## <a name="prerequisites"></a>Предварительные требования
Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:
- [Один сервер базы данных Azure для MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57"></a>Выполнение обновления основной версии с MySQL 5,6 до MySQL 5,7

Выполните следующие действия, чтобы выполнить обновление основной версии для базы данных Azure сервера MySQL 5,6.

> [!IMPORTANT]
> Рекомендуется сначала выполнить обновление на восстановленной копии сервера, а не напрямую обновлять рабочую среду. См. раздел [как выполнить восстановление до точки во времени](howto-restore-server-portal.md#point-in-time-restore). 

1. В [портал Azure](https://portal.azure.com/)выберите существующий сервер базы данных Azure для MySQL 5,6.

2. На странице **Обзор** нажмите кнопку **Обновить** на панели инструментов.

3. В разделе **Обновление** нажмите кнопку **ОК** , чтобы обновить сервер базы данных Azure для MySQL 5,6 на сервер 5,7.

    :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="Обзор базы данных Azure для MySQL — обновление":::

4. В уведомлении будет подтверждено, что обновление прошло успешно.

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

**1. когда эта функция обновления будет общедоступна, так как у нас есть MySQL v 5.6 в производственной среде, которую необходимо обновить?**

Общедоступная версия этой функции планируется до выхода из MySQL v 5.6. Однако эта функция готова к работе и полностью поддерживается Azure, поэтому ее следует использовать в вашей среде с уверенностью. Рекомендуется сначала выполнить и протестировать его на восстановленной копии сервера, чтобы оценить время простоя и выполнить проверку совместимости приложений перед запуском в рабочей среде. Дополнительные сведения см. в статье [как выполнить восстановление](howto-restore-server-portal.md#point-in-time-restore) на момент времени, чтобы создать копию сервера на момент времени. 

**2. это приведет к простою сервера и, если да, сколько времени?**

Да, сервер будет недоступен в процессе обновления, поэтому мы рекомендуем выполнить эту операцию во время планового периода обслуживания. Предполагаемое время простоя зависит от размера базы данных, подготовленного объема хранилища (количество операций ввода-вывода в секунду) и количества таблиц в базе данных. Время обновления прямо пропорционально количеству таблиц на сервере. Обновление основных серверов SKU должно занять больше времени, чем на стандартной платформе хранения. Чтобы оценить время простоя для серверной среды, рекомендуется сначала выполнить обновление для восстановленной копии сервера.  

**3. отмечено, что он еще не поддерживается на сервере реплики. Что означает конкретное?**

В настоящее время обновление основной версии не поддерживается для сервера реплики. Это означает, что не следует запускать его для серверов, участвующих в репликации (на исходном или сервере-реплике). Если вы хотите протестировать обновление серверов, участвующих в репликации, прежде чем добавить поддержку реплики для обновления, рекомендуется выполнить следующие действия:

1. Во время запланированного обслуживания следует [отключить репликацию и удалить сервер реплики](howto-read-replicas-portal.md) после записи его имени и всех сведений о конфигурации (параметры брандмауэра, конфигурации параметров сервера, если они отличаются от исходного сервера).
2. Выполните обновление исходного сервера.
3. Подготавливает новый сервер реплики чтения с тем же именем и параметрами конфигурации, которые были сохранены на шаге 1. Новый сервер реплики будет на v 5.7 автоматически после обновления исходного сервера до версии 5.7.

**4. что произойдет, если не обновить сервер MySQL v 5.6 до 5 февраля 2021?**

Вы по-прежнему можете продолжить работу сервера MySQL v 5.6, как и раньше. Azure **никогда не будет** выполнять принудительное обновление на сервере. Однако будут применены ограничения, описанные в статье [Политика управления версиями базы данных Azure для MySQL](concepts-version-policy.md) .

## <a name="next-steps"></a>Следующие шаги

Сведения о [политике управления версиями базы данных Azure для MySQL](concepts-version-policy.md).

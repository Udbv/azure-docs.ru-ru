---
title: включить файл
description: включить файл
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 08/13/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 3221c4f3e196cf1573bd7c0424fa3b4530c0b2ca
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "90708298"
---
## <a name="create-azure-context"></a>Создание контекста Azure

Чтобы использовать команды Docker для запуска контейнеров в службе "Экземпляры контейнеров Azure", сначала войдите в Azure:

```bash
docker login azure
```

При появлении запроса введите или выберите свои учетные данные Azure.


Создайте контекст ACI, выполнив `docker context create aci`. Этот контекст связывает Docker с подпиской Azure и группой ресурсов, чтобы вы могли создавать экземпляры контейнеров и управлять ими. Например, чтобы создать контекст с именем *myacicontext*:

```
docker context create aci myacicontext
```

При появлении запроса выберите идентификатор подписки Azure, а затем существующую группу ресурсов или **создайте новую**. Если вы выбираете создание новой группы ресурсов, ее имя будет задано системой. Экземпляры контейнеров Azure, как и все ресурсы Azure, должны быть развернуты в группе ресурсов. Группы ресурсов позволяют организовать соответствующие ресурсы Azure и управлять ими.


Запустите `docker context ls`, чтобы убедиться, что вы добавили контекст ACI в контексты Docker:

```
docker context ls
```
---
title: Настройка Синхронизация файлов Azure
description: Настройте Синхронизация файлов Azure. Общий текстовый блок, совместно используемый в документах миграции.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 64b99976a306c3c8423f5115c95a15158a3ddb51
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043164"
---
На этом шаге вместе все ресурсы и папки, настроенные на экземпляре Windows Server, будут объединены в ходе выполнения предыдущих действий.

1. Войдите на [портал Azure](https://portal.azure.com).
1. Нахождение ресурса службы синхронизации хранилища.
1. Создайте новую *группу синхронизации* в ресурсе службы синхронизации хранилища для каждой общей папки Azure. В Синхронизация файлов Azure терминологии файловый ресурс Azure станет *облачной конечной точкой* в топологии синхронизации, которую вы описываете при создании группы синхронизации. При создании группы синхронизации присвойте ей привычное имя, чтобы определить, какой набор файлов будет синхронизироваться здесь. Убедитесь, что вы ссылаетесь на файловый ресурс Azure с совпадающим именем.
1. После создания группы синхронизации в списке групп синхронизации появится строка для нее. Выберите имя (ссылку), чтобы отобразить содержимое группы синхронизации. Файловый ресурс Azure будет отображаться в **облачных конечных точках** .
1. Откройте кнопку **+ Добавить конечную точку сервера** . Папка на локальном сервере, подготовленная для подготовки, станет путем к этой *конечной точке сервера* .

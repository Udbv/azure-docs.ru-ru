---
title: Показано заданий azcopy | Документация Майкрософт
description: В этой статье содержатся справочные сведения по команде azcopy jobs.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 7ffb16936ec96f30ad7d8e05babbadb85e01a71e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "87281945"
---
# <a name="azcopy-jobs-show"></a>azcopy jobs show

Отображает подробные сведения для данного идентификатора задания.

## <a name="synopsis"></a>Краткий обзор

Если только идентификатор задания предоставляется без флага, возвращается сводка о ходе выполнения задания.

Счетчики байтов и процент завершения, отображаемые при выполнении этой команды, отображают только те файлы, которые были завершены в задании. Они не отображают частично завершенные файлы.

Если установлен `with-status` флаг, будет показан список перемещений в задании с заданным значением.

```azcopy
azcopy jobs show [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>Связанные концептуальные статьи

- [Начало работы с AzCopy](storage-use-azcopy-v10.md)
- [Перенос данных с помощью AzCopy и хранилища BLOB-объектов](storage-use-azcopy-blobs.md)
- [Перенос данных с помощью AzCopy и хранилища файлов](storage-use-azcopy-files.md)
- [Configure, optimize, and troubleshoot AzCopy](storage-use-azcopy-configure.md) (Настройка, оптимизация и устранение неполадок с AzCopy)

## <a name="options"></a>Параметры

|Параметр|Описание|
|--|--|
|-h, --help|Отображает содержимое справки для команды Показать.|
|--with-status, строка|Перечислить только передачи заданий с этим состоянием, доступные значения: запущено, успешно, сбой|

## <a name="options-inherited-from-parent-commands"></a>Параметры, унаследованные от родительских команд

|Параметр|Описание|
|---|---|
|--Cap-Мбит/с, с плавающей запятой|Скорость передачи с прописными буквами в мегабит в секунду. Посекундная пропускная способность может немного отличаться от ограничения. Если этот параметр имеет значение 0 или пропущен, пропускная способность не ограничена.|
|--строка выходного типа|Формат вывода команды. Среди вариантов: Text, JSON. Значение по умолчанию — "Text".|
|--Trusted-Microsoft-суффикс строка   |Указывает дополнительные суффиксы домена, в которых могут быть отправлены Azure Active Directory токены входа.  Значение по умолчанию — "*. Core.Windows.NET;*". core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net ". Все перечисленные здесь значения добавляются к значениям по умолчанию. В целях безопасности следует размещать только Microsoft Azureные домены. Несколько записей разделяются точкой с запятой.|

## <a name="see-also"></a>См. также

- [azcopy jobs](storage-ref-azcopy-jobs.md)

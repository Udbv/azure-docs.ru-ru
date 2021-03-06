---
title: Соглашения об именовании для размеров виртуальных машин Azure
description: Описание соглашений об именовании, используемых для размеров виртуальных машин Azure.
ms.service: virtual-machines
subservice: sizes
author: mimckitt
ms.topic: conceptual
ms.date: 7/22/2020
ms.author: mimckitt
ms.custom: sttsinar
ms.openlocfilehash: 13894e534dc8d6dd89baf75ea2bd3b6500b718f7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "88650967"
---
# <a name="azure-virtual-machine-sizes-naming-conventions"></a>Соглашения об именовании размеров виртуальных машин Azure

На этой странице описаны соглашения об именовании, используемые для виртуальных машин Azure. Эти соглашения об именовании используются виртуальными машинами для обозначения различных функций и спецификаций.

## <a name="naming-convention-explanation"></a>Описание соглашения об именовании

**[Family]**  +  **[Подсемейство]**  +  **[число виртуальных ЦП]**  +  **[Аддитивные функции]**  +  **[Тип ускорителя *]**  +  **[Версия]**

|Значение | Объяснение|
|---|---|
| Family | Указывает серию семейств виртуальных машин| 
| * Подсемейство | Используется только для специальных различий виртуальной машины|
| число виртуальных ЦП| Обозначает количество виртуальных ЦП виртуальной машины. |
| Аддитивные функции | Одна или несколько строчных букв обозначают Аддитивные функции, такие как: <br> a = процессор на основе AMD <br> d = диск (локальный временный диск); Дополнительные сведения о новых виртуальных машинах Azure см. в статье [серии Ddv4 и Ddsv4](./ddv4-ddsv4-series.md) . <br> h = с поддержкой спящего режима <br> i = изолированный размер <br> l = недостаточно памяти; меньший объем памяти по сравнению с размером, интенсивно используемым в памяти <br> m = интенсивное потребление памяти; наибольший объем памяти в определенном размере <br> t — небольшая память; Минимальный объем памяти в определенном размере <br> r = с поддержкой RDMA <br> s = поддерживается хранилище класса Premium, включая возможное использование [SSD (цен. Категория "ультра")](./disks-types.md#ultra-disk) (Примечание. Некоторые новые размеры без атрибута s могут по-прежнему поддерживать хранилище класса Premium, например M128, M64 и т. д.).<br> |
| * Тип ускорителя | Обозначает тип аппаратного ускорителя в специализированных номерах или SKU GPU. Только новые специализированные SKU или номера GPU, запущенные из версии Q3 2020, будут иметь ускоритель оборудования в имени. |
| Версия | Обозначает версию серии семейств виртуальных машин |

## <a name="example-breakdown"></a>Пример разбивки

**[Family]**  +  **[Подсемейство]**  +  **[число виртуальных ЦП]**  +  **[Аддитивные функции]**  +  **[Тип ускорителя *]**  +  **[Версия]**

### <a name="example-1-m416ms_v2"></a>Пример 1. M416ms_v2

|Значение | Объяснение|
|---|---|
| Family | M | 
| число виртуальных ЦП | 416 |
| Аддитивные функции | m = интенсивное потребление памяти <br> s = поддерживается хранилище класса Premium |
| Версия | Версия 2 |

### <a name="example-2-nv16as_v4"></a>Пример 2. NV16as_v4

|Значение | Объяснение|
|---|---|
| Family | Нет | 
| Подсемейство | V |
| число виртуальных ЦП | 16 |
| Аддитивные функции | a = процессор на основе AMD <br> s = поддерживается хранилище класса Premium |
| Версия | версия 4 |

### <a name="example-3-nc4as_t4_v3"></a>Пример 3. NC4as_T4_v3

|Значение | Объяснение|
|---|---|
| Family | Нет | 
| Подсемейство | C |
| число виртуальных ЦП | 4 |
| Аддитивные функции | a = процессор на основе AMD <br> s = поддерживается хранилище класса Premium |
| Тип ускорителя | T4 |
| Версия | Версия 3 |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о доступных [размерах виртуальных машин](./sizes.md) в Azure см. здесь. 
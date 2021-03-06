---
title: включить файл
description: включить файл
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/24/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b660d3a0d49de80ed85cfbdcdf8e28b9828cbf26
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91544862"
---
- Поддерживаются только [ключи RSA для программного обеспечения и HSM](../articles/key-vault/keys/about-keys.md) с размерами 2 048-бит, 3 072-бит и 4 096-bit, без других ключей или размеров.
    - Для ключей [HSM](../articles/key-vault/keys/hsm-protected-keys.md) требуется уровень **Premium** хранилища ключей Azure.
- Диски, созданные на основе пользовательских образов, зашифрованных с помощью шифрования на стороне сервера и ключей, управляемых клиентом, должны быть зашифрованы с помощью тех же самых управляемых клиентом ключей и должны находиться в одной подписке.
- Моментальные снимки дисков, зашифрованных с помощью шифрования на стороне сервера и ключей, управляемых клиентом, должны быть зашифрованы с помощью тех же управляемых клиентом ключей.
- Все ресурсы, связанные с ключами, управляемыми клиентом (хранилища Azure Key Vault, наборы шифрования дисков, виртуальные машины, диски и моментальные снимки), должны находиться в одной подписке и регионе.
- Диски, моментальные снимки и образы, зашифрованные с помощью управляемых клиентом ключей, не могут перемещаться в другую группу ресурсов и подписку.
- Управляемые диски, которые сейчас или ранее были зашифрованы с помощью шифрования дисков Azure, не могут быть зашифрованы с помощью управляемых клиентом ключей.
- Можно создать до 50 наборов шифрования дисков на регион для каждой подписки.
- Сведения об использовании управляемых клиентом ключей с общими коллекциями образов см. в разделе [Предварительная версия: использование управляемых клиентом ключей для шифрования образов](../articles/virtual-machines/image-version-encryption.md).

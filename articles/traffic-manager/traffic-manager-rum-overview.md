---
title: Измерения на стороне пользователей в диспетчере трафика Azure
description: В этом обзоре вы узнаете, как работает диспетчер трафика Azure Измерения на стороне пользователей.
services: traffic-manager
documentationcenter: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: duau
ms.custom: ''
ms.openlocfilehash: 45c889ef8d67570fb075ec710616f20d110b7f50
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "89401611"
---
# <a name="traffic-manager-real-user-measurements-overview"></a>Общие сведения об измерениях на стороне пользователей в диспетчере трафика

Когда вы настраиваете в профиле диспетчера трафика использование метода маршрутизации для повышения производительности, служба анализирует, откуда исходят запросы DNS, а затем принимает решения о маршрутизации, чтобы направить эти запрашивающие стороны в регион Azure, обеспечивающий для них минимальную задержку. Это достигается за счет использования аналитики задержки сети, которую диспетчер трафика поддерживает для различных сетей конечных пользователей.

Измерения на стороне пользователей позволяют измерить задержку сети для регионов Azure из клиентских приложений, используемых конечными пользователями, а диспетчер трафика учитывает эти сведения при принятии решений о маршрутизации. Если вы выберите функцию "Измерения на стороне пользователей", вы сможете повысить точность маршрутизации для запросов, исходящих из сетей, где расположены конечные пользователи. 

## <a name="how-real-user-measurements-work"></a>Как работают измерения на стороне пользователей

Измерения на стороне пользователей работают следующим образом: измеряется задержка при подключении клиентских приложений к регионам Azure из сетей конечных пользователей, в которых они используются. Например, если у вас есть веб-страница, доступная для пользователей из различных мест (например, в регионах Северной Америки), для повышения производительности можно применить функцию Измерения на стороне пользователей при использовании метода маршрутизации, чтобы направить их в наиболее оптимальный регион Azure, где размещается серверное приложение.

Запуск начинается с внедрения кода JavaScript, предоставляемого Azure (и который содержит уникальный ключ), на веб-страницах. После этого каждый раз, когда пользователь посещает эту веб-страницу, JavaScript отправляет запрос диспетчеру трафика, чтобы получить сведения о регионах Azure, где необходимо выполнить измерения. Служба возвращает набор конечных точек в сценарий, с помощью которого затем последовательно выполняются измерения в регионах: загружается изображение размером в один пиксель, размещенное в этих регионах Azure, и отслеживается задержка между временем отправки запроса и временем получения первого байта. Сведения об этих измерениях затем возвращаются в службу диспетчера трафика.

Эта процедура повторяется для большого количества сетей, что дает возможность диспетчеру трафика получить более точные сведения о характеристиках задержки сетей, в которых расположены конечные пользователи. Эти сведения играют определенную роль при принятии решений о маршрутизации диспетчером трафика. Таким образом на основе отправленных измерений на стороне пользователей можно добиться повышения точности маршрутизации.

При использовании этой функции счета выставляются в зависимости от количества измерений, отправленных диспетчеру трафика. Дополнительные сведения о ценах см. на [странице цен на диспетчер трафика](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faqs"></a>Часто задаваемые вопросы

* [Каковы преимущества при использовании реальных измерений пользователя?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#what-are-the-benefits-of-using-real-user-measurements)

* [Можно ли использовать реальные измерения пользователя для регионов, отличных от регионов Azure?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-real-user-measurements-with-non-azure-regions)

* [Какие методы маршрутизации могут лучше работать с реальными измерениями пользователя?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#which-routing-method-benefits-from-real-user-measurements)

* [Нужно ли включать реальные измерения пользователя для каждого профиля отдельно?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#do-i-need-to-enable-real-user-measurements-each-profile-separately)

* [Как отключить реальные измерения пользователя для подписки?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-do-i-turn-off-real-user-measurements-for-my-subscription)

* [Можно ли использовать реальные измерения пользователя в других клиентских приложениях, кроме веб-страниц?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-real-user-measurements-with-client-applications-other-than-web-pages)

* [Сколько измерений выполняется при каждом отображении страницы, для которой включены реальные измерения пользователя?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered)

* [Существует ли задержка перед запуском скрипта реальных измерений пользователя на веб-странице?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage)

* [Можно ли настроить измерения на стороне пользователя так, чтобы они выполнялись только для нужных мне регионов Azure?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-use-real-user-measurements-with-only-the-azure-regions-i-want-to-measure)

* [Можно ли ограничить число измерений?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-limit-the-number-of-measurements-made-to-a-specific-number)

* [Можно ли увидеть измерения, которые выполняет клиентское приложение в рамках реальных измерений пользователя?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements)

* [Можно ли изменить скрипт измерения, полученный из диспетчера трафика?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-i-modify-the-measurement-script-provided-by-traffic-manager)

* [Могут ли другие лица увидеть ключ, который я использую для реальных измерений пользователя?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements)

* [Могут ли злоумышленники использовать ключ RUM?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-others-abuse-my-rum-key)

* [Нужно ли размещать скрипт измерений JavaScript на всех веб-страницах?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages)

* [Может ли диспетчер трафика получить сведения, идентифицирующие моих пользователей, когда я использую реальные измерения пользователя?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements)

* [Обязательно ли использовать маршрутизацию диспетчера трафика для веб-страниц, на которых выполняются реальные измерения пользователя?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing)

* [Нужно ли размещать службы в регионах Azure, чтобы использовать реальные измерения пользователя?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements)

* [Увеличится ли мой трафик в Azure при использовании реальных измерений пользователя?](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs#will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements)

## <a name="next-steps"></a>Дальнейшие шаги
- Узнайте об использовании [измерений на стороне пользователей с веб-страницами](traffic-manager-create-rum-web-pages.md).
- Узнайте о том, [как работает диспетчер трафика](traffic-manager-overview.md)
- Дополнительные сведения о Mobile Center см. [здесь](https://docs.microsoft.com/mobile-center/).
- Узнайте больше о [методах маршрутизации трафика](traffic-manager-routing-methods.md) , поддерживаемых в диспетчере трафика.
- Узнайте, как [создать профиль диспетчера трафика](traffic-manager-create-profile.md)


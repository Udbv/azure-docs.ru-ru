---
title: Панель мониторинга Marketplace Insights в коммерческой аналитике Marketplace
description: Получите доступ к сводке веб-аналитики Marketplace в центре партнеров, которая позволяет измерять участие клиентов в Microsoft AppSource и Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 11/09/2020
author: sayantanroy83
ms.author: sroy
ms.openlocfilehash: 8f85e9c77cc6fed7e2763f694664332b124d0780
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2020
ms.locfileid: "94561800"
---
# <a name="marketplace-insights-dashboard-in-commercial-marketplace-analytics"></a>Панель мониторинга Marketplace Insights в коммерческой аналитике Marketplace

Эта статья содержит сведения о панели мониторинга "Аналитика Marketplace" в Центре партнеров. На этой панели мониторинга отображается сводка веб-аналитики коммерческого рынка, которая позволяет издателям измерять взаимодействия клиентов с соответствующими страницами сведений о продуктах, перечисленными в Интернет-магазинах коммерческого рынка: Microsoft AppSource и Azure Marketplace.

Чтобы получить доступ к панели мониторинга **Marketplace Insights** в центре партнеров, в разделе коммерческий Marketplace выберите **[анализ](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)**  >  **Marketplace Insights**.

Подробное описание терминологии аналитики см. в статье [терминология анализа коммерческих рынков и распространенные вопросы](./partner-center-portal/faq-terminology.md).

## <a name="marketplace-insights-dashboard"></a>Панель мониторинга аналитики Marketplace

На информационной панели Marketplace Insights представлены общие сведения о производительности Azure Marketplace и AppSource. Эта панель мониторинга предоставляет широкий обзор следующих элементов:

- Тренд посещений страниц
- Тренд вызова действий
- Посещения страниц и вызовы действий по предложениям, доменам ссылок и идентификаторам кампаний
- Аналитика Marketplace по географическому представлению
- Подробная таблица аналитики Marketplace

Информационная панель Marketplace Insights предоставляет данные навигации, которые не должны быть связаны с интересами, созданными в конечной точке назначения интереса.

> [!NOTE]
> Максимальная задержка между пользователями, посещаемыми в Azure Marketplace или AppSource, и отчетами в центре партнеров составляет 48 часов.

## <a name="elements-of-the-marketplace-insights-dashboard"></a>Элементы панели мониторинга Marketplace Insights

На панели мониторинга "аналитика Marketplace" отображаются сведения о веб-телеметрии для Azure Marketplace и AppSource на двух отдельных вкладках. В следующих разделах описывается, как использовать панель мониторинга Marketplace Insights и как считывать данные.

### <a name="month-range"></a>Диапазон месяцев

Выбор диапазона месяца можно найти в правом верхнем углу каждой страницы. Настройте выходные данные графов страниц **Marketplace Insights** , выбрав диапазон месяцев на основе последних 6 или 12 месяцев или выбрав диапазон настраиваемых месяцев с максимальной длительностью 12 месяцев. Диапазон месяцев по умолчанию (расчетный период) — 6 месяцев.

:::image type="content" source="./media/insights-dashboard/month-filters.png" alt-text="Показаны фильтры месяцев на панели мониторинга Marketplace Insights.":::

> [!NOTE]
> Все метрики в мини-приложениях визуализации и отчетах экспорта учитывают период вычислений, выбранный пользователем.

### <a name="page-visits-trends"></a>Тенденции посещения страницы

На диаграмме **посетителей** Marketplace Insights отображается число _посещений страниц_ и _уникальных посетителей_ за выбранный период вычислений.

**Посещения страниц** : это число, представляющее количество отдельных пользовательских сеансов на странице со списком предложений (страница сведений о продукте) за выбранный период вычислений. Процентные индикаторы красного и зеленого цвета представляют процент роста для посещения страниц. Диаграмма трендов представляет число посещений страниц по месяцам.

**Уникальные посетители**. это число представляет отдельный счетчик посетителей за выбранный период вычислений для предложений в Azure Marketplace и AppSource. Посетитель, который посещает одну или несколько страниц сведений о продукте, считается одним уникальным посетителем.

[![Демонстрирует диаграмму посетителей на панели мониторинга Marketplace Insights.](./media/insights-dashboard/visitors.png)](./media/insights-dashboard/visitors.png#lightbox)

### <a name="call-to-actions-trend"></a>Тренд вызова действий

Это число представляет число вызовов нажатий кнопок **действий** , выполненных на странице со списком предложений (страница сведений о продукте). _Вызовы действия_ подсчитываются, когда пользователь выбирает кнопки **получить сейчас** , **Бесплатная пробная версия** , **связаться со мной** или **тестовый диск** .

[![Иллюстрируется вызов диаграммы действий на панели мониторинга Marketplace Insights.](./media/insights-dashboard/call-to-actions-trend.png)](./media/insights-dashboard/call-to-actions-trend.png#lightbox)

### <a name="page-visits-and-call-to-actions-against-offers-referral-domains-and-campaign-ids"></a>Посещения страниц и вызовы действий по предложениям, доменам ссылок и идентификаторам кампаний

**Домены ссылок**. Выбор конкретного домена ссылок показывает месячную тенденцию посещения страниц и вызовов действий на диаграмме вправо.

:::image type="content" source="./media/insights-dashboard/referral-domain.png" alt-text="Иллюстрируется диаграмма домена ссылок на панели мониторинга Marketplace Insights.":::

**Предложения**. Выберите конкретное предложение, чтобы просмотреть месячную тенденцию посещения страниц и обращения к действию на диаграмме справа.

:::image type="content" source="./media/insights-dashboard/offer-alias.png" alt-text="Демонстрирует диаграмму псевдонима предложения на панели мониторинга Marketplace Insights.":::

**Идентификаторы кампаний** : ВЫБРАВ конкретный идентификатор кампании, вы сможете понять успешность кампании. Для каждой кампании вы должны видеть ежемесячную тенденцию посещения страниц и вызовы действий на диаграмме справа.

:::image type="content" source="./media/insights-dashboard/campaign.png" alt-text="Демонстрирует диаграмму кампании на панели мониторинга Marketplace Insights.":::

### <a name="marketplace-insights-by-geography"></a>Аналитика Marketplace по географическому представлению

Для выбранного периода вычислений тепловой карты отображает количество посещений страниц, уникальных посетителей и вызовов действия (ПНА). Светло-темное цветовое на карте представляет собой низкое и высокое значение уникальных посетителей. Выберите запись в таблице, чтобы увеличить страну или регион.

:::image type="content" source="./media/insights-dashboard/geographical-spread.png" alt-text="Иллюстрация географического разворота на панели мониторинга Marketplace Insights.":::

Следует отметить следующее.

- Чтобы увидеть точное расположение, можно переместить карту.
- Также можно приблизить конкретное расположение.
- Тепловой карты имеет дополнительную сетку для просмотра сведений о количестве клиентов, количестве заказов и нормализованных часах использования в определенном месте.
- Вы можете найти и выбрать страну в таблице, чтобы приблизить соответствующее расположение на карте. Вернитесь к исходному представлению, нажав кнопку **Главная** на карте.

### <a name="marketplace-insights-details-table"></a>Подробная таблица аналитики Marketplace

В этой таблице представлен список посещений страниц и вызовов действий с выбранными страницами предложений, отсортированных по датам.

- Данные могут быть извлечены в. TSV или. CSV-файл, если число записей меньше 1 000.
- Если число записей превышает 1 000, экспортированные данные будут асинхронно помещены на страницу загрузки в течение следующих 30 дней.
- Отфильтруйте данные по именам предложений и названиям кампаний, чтобы отобразить нужные данные.

> [!TIP]
> Для загрузки данных можно использовать значок скачивания в правом верхнем углу любого мини-приложения. Вы можете отправить отзыв о каждом из мини-приложений, щелкнув значок "палец Up" или "палец вниз".

## <a name="next-steps"></a>Дальнейшие действия

- Обзор аналитических отчетов, доступных в коммерческом магазине, см. в статье [доступ к аналитикам для коммерческого рынка в центре партнеров](./partner-center-portal/analytics.md).
- Сведения о заказах в графическом и загружаемом формате см. в статье [панель мониторинга заказов в коммерческом Analytics Marketplace](./orders-dashboard.md).
- Для виртуальной машины (VM) предоставляет метрики использования и измерения для выставления счетов. см. раздел [панель мониторинга "использование" в коммерческом Analytics Marketplace](./usage-dashboard.md).
- Подробные сведения о клиентах, в том числе тренды роста, см. в статье [Панель мониторинга клиентов в аналитике коммерческой платформы](./customer-dashboard.md).
- Список запросов на загрузку за последние 30 дней см. в статье [Панель мониторинга загрузок в аналитике коммерческой платформы](./partner-center-portal/downloads-dashboard.md).
- Чтобы просмотреть объединенное представление отзывов клиентов о предложениях в Azure Marketplace и AppSource, см. статью [оценки & рецензий анализ панели мониторинга в центре партнеров](./partner-center-portal/ratings-reviews.md).
- Часто задаваемые вопросы о коммерческой аналитике и подробном словаре терминов данных см. в разделе [терминология и общие вопросы по анализу коммерческих рынков](./partner-center-portal/faq-terminology.md).

---
title: Организация счета на основе ваших потребностей — Azure
description: Узнайте, как упорядочить затраты в счете. Вы можете настроить учетную запись выставления счетов, создав профили выставления счетов и разделы счетов.
author: amberbhargava
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: 6424fc0ff49566fad949b3fba4718acb2bad4cd3
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91362783"
---
# <a name="organize-costs-by-customizing-your-billing-account"></a>Упорядочивание расходов с помощью учетной записи выставления счетов

Учетная запись выставления счетов для Клиентского соглашения Майкрософт предоставляет широкие возможности для организации затрат в зависимости от потребностей отдела, проекта или среды разработки.

В этой статье содержатся сведения об использовании портала Azure для организации затрат. В этой статье рассматривается учетная запись выставления счетов для Клиентского соглашения Майкрософт. [Проверьте наличие доступа к Клиентскому соглашению Майкрософт](#check-access-to-a-microsoft-customer-agreement).

## <a name="structure-your-account-with-billing-profiles-and-invoice-sections"></a>Структурирование учетной записи с помощью профилей выставления счетов и разделов счетов

В учетной записи Клиентского соглашения Майкрософт для организации затрат используются профили выставления счетов и разделы счета.

![Снимок экрана: иерархия выставления счетов с Клиентским соглашением Майкрософт](./media/mca-section-invoice/mca-hierarchy.png)

### <a name="billing-profile"></a>Профиль выставления счетов

Профиль выставления счетов представляет собой счет и связанные с ним сведения о выставлении счетов, в том числе методы оплаты и адрес для выставления счетов. В начале месяца для каждого профиля выставления счетов в вашей учетной записи создается счет за месяц. Счет содержит расходы за использование Azure и другие покупки за предыдущий месяц.

Профиль выставления счетов и учетная запись выставления счетов создаются автоматически при регистрации в Azure. Вы можете создать дополнительные профили выставления счетов для организации затрат в нескольких ежемесячных счетах.

> [!IMPORTANT]
>
> Создание дополнительных профилей выставления счетов может повлиять на общую стоимость. Дополнительные сведения см. в разделе [Things to consider when adding new billing profiles](#things-to-consider-when-adding-new-billing-profiles) (Факторы, которые следует учитывать при добавлении новых профилей выставления счетов).

### <a name="invoice-section"></a>Раздел счета

Раздел счета представляет группирование затрат в счете. Раздел счета создается автоматически для каждого профиля выставления счетов в вашей учетной записи. Вы можете создать дополнительные разделы для организации затрат в соответствии с вашими потребностями. Каждый раздел счета отображается в счете с оплатой за этот месяц.

На рисунке ниже показан счет с двумя разделами — "Инженерия" и "Маркетинг". В счете отображается сводка и подробные сведения по каждому разделу. Цены, показанные на изображении, предназначены только для примера и не представляют фактические цены на службы Azure.

![Изображение, показывающее счет с разделами](./media/mca-section-invoice/mca-invoice-with-sections.png)

## <a name="billing-account-structure-for-common-scenarios"></a>Структура счета выставления счетов для распространенных сценариев

В этом разделе описаны распространенные сценарии для организации затрат и соответствующих структур счетов.

|Сценарий  |Структура  |
|---------|---------|
|Джек регистрируется в Azure и ему нужен один ежемесячный счет. | Профиль выставления счетов и раздел счета. Эта структура автоматически настраивается для Джека при регистрации в Azure и не требует дополнительных действий. |

![Инфографика для простого сценария выставления счетов.](./media/mca-section-invoice/organize-billing-scenario1.png)

|Сценарий  |Структура  |
|---------|---------|
|Contoso — это небольшая организация, которой требуется один месячный счет, в котором затраты группируются по отделам — маркетинг и инженерия.  | Профиль выставления счетов для Contoso и раздел счета для маркетинговых и инженерных отделов. |

![Инфографика для сложного сценария выставления счетов.](./media/mca-section-invoice/organize-billing-scenario2.png)

|Сценарий  |Структура  |
|---------|---------|
|Fabrikam — это организация среднего размера, которой требуются отдельные счета для отделов по инженерии и маркетингу. Для инженерного отдела им нужно сгруппировать затраты по средам — производству и разработке.  | Профиль выставления счетов для отделов маркетинга и инженерии. Для инженерного отдела — раздел счета для рабочей среды разработки и эксплуатации. |

![Инфографика для сложного сценария выставления счетов с раздельным выставлением счетов для рабочей среды и среды разработки.](./media/mca-section-invoice/organize-billing-scenario3.png)

## <a name="create-a-new-invoice-section"></a>Создание нового раздела счета

Чтобы создать раздел счета, вы должны быть **владельцем учетной записи выставления счетов** или **поставщиком учетной записи выставления счетов**. Дополнительные сведения см. в разделе [Управление разделами счетов в учетной записи выставления счетов](understand-mca-roles.md#manage-invoice-sections-for-billing-profile).

1. Войдите на [портал Azure](https://portal.azure.com).

2. Выполните поиск по фразе **Управление затратами + выставление счетов**.

   ![Снимок экрана с изображением поиска на портале по фразе "Управление затратами + выставление счетов"](./media/mca-section-invoice/search-cmb.png)

3. Выберите раздел **Профили выставления счетов** на панели слева. Выберите профиль выставления счетов в списке. Для выбранного профиля выставления счетов буде показан новый раздел.

   [![Снимок экрана: список профилей выставления счетов.](./media/mca-section-invoice/mca-select-profile.png)](./media/mca-section-invoice/mca-select-profile-zoomed-in.png#lightbox)

4. Выберите **Разделы счетов** в левой области, а затем **Добавить** в верхней части страницы.

   [![Снимок экрана, показывающий добавление разделов счета](./media/mca-section-invoice/mca-list-invoice-sections.png)](./media/mca-section-invoice/mca-list-invoice-sections-zoomed-in.png#lightbox)

5. Введите имя раздела счета.

   [![Снимок экрана, показывающий страницу создания разделов счетов](./media/mca-section-invoice/mca-create-invoice-section.png)](./media/mca-section-invoice/mca-create-invoice-section-zoomed-in.png#lightbox)

6. Нажмите кнопку **создания**.

## <a name="create-a-new-billing-profile"></a>Создание нового профиля выставления счетов

Чтобы создать профиль выставления счетов, необходимо быть **владельцем учетной записи выставления счетов** или **участником учетной записи выставления счетов**. Дополнительные сведения см. в разделе [Управление профилями выставления счетов в учетной записи выставления счетов](understand-mca-roles.md#manage-billing-profiles-for-billing-account).

> [!IMPORTANT]
>
> Создание дополнительных профилей выставления счетов может повлиять на общую стоимость. Дополнительные сведения см. в разделе [Things to consider when adding new billing profiles](#things-to-consider-when-adding-new-billing-profiles) (Факторы, которые следует учитывать при добавлении новых профилей выставления счетов).

1. Войдите на [портал Azure](https://portal.azure.com).

2. Выполните поиск по фразе **Управление затратами + выставление счетов**.

   ![Снимок экрана с изображением поиска на портале по фразе "Управление затратами + выставление счетов"](./media/mca-section-invoice/search-cmb.png)

3. Выберите **Профили выставления счетов** на левой панели, а затем **Добавить** в верхней части страницы.

   [![Снимок экрана: список профилей выставления счетов с выбранной кнопкой "Добавить".](./media/mca-section-invoice/mca-list-profiles.png)](./media/mca-section-invoice/mca-list-profiles-zoomed-in.png#lightbox)

    > [!Note]
    >
    > Если кнопка "Добавить" не отображается на странице "Профиль выставления счетов", эта функция недоступна для вашей учетной записи. Сейчас она доступна только для учетных записей, настроенных при работе с представителями корпорации Майкрософт.

4. Заполните форму и щелкните **Создать**.

   [![Снимок экрана, показывающий страницу создания профиля выставления счетов](./media/mca-section-invoice/mca-add-profile.png)](./media/mca-section-invoice/mca-add-profile-zoomed-in.png#lightbox)

    |Поле  |Определение  |
    |---------|---------|
    |Имя     | Отображаемое имя поможет легко найти профиль выставления счетов на портале Azure.  |
    |Номер заказа на покупку    | Необязательный номер заказа на покупку. Номер заказа на покупку будет отображаться в счетах, созданных для профиля выставления счетов. |
    |адрес выставления счетов;   | Адрес для выставления счетов будет отображаться в счетах, созданных для профиля выставления счетов. |
    |Отправить счет по электронной почте   | Установите флажок "счет по электронной почте", чтобы получить счета для этого профиля выставления счетов по электронной почте. Если вы не согласитесь, вы можете просматривать и скачивать счета на портале Azure.|

5. Нажмите кнопку **создания**.

## <a name="link-charges-to-invoice-sections-and-billing-profiles"></a>Связывание расходов с разделами счетов и профилями выставления счетов

После настройки учетной записи выставления счетов на основе ваших потребностей подписки и другие продукты можно связать с нужным разделом счета и профилем выставления счетов.

### <a name="link-a-new-subscription"></a>Связывание новой подписки

1. Войдите на [портал Azure](https://portal.azure.com).

2. Выполните поиск по запросу **Подписки**.

   [![Снимок экрана: поиск подписки на портале Azure.](./media/mca-section-invoice/search-subscriptions.png)](./media/mca-section-invoice/search-subscriptions.png#lightbox)

3. В верхней части страницы выберите **Добавить**.

   ![Снимок экрана: кнопка "Добавить" в представлении "Подписки" для новой подписки.](./media/mca-section-invoice/subscription-add.png)

4. Если у вас есть доступ к нескольким учетным записям выставления счетов, выберите учетную запись выставления счетов Клиентского соглашения Майкрософт.

   ![Снимок экрана: область создания подписки.](./media/mca-section-invoice/mca-create-azure-subscription.png)

5. Выберите профиль выставления счетов, для которого будет выставлен счет за использование подписки. Плата за использование Azure и другие покупки для этой подписки будет взиматься по выбранному счету профиля выставления счетов.

6. Выберите раздел счета, чтобы связать расходы по подписке. Плата будет отображаться в этом разделе в счете профиля выставления счетов.

7. Выберите план Azure и введите понятное имя для своей подписки.

9. Нажмите кнопку **Создать**.  

### <a name="link-existing-subscriptions-and-products"></a>Связывание существующих подписок и продуктов

Если у вас есть подписки Azure или другие продукты, такие как Azure Marketplace и исходные ресурсы приложения, их можно переместить из существующего раздела счета в другой, чтобы реорганизовать затраты.

> [!IMPORTANT]
>
> Подписки и другие продукты можно перемещать только между разделами счетов, которые принадлежат одному профилю выставления счетов. Перемещение подписок и продуктов между разделами счетов в разных профилях выставления счетов не поддерживается.

1. Войдите на [портал Azure](https://portal.azure.com).

2. Выполните поиск по фразе **Управление затратами + выставление счетов**.

   ![Снимок экрана: поиск на портале Azure по фразе "управление затратами + выставление счетов".](./media/mca-section-invoice/search-cmb.png)

3. Чтобы связать подписку с новым разделом счета, выберите **Подписки Azure** в левой части экрана. Для других продуктов, таких как Azure Marketplace и исходных ресурсов приложений, выберите **Повторяющиеся платежи**.

   [![Снимок экрана, на котором показан параметр изменения раздела счета](./media/mca-section-invoice/mca-select-change-invoice-section.png)](./media/mca-section-invoice/mca-select-change-invoice-section.png#lightbox)

4. На странице щелкните многоточие (три точки) для подписки или продукта, которые нужно связать с новым разделом счета. Выберите **Изменение раздела счета**

5. В раскрывающемся списке выберите новый раздел счета. В раскрывающемся списке отображаются только разделы счета, связанные с тем же профилем выставления счетов, что и в существующего раздела счета.

    [![Снимок экрана: выбор нового раздела счета](./media/mca-section-invoice/mca-select-new-invoice-section.png)](./media/mca-section-invoice/mca-select-new-invoice-section-zoomed-in.png#lightbox)

6. Щелкните **Сохранить**.

## <a name="things-to-consider-when-adding-new-billing-profiles"></a>Факторы, которые следует учитывать при добавлении новых профилей выставления счетов

### <a name="azure-usage-charges-may-be-impacted"></a>Могут возникать расходы на использование Azure

В учетной записи выставления счетов для Клиентского соглашения Майкрософт использование Azure вычисляется для каждого профиля выставления счетов на ежемесячной основе. Цены на ресурсы Azure с многоуровневым ценообразованием определяются на основе отдельного использования каждого профиля выставления счетов. При вычислении цены использование не объединяется по профилям выставления счетов. Это может повлиять на общую стоимость использования Azure для учетных записей с несколькими профилями выставления счетов.

Рассмотрим отличие цен для двух сценариев на примере. Цены, используемые в сценариях, предназначены только для примера и не представляют фактические цены на службы Azure.

#### <a name="you-only-have-one-billing-profile"></a>У вас есть только один профиль выставления счетов.

Предположим, что вы используете хранилище блочных BLOB-объектов Azure, стоимость которого составляет .00184 долларов США за ГБ для первых 50 терабайт (ТБ), а затем .00177 за ГБ на следующие 450 терабайт (ТБ). Вы использовали 100 ТБ в подписках, счет за которые выставляется в профиле выставления счетов. В таком случае плата будем взиматься, как показано ниже.

|  Многоуровневое ценообразование (долл. США) |Количество | Сумма (дол. США)|
|---------|---------|---------|
|1,84 за ТБ за первые 50 ТБ в месяц    | 50 ТБ        | 92,0   |
|1,77 за ТБ на следующие 450 ТБ в месяц    |  50 ТБ         | 88,5   |
|Итог     |     100 ТБ  | 180,5

Общая плата за использование 100 ТБ данных в этом сценарии составляет **180,5**

#### <a name="you-have-multiple-billing-profiles"></a>У вас есть несколько профилей выставления счетов.

Теперь предположим, что вы создали еще один профиль выставления счетов и использовали 50 Гб через подписки, счета за которые выставляются в первый профиль выставления счетов, а 50 Гб — через подписки, счета за которые выставляются во второй профиль выставления счетов.

`Charges for the first billing profile`

|  Многоуровневое ценообразование (долл. США) |Количество | Сумма (дол. США)|
|---------|---------|---------|
|1,84 за ТБ за первые 50 ТБ в месяц    | 50 ТБ        | 92,0  |
|1,77 за ТБ на следующие 450 ТБ в месяц    |  0 TБ         | 0,0  |
|Итог     |     50 ТБ  | 92,0

`Charges for the second billing profile`

|  Многоуровневое ценообразование (долл. США) |Количество | Сумма (дол. США)|
|---------|---------|---------|
|1,84 за ТБ за первые 50 ТБ в месяц    | 50 ТБ        | 92,0  |
|1,77 за ТБ на следующие 450 ТБ в месяц    |  0 TБ         | 0,0  |
|Итог     |     50 ТБ  | 92,0

Общая плата за использование 100 ТБ данных в этом сценарии составляет **184,0** (92,0 * 2).

### <a name="azure-reservation-benefits-might-not-apply-to-all-subscriptions"></a>Преимущества резервирования Azure могут не применяться ко всем подпискам

Резервирования Azure с совместно используемой областью применяются к подпискам в одном профиле выставления счетов и не являются общими для профилей выставления счетов.

![Изображение сведений о приложении резервирования для разных структур счетов](./media/mca-section-invoice/mca-reservations-benefits-by-bg.png)

На приведенном выше рисунке в компании Contoso есть две подписки. Преимущество резервирования Azure применяется по-разному в зависимости от структуры счета выставления счетов. В сценарии, расположенном слева, преимущество резервирования применяется к обеим подпискам, счета за которые выставляются в профиле выставления счетов. В сценарии, расположенном справа, преимущество резервирования будет применено только к подписке 1, так как это единственная подписка, счет за которую выставляется для профиля выставления счетов отдела инженерии.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Проверка доступа к Клиентскому соглашению Майкрософт
[!INCLUDE [billing-check-mca](../../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Требуется помощь? Обращение в службу поддержки

Если вам нужна помощь, [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), которая поможет быстро устранить проблему.

## <a name="next-steps"></a>Дальнейшие действия

- [Создание дополнительной подписки Azure для клиентского соглашения Майкрософт](create-subscription.md)
- [Управление ролями выставления счетов на портале Azure](understand-mca-roles.md#manage-billing-roles-in-the-azure-portal)
- [Получение запроса права владения на выставление счетов в подписках от пользователей в других учетных записях выставления счетов](mca-request-billing-ownership.md)

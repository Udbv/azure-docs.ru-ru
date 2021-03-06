---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: e3bff61cfbf89aee3566d677ccf593b102cff36d
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375850"
---
#### <a name="to-add-a-storsimple-backup-policy"></a>Добавление политики резервного копирования StorSimple

1. Перейдите к устройству StorSimple и щелкните **Политика архивации**.

2. В колонке **Политика архивации** на панели команд щелкните **Добавить политику**.
   
    ![Добавить политику резервного копирования](./media/storsimple-8000-add-backup-policy-u2/addbupol1.png)

3. В колонке **Создать политику архивации** выполните следующие действия:
   
   1. Значение **Выбор устройства** автоматически заполняется в зависимости от выбранного устройства.
   
   2. Укажите **имя политики** архивации, содержащее от 3 до 150 символов. После создания политики ее нельзя переименовать.
       
   3. Чтобы назначить тома для этой политики архивации, щелкните **Добавить тома** , а затем в табличном списке томов установите флажки напротив одного или нескольких томов.

       ![Добавление политики архивации 2](./media/storsimple-8000-add-backup-policy-u2/addbupol2.png)

   4. Чтобы определить расписание для этой политики архивации, щелкните **Первое расписание** , а затем измените следующие параметры:

       ![Добавление политики архивации 3](./media/storsimple-8000-add-backup-policy-u2/addbupol3.png)

       1. В качестве **типа моментального снимка** выберите **Облако** или **Локально**.

       2. Укажите частоту резервного копирования (укажите число, а затем выберите **дни** или **недели** из раскрывающегося списка).

       3. Укажите расписание хранения.

       4. Выберите время и дату начала применения политики архивации.

       5. Нажмите кнопку **ОК** , чтобы задать расписание.

   5. Нажмите кнопку **Создать** , чтобы создать политику архивации.

       ![Добавление политики архивации 4](./media/storsimple-8000-add-backup-policy-u2/addbupol4.png)
   
   6. Вы получите уведомление, когда политика архивации будет создана. Добавленная политика появится в таблице в колонке **Политика архивации**.

       ![Добавление политики архивации 5](./media/storsimple-8000-add-backup-policy-u2/addbupol7.png)


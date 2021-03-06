---
title: Руководство. Создание экземпляра видеоаналитики IoT Edge в Azure IoT Central (Intel NUC)
description: В этом руководстве показано, как создать экземпляр видеоаналитики IoT Edge для использования с шаблоном приложения видеоаналитики для распознавания объектов и движения.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
ms.author: nandab
author: KishorIoT
ms.date: 07/27/2020
ms.openlocfilehash: e2e97b857f648390ec017a529115b23c4f17a68d
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94427995"
---
# <a name="tutorial-create-an-iot-edge-instance-for-video-analytics-intel-nuc"></a>Руководство по созданию экземпляра IoT Edge для видеоаналитики (Intel NUC)

Azure IoT Edge — это полностью управляемая служба, которая предоставляет облачную аналитику в локальной среде, развертывая и запуская следующие возможности:

* пользовательская логика;
* Службы Azure
* Искусственный интеллект

В IoT Edge эти службы выполняются непосредственно на кросс-платформенных устройствах Интернета вещей, позволяя вам безопасно выполнять решения Интернета вещей в большом масштабе как в облаке, так и локально.

В этом руководстве показано, как установить и настроить среду выполнения IoT Edge на устройстве Intel NUC.

В этом руководстве описано следующее:
> [!div class="checklist"]
> * обновление и настройка IoT Edge;
> * настройка шлюза IoT Edge;
> * подключение локальной камеры с поддержкой ONVIF к устройству Intel NUC.

## <a name="prerequisites"></a>Предварительные требования

* Прежде чем начать работу, необходимо выполнить инструкции из статей [Руководство. Создание приложения видеоаналитики для распознавания объектов и движения в Azure IoT Central (YOLO v3)](./tutorial-video-analytics-create-app-yolo-v3.md) и [Руководство. Создание приложения видеоаналитики для распознавания объектов и движения в Azure IoT Central (OpenVINO)&trade;](tutorial-video-analytics-create-app-openvino.md).
* Любое устройство под управлением Linux, например Intel NUC, на котором могут выполняться контейнеры Docker и которое имеет достаточно вычислительной мощности для работы с видеоаналитикой.
* Среда выполнения IoT Edge, которая [установлена](../../iot-edge/how-to-install-iot-edge.md) и запущена на этом устройстве.
* Чтобы подключиться к устройству IoT Edge с компьютера под управлением Windows, вам потребуется [клиент PuTTY SSH](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) или аналогичная служебная программа.
* Вам также понадобится подписка Azure. Если у вас еще нет подписки, создайте ее бесплатно на [странице регистрации в Azure](https://aka.ms/createazuresubscription).

## <a name="configure-the-iot-edge-device"></a>Настройка устройства IoT Edge

Если на вашем компьютере Intel NUC не установлена среда выполнения IoT Edge, см. статью [Установка среды выполнения Azure IoT Edge в системах Linux на основе Debian](../../iot-edge/how-to-install-iot-edge.md).

Чтобы обновить среду выполнения IoT Edge, сделайте следующее:

1. С помощью PuTTY подключитесь к устройству IoT Edge.

1. Выполните следующие команды, чтобы обновить и проверить версию среды выполнения IoT Edge. На момент написания статьи это версия 1.0.9.

    ```bash
    sudo apt-get update
    sudo apt-get install libiothsm iotedge
    sudo iotedge --version
    ```

1. С помощью следующих команд создайте папки для развертывания с соответствующими разрешениями.

    ```bash
    sudo mkdir -p /data/storage
    sudo mkdir -p /data/media
    sudo chmod -R 777 /data
    ```

Чтобы добавить файл конфигурации *state.json* в папку */data/storage* на устройстве IoT Edge, сделайте следующее:

1. В любом текстовом редакторе откройте файл *state.json*, расположенный в папке *lva-configuration* на локальном компьютере.

1. Замените заполнители `system` и `iotCentral > properties` строковыми значениями, которые описывают ваше устройство шлюза. Эти значения можно просмотреть позже на панели мониторинга приложения IoT Central.

1. Замените заполнители `iotCentral > appKeys` значениями, которые вы ранее сохранили в файле *scratchpad.txt*, работая с предыдущим руководством. Сохраните изменения.

1. С помощью служебной программы PuTTY `scp` скопируйте файл *state.json*, который вы только что изменили, в папку */data/storage* на устройстве IoT Edge. В нашем примере используется IP-адрес `192.168.0.144`, но его необходимо заменить реальным IP-адресом вашего устройства IoT Edge.

    ```cmd
    scp state.json YourUserName@192.168.0.144:/data/storage/state.json`
    ```

Чтобы настроить в IoT Edge регистрацию и подключение к приложению IoT Central, сделайте следующее:

1. С помощью PuTTY подключитесь к устройству IoT Edge.

1. С помощью любого текстового редактора, например, `nano`, откройте файл config.yaml с конфигурацией IoT Edge.

    ```bash
    sudo nano /etc/iotedge/config.yaml
    ```

    > [!WARNING]
    > В файлах YAML нельзя использовать символы табуляции для выравнивания отступа, поэтому используйте вместо них два пробела. Перед элементами верхнего уровня не допускаются начальные пробелы.

1. Прокручивайте код вниз, пока не отобразится `# Manual provisioning configuration`. Закомментируйте следующие три строки, как показано в следующем фрагменте кода:

    ```yaml
    # Manual provisioning configuration
    #provisioning:
    #  source: "manual"
    #  device_connection_string: "temp"
    ```

1. Прокручивайте код вниз, пока не отобразится `# DPS symmetric key provisioning configuration`. Раскомментируйте следующие восемь строк, как показано в следующем фрагменте кода:

    ```yaml
    # DPS symmetric key provisioning configuration
    provisioning:
      source: "dps"
      global_endpoint: "https://global.azure-devices-provisioning.net"
      scope_id: "{scope_id}"
      attestation:
        method: "symmetric_key"
        registration_id: "{registration_id}"
        symmetric_key: "{symmetric_key}"
    ```

1. Замените `{scope_id}` значением **области идентификатора**, которое вы записали в файл *scratchpad.txt* при работе с предыдущим руководством.

1. Замените `{registration_id}` значением *gateway-001*, то есть именем устройства, которое вы создали при работе с предыдущим учебником.

1. Замените `{symmetric_key}` значением **первичного ключа** для устройства **gateway-001**, которое вы сохранили в файле *scratchpad.txt* при работе с предыдущим учебником.

1. Выполните следующую команду, чтобы перезапустить управляющую программу IoT Edge:

    ```bash
    sudo systemctl restart iotedge
    ```

1. Чтобы проверить состояние модулей IoT Edge, выполните следующую команду:

    ```bash
    iotedge list
    ```

    Выходные данные приведенной выше команды содержат сведения о пяти выполняемых модулях. Вы также можете проверить состояние выполняемых модулей в приложении IoT Central.

    > [!TIP]
    > Повторно выполните эту же команду, чтобы получить сведения о состоянии. Возможно, потребуется подождать, пока все модули запустятся.

Если модули IoT Edge не запускаются нормально, см. статью [Устранение неполадок на устройстве IoT Edge](../../iot-edge/troubleshoot.md).

## <a name="collect-the-rtsp-stream-from-your-camera"></a>Сбор потоковых данных RSTP от камеры

Найдите URL-адреса потока RTSP для каждой камеры, которая подключена к устройству IoT Edge, например:

`rtsp://192.168.1.64:554/Streaming/Channels/101/`

> [!TIP]
> Попробуйте открыть поток камеры на компьютере IoT Edge, используя VLC или другой проигрыватель мультимедиа.

## <a name="next-steps"></a>Дальнейшие действия

Итак, вы развернули среду выполнения IoT Edge и модули LVA на устройстве шлюза Intel NUC.

Чтобы управлять камерами, воспользуйтесь указаниями из следующего руководства:

> [!div class="nextstepaction"]
> [Мониторинг и администрирование приложения видеоаналитики для распознавания объектов и движения](./tutorial-video-analytics-manage.md)
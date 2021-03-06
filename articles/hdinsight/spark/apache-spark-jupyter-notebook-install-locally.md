---
title: Установка записной книжки Jupyter в локальной среде и ее подключение к Spark в Azure HDInsight
description: Сведения о том, как установить записную книжку Jupyter на компьютере локально и как подключить ее к кластеру Apache Spark.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020, devx-track-python
ms.date: 04/23/2020
ms.openlocfilehash: 96b2e7deff464f00ced4457a514ac833a90bd42d
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "95999898"
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Установка записной книжки Jupyter на компьютере и ее подключение к Apache Spark в HDInsight

Из этой статьи вы узнаете, как установить Jupyter Notebook с пользовательскими PySpark (для Python) и Apache Spark (для Scala) с помощью программы Spark Magic. Затем вы подключаете записную книжку к кластеру HDInsight.

При установке Jupyter и подключении к Apache Spark в HDInsight необходимо выполнить четыре основных шага.

* Настройка кластера Spark.
* Установите записную книжку Jupyter.
* Установите ядра PySpark и Spark с помощью волшебной команды Spark.
* Настройте волшебную команду Spark для доступа к кластеру Spark в HDInsight.

Дополнительные сведения о пользовательских ядрах и волшебе Spark см. в разделе [ядра, доступные для записных книжек Jupyter с кластерами Apache Spark Linux в HDInsight](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Предварительные требования

* Кластер Apache Spark в HDInsight. Инструкции см. в статье [Начало работы. Создание кластера Apache Spark в HDInsight на платформе Linux и выполнение интерактивных запросов с помощью SQL Spark](apache-spark-jupyter-spark-sql.md). Локальная Записная книжка подключается к кластеру HDInsight.

* Опыт работы с записными книжками Jupyter с Spark в HDInsight.

## <a name="install-jupyter-notebook-on-your-computer"></a>Установка записной книжки Jupyter на компьютер

Установите Python перед установкой записных книжек Jupyter. В [дистрибутиве Anaconda](https://www.anaconda.com/download/) будут установлены оба, Python и Jupyter Notebook.

Скачайте [установщик Anaconda](https://www.anaconda.com/download/) для своей платформы и запустите программу установки. В мастере установки укажите параметр для добавления Anaconda в переменную PATH.  См. также [Установка Jupyter с помощью Anaconda](https://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-spark-magic"></a>Установка программы Spark Magic

1. Введите одну из приведенных ниже команд, чтобы установить магическую версию Spark. См. также [документацию по sparkmagic](https://github.com/jupyter-incubator/sparkmagic#installation).

    |Cluster version | Команда установки |
    |---|---|
    |v 3.6 и v 3.5 |`pip install sparkmagic==0.13.1`|
    |v 3.4|`pip install sparkmagic==0.2.3`|

1. Убедитесь, что `ipywidgets` установлен правильный параметр, выполнив следующую команду:

    ```cmd
    jupyter nbextension enable --py --sys-prefix widgetsnbextension
    ```

## <a name="install-pyspark-and-spark-kernels"></a>Установка ядер PySpark и Spark

1. Найдите место `sparkmagic` установки, введя следующую команду:

    ```cmd
    pip show sparkmagic
    ```

    Затем измените рабочий каталог на **Расположение** , указанное в приведенной выше команде.

1. В новом рабочем каталоге введите одну или несколько приведенных ниже команд, чтобы установить требуемые ядра.

    |Ядро | Команда |
    |---|---|
    |Spark|`jupyter-kernelspec install sparkmagic/kernels/sparkkernel`|
    |SparkR|`jupyter-kernelspec install sparkmagic/kernels/sparkrkernel`|
    |PySpark|`jupyter-kernelspec install sparkmagic/kernels/pysparkkernel`|
    |PySpark3|`jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel`|

1. Необязательный элемент. Введите следующую команду, чтобы включить расширение сервера:

    ```cmd
    jupyter serverextension enable --py sparkmagic
    ```

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Настройка волшебной команды Spark для подключения к кластеру HDInsight Spark

В этом разделе вы настроите магическое значение Spark, установленное ранее, для подключения к кластеру Apache Spark.

1. Запустите оболочку Python с помощью следующей команды:

    ```cmd
    python
    ```

2. Сведения о конфигурации Jupyter обычно хранятся в домашнем каталоге пользователей. Введите следующую команду, чтобы указать домашний каталог, и создайте папку с именем **\. sparkmagic**.  Будет выведен полный путь.

    ```python
    import os
    path = os.path.expanduser('~') + "\\.sparkmagic"
    os.makedirs(path)
    print(path)
    exit()
    ```

3. В папке `.sparkmagic` Создайте файл с именем **config.js** и добавьте в него следующий фрагмент JSON.  

    ```json
    {
      "kernel_python_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "kernel_scala_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "custom_headers" : {
        "X-Requested-By": "livy"
      },

      "heartbeat_refresh_seconds": 5,
      "livy_server_heartbeat_timeout_seconds": 60,
      "heartbeat_retry_seconds": 1
    }
    ```

4. Внесите следующие изменения в файл:

    |Значение шаблона | Новое значение |
    |---|---|
    |ИМЕН|Имя входа кластера, по умолчанию — `admin` .|
    |CLUSTERDNSNAME|Имя кластера|
    |{BASE64ENCODEDPASSWORD}|Пароль в кодировке Base64 для фактического пароля.  Пароль для base64 можно создать по адресу [https://www.url-encode-decode.com/base64-encode-decode/](https://www.url-encode-decode.com/base64-encode-decode/) .|
    |`"livy_server_heartbeat_timeout_seconds": 60`|При использовании `sparkmagic 0.12.7` (кластеры версии 3.5 и 3.6) не заключайте.  При использовании `sparkmagic 0.2.3` (Clusters v 3.4) Замените на `"should_heartbeat": true` .|

    Полный пример файла можно просмотреть в [образце config.jsв](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json)разделе.

   > [!TIP]  
   > Сигналы пульса отправляются, чтобы предотвратить утечку сеансов. При переходе в спящий режим или завершении работы компьютера пульс не отправляется, что приводит к очистке сеанса. Если вы хотите отключить такое поведение для кластеров версии 3.4, то можете настроить для параметра Livy `livy.server.interactive.heartbeat.timeout` значение `0` с помощью пользовательского интерфейса Ambari. Если для кластеров версии 3.5 не настроить соответствующую конфигурацию, приведенную выше, то сеанс не будет удален.

5. Запустите Jupyter. Выполните следующую команду из командной строки.

    ```cmd
    jupyter notebook
    ```

6. Убедитесь, что вы можете использовать магическую платформу Spark, доступную в ядрах. Выполните следующие шаги.

    a. Создайте новую записную книжку. В правом углу выберите **создать**. Вы должны увидеть ядро по умолчанию **Python 2** или **Python 3** и установленные ядра. Фактические значения могут различаться в зависимости от выбранных вариантов установки.  Выберите **PySpark**.

    ![Доступные ядра в записной книжке Jupyter](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels-notebook.png "Ядра в записной книжке Jupyter")

    > [!IMPORTANT]  
    > После выбора **нового** проверьте оболочку на наличие ошибок.  Если отображается сообщение об ошибке, `TypeError: __init__() got an unexpected keyword argument 'io_loop'` возможно, возникла известная проблема с определенными версиями Торнадо.  Если это так, завершите работу ядра, а затем понизить установку торнадо, выполнив следующую команду: `pip install tornado==4.5.3` .

    b. Запустите следующий фрагмент кода.

    ```sql
    %%sql
    SELECT * FROM hivesampletable LIMIT 5
    ```  

    Если вы успешно получили выходные данные, подключение к кластеру HDInsight работает.

    Если вы хотите обновить конфигурацию записной книжки для подключения к другому кластеру, обновите config.jsс новым набором значений, как показано на шаге 3 выше.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Зачем устанавливать Jupyter на моем компьютере?

Причины для установки Jupyter на компьютере и последующего подключения к кластеру Apache Spark в HDInsight:

* Предоставляет возможность создавать записные книжки локально, тестировать приложение в работающем кластере, а затем отправлять в кластер записные книжки. Чтобы отправить записные книжки в кластер, можно отправить их с помощью записной книжки Jupyter, которая работает под управлением или в кластере, или сохранить в `/HdiNotebooks` папке в учетной записи хранения, связанной с кластером. Дополнительные сведения о хранении записных книжек в кластере см. в разделе [Где хранятся записные книжки](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored).
* С помощью локально доступных записных книжек вы сможете подключиться к различным кластерам Spark в зависимости от потребностей вашего приложения.
* Можно использовать GitHub для реализации системы управления версиями, чтобы контролировать версии записных книжек. Вы также можете создать среду совместной работы, в которой несколько пользователей будут работать с одной записной книжкой.
* Вы можете работать с записными книжками локально даже без кластера. Кластер нужен только для тестирования записных книжек, но не обязателен для ручного управления записными книжками или средой разработки.
* Настройка локальной среды разработки может быть проще, чем настройка установки Jupyter в кластере.  Вы можете воспользоваться всеми преимуществами программного обеспечения, установленными локально, без настройки одного или нескольких удаленных кластеров.

> [!WARNING]  
> Если Jupyter установлен на локальном компьютере, несколько пользователей могут одновременно запустить одну и ту же записную книжку в одном кластере Spark. В такой ситуации создаются несколько сеансов Livy. Если вы столкнетесь с проблемами и начнете их отладку, вам будет сложно определить, какой сеанс Livy какому пользователю принадлежит.  

## <a name="next-steps"></a>Дальнейшие действия

* [Обзор: Spark в Azure HDInsight](apache-spark-overview.md)
* [Ядра для записной книжки Jupyter на Apache Spark](apache-spark-jupyter-notebook-kernels.md)
* [Использование внешних пакетов с записными книжками Jupyter в Apache Spark](apache-spark-jupyter-notebook-use-external-packages.md)

---
title: Использование SSH c Hadoop в Azure HDInsight
description: Вы можете получить доступ к HDInsight с помощью Secure Shell (SSH). В этом документе содержатся сведения о подключении к HDInsight с помощью команд SSH для клиентов Windows, Linux, UNIX или macOS.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017,seoapr2020
ms.date: 02/28/2020
ms.openlocfilehash: dbbb205aea48dedbe13696d0a43e632faf2ae339
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2020
ms.locfileid: "92546149"
---
# <a name="connect-to-hdinsight-apache-hadoop-using-ssh"></a>Подключение к HDInsight (Apache Hadoop) с помощью SSH

Сведения о выполнении безопасного подключения к Apache Hadoop в Azure HDInsight с помощью [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell). Сведения о подключении через виртуальную сеть см. в статье [Архитектура виртуальной сети Azure HDInsight](./hdinsight-virtual-network-architecture.md). См. также [Планирование развертывания виртуальной сети для кластеров Azure HDInsight](./hdinsight-plan-virtual-network-deployment.md).

В следующей таблице содержатся сведения об адресе и порте, необходимые при подключении к HDInsight с помощью клиента SSH.

| Адрес | Port | Подключается к... |
| ----- | ----- | ----- |
| `<clustername>-ssh.azurehdinsight.net` | 22 | Основной головной узел |
| `<clustername>-ssh.azurehdinsight.net` | 23 | Дополнительный головной узел |
| `<clustername>-ed-ssh.azurehdinsight.net` | 22 | граничные узлы (службы ML в HDInsight) |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | 22 | краевой узел (любой другой тип кластера, если существует краевой узел) |

Замените `<clustername>` именем кластера. Замените `<edgenodename>` на имя граничного узла.

Мы рекомендуем __всегда подключаться к граничному узлу__ (при наличии его в кластере) с помощью SSH. Службы, размещенные на головных узлах, критически важны для работоспособности Hadoop. Граничный узел выполняет только размещаемые на нем службы. Дополнительные сведения об использовании граничных узловсм. в разделе Доступ к граничному узлу.

> [!TIP]  
> При первом подключении к HDInsight клиент SSH может отобразить предупреждение о том, что не удается установить подлинность узла. При появлении запроса выберите "Да", чтобы добавить узел в список доверенных серверов клиента SSH.
>
> Если вы ранее подключались к серверу с тем же именем, вы можете получить предупреждение о том, что сохраненный ключ узла не совпадает с ключом узла сервера. Обратитесь к документации для вашего клиента SSH, чтобы узнать, как удалить имеющуюся запись для имени сервера.

## <a name="ssh-clients"></a>SSH-клиенты

Системы Linux, Unix и macOS предоставляют команды `ssh` и `scp`. Клиент `ssh` обычно используется для создания удаленного сеанса командной строки с помощью системы на основе Unix или Linux. С помощью `scp` можно безопасно копировать файлы между клиентской и удаленной системой.

По умолчанию в Microsoft Windows не устанавливаются клиенты SSH. Клиенты `ssh` и `scp` для Windows доступны в следующих пакетах:

* [Клиент OpenSSH](/windows-server/administration/openssh/openssh_install_firstuse). Этот клиент является дополнительным компонентом, представленным в обновлении Windows 10 для дизайнеров.

* [Bash на Ubuntu в Windows 10](/windows/wsl/about).

* [Azure Cloud Shell](../cloud-shell/quickstart.md). Cloud Shell предоставляет среду Bash в браузере.

* [Git](https://git-scm.com/).

Существует также несколько графических SSH-клиентов, [таких как](https://www.chiark.greenend.org.uk/~sgtatham/putty/) выводимые и [MobaXterm](https://mobaxterm.mobatek.net/). Хотя эти клиенты можно использовать для подключения к HDInsight, процесс подключения отличается от использования служебной программы `ssh`. Дополнительные сведения см. в документации графического клиента, который вы используете.

## <a name="authentication-ssh-keys"></a><a id="sshkey"></a>Проверка подлинности с помощью ключей SSH

Ключи SSH используют [Шифрование с открытым ключом](https://en.wikipedia.org/wiki/Public-key_cryptography) для проверки подлинности сеансов SSH. В отличие от паролей, они являются более безопасными и предоставляют простой способ защиты доступа к кластеру Hadoop.

Если учетная запись SSH защищена ключом, при подключении клиент должен предоставить соответствующий закрытый ключ.

* В большинстве клиентов можно настроить использование __ключа по умолчанию__ . Например, клиент `ssh` будет искать закрытый ключ в `~/.ssh/id_rsa` в средах Linux и Unix.

* Вы можете указать __путь к закрытому ключу__ . В клиенте `ssh` это можно сделать с помощью параметра `-i`. Например, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.

* Если у вас есть __несколько закрытых ключей__ для использования на разных серверах, рекомендуем использовать служебные программы, например [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent). Служебную программу `ssh-agent` можно использовать для автоматического выбора подходящего ключа при установке сеанса SSH.

> [!IMPORTANT]  
> Если закрытый ключ защищен парольной фразой, при использовании ключа ее необходимо будет ввести. Существуют также служебные программы (например, `ssh-agent`), позволяющие кэшировать пароль.

### <a name="create-an-ssh-key-pair"></a>Создание пары ключей SSH

Чтобы создать файлы открытого и закрытого ключей, используйте команду `ssh-keygen`. Приведенная ниже команда создает пару 2048-разрядных ключей RSA, которую можно использовать с HDInsight.

```azurepowershell-interactive
ssh-keygen -t rsa -b 2048
```

В процессе создания ключа Вам будет предложено ввести сведения. например о том, где хранятся ключи и следует ли использовать парольную фразу. После завершения этой процедуры будут созданы два файла: файл открытого ключа и файл закрытого ключа.

* __Открытый ключ__ используется для создания кластера HDInsight. Этот файл имеет расширение `.pub`.

* __Закрытый ключ__ используется для проверки подлинности клиента в кластере HDInsight.

> [!IMPORTANT]  
> Эти ключи можно защитить с помощью парольной фразы. Парольная фраза — это, по сути, пароль закрытого ключа. Даже если кто-то получит ваш закрытый ключ, он не сможет использовать его, не зная парольную фразу.

### <a name="create-hdinsight-using-the-public-key"></a>Создание кластера HDInsight с помощью открытого ключа

| Метод создания | Использование открытого ключа |
| ------- | ------- |
| Портал Azure | Снимите флажок __использовать пароль для входа в кластер для SSH__ , а затем выберите __открытый ключ__ в качестве типа проверки подлинности SSH. Наконец, выберите файл открытого ключа или вставьте текстовое содержимое файла в поле __Открытый ключ SSH__ .</br>![Диалоговое окно открытого ключа SSH при создании кластера HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png) |
| Azure PowerShell | Используйте `-SshPublicKey` параметр командлета [New-аздинсигхтклустер](/powershell/module/az.hdinsight/new-azhdinsightcluster) и передайте содержимое открытого ключа в виде строки.|
| Azure CLI | Используйте `--sshPublicKey` параметр [`az hdinsight create`](/cli/azure/hdinsight#az-hdinsight-create) команды и передайте содержимое открытого ключа в виде строки. |
| Шаблон Resource Manager | Пример использования ключей SSH с помощью шаблона см. на странице [Deploy HDInsight on Linux (w/ Azure Storage, SSH key)](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/) (Развертывание HDInsight в Linux с использованием службы хранилища Azure и ключа SSH). Элемент `publicKeys` в файле [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) используется для передачи ключей в Azure при создании кластера. |

## <a name="authentication-password"></a>Проверка подлинности: пароль

Учетные записи SSH можно защитить с помощью пароля. При подключении к HDInsight с помощью SSH вам будет предложено ввести пароль.

> [!WARNING]  
> Мы не советуем использовать такой метод аутентификации для SSH. Пароли можно угадать. К тому же они подвержены атакам методом подбора. Вместо этого рекомендуется использовать [проверку подлинности с помощью ключей SSH](#sshkey).

> [!IMPORTANT]  
> Срок действия пароля учетной записи SSH истекает через 70 дней после создания кластера HDInsight. Если срок действия пароля истек, его можно изменить с помощью инструкций в документе по [управлению HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords).

### <a name="create-hdinsight-using-a-password"></a>Создание кластера HDInsight с использованием пароля

| Метод создания | Указание пароля |
| --------------- | ---------------- |
| Портал Azure | По умолчанию учетная запись пользователя SSH содержит тот же пароль, что и учетная запись входа кластера. Чтобы использовать другой пароль, снимите флажок __использовать пароль для входа в кластер для SSH__ , а затем введите пароль в поле __пароль SSH__ .</br>![Диалоговое окно пароля SSH при создании кластера HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)|
| Azure PowerShell | Используйте `--SshCredential` параметр командлета [New-аздинсигхтклустер](/powershell/module/az.hdinsight/new-azhdinsightcluster) и передайте `PSCredential` объект, содержащий имя и пароль учетной записи пользователя SSH. |
| Azure CLI | Используйте `--ssh-password` параметр [`az hdinsight create`](/cli/azure/hdinsight#az-hdinsight-create) команды и укажите значение пароля. |
| Шаблон Resource Manager | Пример использования пароля с помощью шаблона см. на странице [Deploy HDInsight cluster with Storage and SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/) (Развертывание HDInsight с использованием службы хранилища и пароля SSH). Элемент `linuxOperatingSystemProfile` в файле [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) используется для передачи имени учетной записи SSH и пароля в Azure при создании кластера.|

### <a name="change-the-ssh-password"></a>Изменение пароля SSH

Дополнительные сведения об изменении пароля учетной записи пользователя SSH см. в разделе __Изменение паролей__ статьи [Управление кластерами Hadoop в HDInsight с помощью портала Azure](hdinsight-administer-use-portal-linux.md#change-passwords).

## <a name="authentication-domain-joined-hdinsight"></a>Домен аутентификации, присоединенный к HDInsight

Если вы используете __кластер HDInsight, присоединенный к домену__ , необходимо использовать `kinit` команду после подключения с локальным пользователем SSH. Эта команда запрашивает имя и пароль пользователя домена и проверяет подлинность сеанса с помощью домена Azure Active Directory, связанного с кластером.

Можно также включить проверку подлинности Kerberos на каждом присоединенном к домену узле (например, головном узле, пограничном узле) к SSH с помощью учетной записи домена. Для этого измените файл конфигурации sshd:

```bash
sudo vi /etc/ssh/sshd_config
```

Раскомментируйте и замените `KerberosAuthentication` на `yes`

```bash
sudo service sshd restart
```

Используйте `klist` команду, чтобы убедиться, что проверка подлинности Kerberos прошла успешно.

Дополнительные сведения см. в статье [Настройка присоединенных к домену кластеров HDInsight (предварительная версия)](./domain-joined/apache-domain-joined-configure-using-azure-adds.md).

## <a name="connect-to-nodes"></a>Подключение к узлам

Головные и граничные узлы (если таковые имеются) доступны через Интернет через порты 22 и 23.

* При подключении к __головному узлу__ используйте порт __22__ , чтобы подключиться к первичному головному узлу, и порт __23__ , чтобы подключиться к вторичному головному узлу. Используйте полное доменное имя `clustername-ssh.azurehdinsight.net`, где `clustername` — имя кластера.

    ```bash
    # Connect to primary head node
    # port not specified since 22 is the default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect to secondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```

* Чтобы подключиться к __граничному узлу__ , используйте порт 22. Полное доменное имя — `edgenodename.clustername-ssh.azurehdinsight.net`, где `edgenodename` — это имя, которое вы указали при создании граничного узла. `clustername` — это имя кластера.

    ```bash
    # Connect to edge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]  
> В предыдущих примерах предполагается, что вы используете аутентификацию на основе пароля или что аутентификация на основе сертификата выполняется автоматически. Если вы используете пару ключей SSH для проверки подлинности и сертификат не используется автоматически, укажите закрытый ключ с помощью параметра `-i`. Например, `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.

После подключения запрос изменится, указывая имя пользователя SSH и узел, к которому вы подключены. Например, если к первичному головному узлу подключается пользователь `sshuser`, командная строка выглядит так: `sshuser@<active-headnode-name>:~$`.

### <a name="connect-to-worker-and-apache-zookeeper-nodes"></a>Подключение к рабочим узлам и узлам Apache Zookeeper

Рабочие узлы и узлы Zookeeper недоступны из Интернета напрямую. но к ним можно получить доступ с головного или граничного узла кластера. Шаги ниже описывают общую процедуру подключения к другим узлам.

1. Используйте SSH, чтобы подключиться к головному или граничному узлу:

    ```bash
    ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net
    ```

2. После подключения по протоколу SSH используйте команду `ssh`, чтобы соединиться с рабочим узлом в кластере:

    ```bash
    ssh sshuser@wn0-myhdi
    ```

    Чтобы получить список имен узлов, обратитесь к разделу [Пример. Получение полного доменного имени для узлов кластера](hdinsight-hadoop-manage-ambari-rest-api.md#get-the-fqdn-of-cluster-nodes).

Если учетная запись SSH защищена __паролем__ , при подключении введите пароль.

Если она защищена __ключами SSH__ , убедитесь, что на клиентском компьютере включено перенаправление SSH.

> [!NOTE]  
> Другой способ получить прямой доступ ко всем узлам кластера — установить HDInsight в виртуальной сети Azure. Затем к той же виртуальной сети можно присоединить удаленный компьютер и получить прямой доступ ко всем узлам кластера.
>
> Дополнительные сведения см. [в статье Планирование виртуальной сети для HDInsight](hdinsight-plan-virtual-network-deployment.md).

### <a name="configure-ssh-agent-forwarding"></a>Настройка перенаправления SSH-агента

> [!IMPORTANT]  
> При описании шагов ниже предполагается использование системы под управлением Linux или UNIX, а также Bash Windows 10. Если выполнение этих шагов в вашей системе не дает результатов, см. документацию по соответствующему SSH-клиенту.

1. Откройте в текстовом редакторе файл `~/.ssh/config`. Если этот файл отсутствует, его можно создать. Для этого в командной строке введите команду `touch ~/.ssh/config`.

2. Добавьте следующий текст в файл `config`.

    ```
    Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
        ForwardAgent yes
    ```

    Замените сведения об __узле__ на адрес узла, к которому вы подключаетесь с помощью SSH. В предыдущем примере используется граничный узел. Эта запись позволяет настроить перенаправление агента SSH для указанного узла.

3. Чтобы проверить перенаправление агента SSH, выполните следующую команду в терминале:

    ```bash
    echo "$SSH_AUTH_SOCK"
    ```

    Эта команда возвращает сведения аналогичные следующим:

    ```output
    /tmp/ssh-rfSUL1ldCldQ/agent.1792
    ```

    Если ничего не возвращается, то `ssh-agent` не работает. Чтобы получить сведения о скриптах запуска агентов, см. документ по [использованию агента SSH с SSH (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) или документацию по соответствующему SSH-клиенту.

4. Убедившись, что **ssh-агент** работает, добавьте к агенту закрытый ключ SSH с помощью следующей команды:

    ```bash
    ssh-add ~/.ssh/id_rsa
    ```

    Если закрытый ключ хранится в другом файле, замените `~/.ssh/id_rsa` на путь к файлу.

5. Подключитесь к граничному или головному узлу кластера с помощью SSH. Затем воспользуйтесь командой SSH для подключения к рабочему узлу или узлу Zookeeper. Подключение устанавливается с использованием перенаправленного ключа.

## <a name="next-steps"></a>Дальнейшие действия

* [Использование туннелирования SSH для доступа к веб-интерфейсу Ambari, JobHistory, NameNode, Oozie и другим веб-интерфейсам](hdinsight-linux-ambari-ssh-tunnel.md)
* [Доступ к граничному узлу](hdinsight-apps-use-edge-node.md#access-an-edge-node)
* [Использование SCP с HDInsight](./use-scp.md)
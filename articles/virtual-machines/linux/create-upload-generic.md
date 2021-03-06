---
title: Создание и загрузка Linux VHD
description: Узнайте, как создать и передать виртуальный жесткий диск (VHD-файл) Azure, содержащий операционную систему Linux.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: how-to
ms.date: 10/08/2018
ms.author: guybo
ms.openlocfilehash: a80cc29f318cff8e5a4c665cd07ba1829d25d66d
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96016341"
---
# <a name="information-for-non-endorsed-distributions"></a>Информация о нерекомендованных дистрибутивах

Соглашение об уровне обслуживания для платформы Azure применяется для виртуальных машин с ОС Linux, только если используется один из [рекомендованных дистрибутивов](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Для рекомендованных дистрибутивов предоставляются предварительно настроенные образы Linux, представленные в коллекции в Azure Marketplace.

* [Linux в Azure — рекомендованные дистрибутивы](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Поддержка образов Linux в Microsoft Azure](https://support.microsoft.com/kb/2941892)

К любым дистрибутивам, выполняющимся в Azure, предъявляются определенные требования. Поскольку каждый дистрибутив имеет свои особенности, привести исчерпывающую информацию в рамках одной статьи невозможно. Даже при соблюдении всех приведенных ниже условий для надлежащей работы может потребоваться масштабная настройка системы Linux.

Мы рекомендуем начать с одного из [рекомендованных дистрибутивов Linux в Azure](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). В следующих статьях описывается подготовка различных рекомендованных дистрибутивов Linux, которые поддерживаются в Azure:

- [Подготовка виртуальной машины на основе CentOS для Azure](create-upload-centos.md)
- [Подготовка виртуального жесткого диска Debian для Azure](debian-create-upload-vhd.md)
- [Flatcar Container Linux](flatcar-create-upload-vhd.md)
- [Oracle Linux](oracle-create-upload-vhd.md)
- [Red Hat Enterprise Linux](redhat-create-upload-vhd.md)
- [Подготовка виртуальной машины SLES или openSUSE для Azure](suse-create-upload-vhd.md)
- [Ubuntu](create-upload-ubuntu.md)

В этой статье приводятся общие рекомендации по работе с дистрибутивом Linux в Azure.

## <a name="general-linux-installation-notes"></a>Общие замечания по установке Linux
* Azure не поддерживает формат виртуального жесткого диска Hyper-V (VHDX) и может работать только с *фиксированными виртуальными жесткими дисками*.  Диск можно преобразовать в формат VHD с помощью диспетчера Hyper-V или командлета [Convert-VHD](/powershell/module/hyper-v/convert-vhd) . Если вы используете VirtualBox, при создании диска нужно выбрать **фиксированный размер** вместо динамически выделяемого, который используется по умолчанию.
* Azure поддерживает виртуальные машины Gen1 (Boot BIOS) & Gen2 (Загрузка UEFI).
* Максимально допустимый размер виртуального жесткого диска составляет 1023 ГБ.
* При установке системы Linux рекомендуется использовать стандартные разделы, а не диспетчер логических томов (LVM), который часто используется по умолчанию при установке. Это позволит избежать конфликта имен LVM c клонированными виртуальными машинами, особенно если диск с OC может быть подключен к другой идентичной виртуальной машине в целях устранения неполадок. Для дисков данных можно использовать [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) или [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Требуется поддержка ядра для монтирования файловых систем UDF. При первой загрузке в Azure конфигурация подготовки передается в виртуальную машину Linux через UDF-носитель, подключенный к гостевой машине. Агент Azure Linux должен подключить файловую систему UDF для считывания конфигурации и подготовки виртуальной машины.
* Версии ядра Linux ниже 2.6.37 не поддерживают NUMA в Hyper-V с виртуальными машинами большего размера. Эта проблема влияет в основном на дистрибутивы более ранних версий, в которых используется исходное ядро Red Hat 2.6.32, и была исправлена в Red Hat Enterprise Linux (RHEL) 6.6 (kernel-2.6.32-504). В системах под управлением модифицированных ядер старше версии 2.6.37 или ядер RHEL старше 2.6.32-504 в командной строке ядра необходимо задать параметр загрузки `numa=off` в файле grub.conf. Дополнительные сведения см. в [статье базы знаний Red Hat 436883](https://access.redhat.com/solutions/436883).
* Не настраивайте раздел подкачки на диске с ОС. Можно настроить агент Linux для создания файла подкачки на временном диске ресурсов, как описывается на следующих шагах.
* Размер виртуальной памяти всех виртуальных жестких дисков в Azure должен быть округлен до 1 МБ. Перед конвертацией диска RAW в формат VHD убедитесь, что размер диска RAW в несколько раз превышает 1 МБ, как описывается на следующих шагах.

### <a name="installing-kernel-modules-without-hyper-v"></a>Установка модулей ядра без Hyper-V
Так как Azure запускается на гипервизоре Hyper-V, Linux требует определенные модули ядра для запуска в Azure. При наличии виртуальной машины, созданной вне Hyper-V, установщики Linux могут не включать драйверы для Hyper-V в начальный электронный диск (initrd или initramfs), если только виртуальная машина не обнаружит, что он работает в среде Hyper-V. При использовании другой системы виртуализации (например, VirtualBox, KVM и т. д.) для подготовки образа Linux может потребоваться перестроить initrd, чтобы по крайней мере hv_vmbus и hv_storvsc модули ядра были доступны на начальном Ramdisk.  Это известная проблема встречается по крайней мере в системах на основе предшествующего дистрибутива Red Hat. Также она может встречаться в других версиях.

Механизм повторной сборки образа initrd или initramfs может отличаться в зависимости от дистрибутива. Чтобы узнать правильную процедуру для вашего дистрибутива, см. документацию по дистрибутиву или раздел поддержки.  Ниже приведен пример выполнения повторной сборки initrd с помощью служебной программы `mkinitrd`:

1. Создайте резервную копию существующего образа initrd:

    ```
    cd /boot
    sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak
    ```

2. Выполните повторную сборку initrd с использованием модулей ядра hv_vmbus и hv_storvsc:

    ```
    sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`
    ```

### <a name="resizing-vhds"></a>Изменение размера VHD
Размер виртуальной памяти образов VHD в Azure должен быть округлен до 1 МБ.  Как правило, размер VHD, созданных с помощью Hyper-V, настроен правильно.  Если виртуальный жесткий диск (VHD) настроен неправильно, при попытке создать образ из VHD-файла может появиться следующее сообщение об ошибке.

* Виртуальный жесткий диск http: \/ / \<mystorageaccount> . BLOB.Core.Windows.NET/VHDs/MyLinuxVM.VHD имеет неподдерживаемый размер 21475270656 байт. Размер должен задаваться целым числом (в МБ).

В таком случае вы можете изменить размер виртуальной машины с помощью консоли диспетчера Hyper-V или командлета Powershell [Resize-VHD](/powershell/module/hyper-v/resize-vhd?view=win10-ps).  Если вы работаете не в среде Windows, воспользуйтесь командой `qemu-img` для преобразования (если необходимо) и изменения размера VHD.

> [!NOTE]
> В команде qemu-img версии 2.2.1 и более поздних версиях есть [ошибка](https://bugs.launchpad.net/qemu/+bug/1490611), которая приводит к созданию неверного формата VHD. Эта проблема устранена в QEMU версии 2.6. Рекомендуется использовать либо версию `qemu-img` 2.2.0 или более раннюю, либо версию 2.6 или более позднюю.
> 

1. Изменение размера VHD с непосредственным использованием таких инструментов, как `qemu-img` или `vbox-manage`, может привести к сбою загрузки VHD.  Мы советуем сначала преобразовать VHD в образ необработанного диска.  Если образ виртуальной машины был создан в качестве образа необработанного диска (по умолчанию для некоторых гипервизоров, например, KVM), вы можете пропустить этот шаг.
 
    ```
    qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw
    ```

1. Рассчитайте необходимый размер образа диска, чтобы округлить размер виртуальной памяти до 1 МБ.  Следующий скрипт для оболочки Bash использует `qemu-img info` для определения виртуального размера образа диска и вычисляет размер с округлением до 1 МБ в большую сторону.

    ```bash
    rawdisk="MyLinuxVM.raw"
    vhddisk="MyLinuxVM.vhd"

    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "$rawdisk" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

    rounded_size=$(((($size+$MB-1)/$MB)*$MB))
    
    echo "Rounded Size = $rounded_size"
    ```

3. Измените размер необработанного диска с помощью `$rounded_size`, как показано выше.

    ```bash
    qemu-img resize MyLinuxVM.raw $rounded_size
    ```

4. А теперь преобразуйте необработанный диск обратно в VHD с фиксированным размером памяти.

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd
    ```

   Или с помощью qemu версии 2.6 или выше добавьте параметр `force_size`.

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd
    ```

## <a name="linux-kernel-requirements"></a>Рекомендации по ядру Linux

Драйверы Linux Integration Services (LIS) для Hyper-V и Azure встраиваются непосредственно в основное ядро Linux. Во многих дистрибутивах, которые включают последнюю версию ядра Linux (например, 3.x), эти драйверы уже доступны, или же в ядре предоставляются их более ранние версии.  Эти ядра в основном ядре постоянно обновляются с помощью новых исправлений и функций, поэтому по возможности рекомендуется использовать [рекомендованный дистрибутив](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), включающий все исправления и обновления.

Если вы работаете с одним из вариантов Red Hat Enterprise Linux версий 6.0–6.3, вам потребуется установить [последние драйверы LIS для Hyper-V](https://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). Начиная с RHEL 6.4 и более поздних версий (и его производных вариантов), драйверы LIS уже включены в ядро. Поэтому дополнительные пакеты установки не требуются.

Если необходимо собственное ядро, рекомендуется использовать одну из последних версий (то есть 3.8 или более позднюю). Для дистрибутивов или поставщиков, предоставляющих собственное ядро, рекомендуется регулярно переносить драйверы LIS из основного ядра в собственное.  Даже если вы работаете с относительно свежей версией ядра, настоятельно рекомендуется отслеживать исправления драйверов LIS в основном ядре и по мере необходимости обновлять драйверы. Расположение файлов исходного кода драйверов LIS указано в файле [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) в дереве исходного кода ядра Linux:
```
    F:    arch/x86/include/asm/mshyperv.h
    F:    arch/x86/include/uapi/asm/hyperv.h
    F:    arch/x86/kernel/cpu/mshyperv.c
    F:    drivers/hid/hid-hyperv.c
    F:    drivers/hv/
    F:    drivers/input/serio/hyperv-keyboard.c
    F:    drivers/net/hyperv/
    F:    drivers/scsi/storvsc_drv.c
    F:    drivers/video/fbdev/hyperv_fb.c
    F:    include/linux/hyperv.h
    F:    tools/hv/
```
Ядро должно включать следующие исправления. Этот список не является исчерпывающим для всех дистрибутивов.

* [ata_piix: предоставление дисков драйверам Hyper-V по умолчанию;](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: принятие во внимание передаваемых пакетов в папке RESET;](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: предотвращение использования WRITE_SAME;](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: отключение WRITE SAME для драйверов RAID и адаптеров виртуальных узлов;](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: исправление разыменования пустого указателя;](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: ошибки кольцевого буфера могут привести к заморозке операций ввода-вывода.](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: защита от двойного выполнения __scsi_remove_device.](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="the-azure-linux-agent"></a>Агент Linux для Azure
[Агент Linux для Azure](../extensions/agent-linux.md) `waagent` выполняет подготовку виртуальной машины Linux в Azure. Можно получить его последнюю версию, узнать о проблемах с файлами или отправить запрос в [репозитории GitHub для агента Linux](https://github.com/Azure/WALinuxAgent).

* Агент Linux выпускается по лицензии Apache 2.0. Многие дистрибутивы уже содержат пакеты RPM или. deb для агента, и эти пакеты можно легко установить и обновить.
* Для работы агента Linux для Azure требуется Python v2.6+.
* Для агента также необходим модуль python-pyasn1. В большинстве дистрибутивов он предоставляется в виде отдельно устанавливаемого пакета.
* В некоторых случаях агент Linux для Azure может быть несовместим с NetworkManager. Многие из пакетов RPM и deb, предоставляемые дистрибутивами, настраивают NetworkManager в качестве конфликта для пакета waagent. В таких случаях при установке пакета агента Linux удаляется NetworkManager.
* Версия агента Linux для Azure должна быть выше [минимальной поддерживаемой версии](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

## <a name="general-linux-system-requirements"></a>Общие системные требования для Linux

1. Измените строку загрузки ядра в GRUB или GRUB2 и включите в нее следующие параметры, задающие отправку всех сообщений консоли в первый последовательный порт. Эти сообщения могут использоваться службой поддержки Azure при отладке возникающих проблем.
    ```  
    console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
    ```
    Кроме того, мы рекомендуем *удалить* следующие параметры (если они заданы).
    ```  
    rhgb quiet crashkernel=auto
    ```
    Графическая и "тихая" загрузка бесполезны в облачной среде, в которой требуется, чтобы все журналы отправлялись на последовательный порт. При желании можно настроить параметр `crashkernel`, однако учитывайте, что он сокращает объем доступной памяти в виртуальной машине на 128 МБ или более, что может оказаться проблемой в виртуальных машинах небольшого размера.

1. Установите агент Linux для Azure.
  
    Агент Linux для Azure необходим для подготовки образа Linux в Azure.  Многие дистрибутивы предоставляют агенту пакет RPM или deb (пакет обычно называется WALinuxAgent или WALinuxAgent).  Агент также можно установить вручную, как описано в [руководстве по агенту Linux](../extensions/agent-linux.md).

1. Убедитесь, что SSH-сервер установлен и настроен для включения во время загрузки.  Как правило, эта конфигурация используется по умолчанию.

1. Не создавайте пространство подкачки на диске с ОС.
  
    Агент Linux для Azure может автоматически настраивать пространство подкачки с использованием диска на локальном ресурсе, подключенном к виртуальной машине после подготовки для работы в среде Azure. Локальный диск ресурсов является *временным* и может быть очищен при отмене подготовки виртуальной машины. После установки агента Linux для Azure (см. шаг 2) измените следующие параметры в /etc/waagent.conf соответствующим образом.
    ```  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: Set this to your desired size.
    ```
1. Выполните следующие команды, чтобы отозвать виртуальную машину.
  
     ```
     sudo waagent -force -deprovision
     export HISTSIZE=0
     logout
     ```  
   > [!NOTE]
   > В Virtualbox после выполнения команды`waagent -force -deprovision` может появиться следующая ошибка: `[Errno 5] Input/output error`. Это сообщение об ошибке не является важным и может быть пропущено.

* Завершите работу виртуальной машины и передайте виртуальный жесткий диск в Azure.

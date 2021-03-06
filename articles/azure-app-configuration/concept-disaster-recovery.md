---
title: Устойчивость и аварийное восстановление конфигурации приложений Azure
description: Поэкономично реализовать устойчивость и аварийное восстановление с помощью конфигурации приложения Azure.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 5c62f10d67345d68cde27af7d0a7663b22d978a0
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96002431"
---
# <a name="resiliency-and-disaster-recovery"></a>Устойчивость и аварийное восстановление

В настоящее время Конфигурация приложений Azure — это региональная служба. Каждое хранилище конфигураций создается в определенном регионе Azure. Региональный сбой влияет на все хранилища в затронутом регионе. Конфигурация приложений не предоставляет автоматическую отработку отказа в другой регион. Эта статья содержит общие рекомендации по использованию нескольких хранилищ конфигураций в различных регионах Azure, позволяющие увеличить географическую устойчивость приложения.

## <a name="high-availability-architecture"></a>Архитектура высокого уровня доступности

Чтобы реализовать межрегиональную избыточность, необходимо создать несколько хранилищ конфигураций приложения в разных регионах. При такой конфигурации приложение обладает по крайней мере одним дополнительным хранилищем конфигураций, на которое можно будет переключиться, если основное хранилище станет недоступным. На следующей схеме топологии отображается взаимодействие приложения и его основного и дополнительного хранилищ конфигураций.

![Геоизбыточные хранилища](./media/geo-redundant-app-configuration-stores.png)

Приложение загрузит конфигурацию из основного и дополнительного хранилищ в параллельном режиме. Это повышает вероятность успешного получения данных конфигурации. Вы несете ответственность за хранение данных в обоих магазинах в синхронизации. В следующих разделах объясняется, как можно создать геоустойчивость в приложении.

## <a name="failover-between-configuration-stores"></a>Отработка отказа между хранилищами конфигураций

С технической точки зрения приложение не выполняет отработку отказа. Оно пытается получить одинаковый набор данных конфигурации из двух хранилищ конфигураций одновременно. Упорядочите код таким образом, чтобы он загружал данные из дополнительного хранилища, а затем — из основного. Такой подход гарантирует, что данные конфигурации в основном хранилище будут иметь приоритет всегда, когда оно доступно. В следующем фрагменте кода показано, как реализовать это размещение в .NET Core:

#### <a name="net-core-2x"></a>[.NET Core 2.x](#tab/core2x)

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                  .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
        })
        .UseStartup<Startup>();
    
```

#### <a name="net-core-3x"></a>[.NET Core 3.x](#tab/core3x)

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
            webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                    .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
            })
            .UseStartup<Startup>());
```
---

Обратите внимание на параметр `optional`, передаваемый в функцию `AddAzureAppConfiguration`. Если для него задано значение `true`, то этот параметр не дает приложению продолжить работу при сбое, когда функции не удается загрузить данные конфигурации.

## <a name="synchronization-between-configuration-stores"></a>Синхронизация хранилищ конфигураций

Важно, чтобы все геоизбыточные хранилища конфигураций содержали одинаковый набор данных. Это достигается двумя способами.

### <a name="backup-manually-using-the-export-function"></a>Резервное копирование вручную с помощью функции Export

Можно использовать функцию **экспорта** в Конфигурации приложений,чтобы копировать данные из основного хранилища в дополнительное хранилище по запросу. Эта функция доступна на портале Azure и в интерфейсе командной строки.

На портале Azure можно отправить изменение в другое хранилище конфигураций, выполнив следующие действия.

1. Перейдите на вкладку **Импорт/Экспорт** и выберите команду **Экспорт** > **Конфигурация приложения** > **Цель** > **Выбрать ресурс**.

1. В открывшейся новой колонке укажите подписку, группу ресурсов и имя ресурса для вторичного хранилища, а затем нажмите кнопку **Применить**.

1. После обновления пользовательского интерфейса вы сможете выбрать, какие данные конфигурации следует экспортировать в дополнительное хранилище. Можно оставить значение времени по умолчанию как есть и задать оба значения в полях **Метка** и **Метка** . Нажмите кнопку **Применить**. Повторите эти действия для всех меток в основном хранилище.

1. Повторите предыдущие шаги при изменении конфигурации.

Процесс экспорта также может быть достигнут с помощью Azure CLI. Следующая команда показывает, как экспортировать все конфигурации из основного хранилища в базу данных-получатель:

```azurecli
    az appconfig kv export --destination appconfig --name {PrimaryStore} --dest-name {SecondaryStore} --label * --preserve-labels -y
```

### <a name="backup-automatically-using-azure-functions"></a>Автоматическое резервное копирование с помощью функций Azure

Процесс резервного копирования можно автоматизировать с помощью функций Azure. Она использует интеграцию со службой "Сетка событий Azure" в конфигурации приложения. После настройки приложение будет публиковать события в сетке событий для любых изменений, внесенных в значения ключа в хранилище конфигураций. Таким образом, приложение "функции Azure" может прослушивать эти события и данные резервного копирования соответствующим образом. Дополнительные сведения см. в руководстве по [автоматическому резервному копированию хранилищ конфигураций приложений](./howto-backup-config-store.md).

## <a name="next-steps"></a>Следующие шаги

В этой статье вы узнали, как расширить возможности приложения, чтобы достичь географической устойчивости во время выполнения для Конфигурации приложений. Также на этапе сборки или развертывания можно внедрить данные конфигурации из Конфигурации приложений. Дополнительную информацию см. в разделе [Интеграция с конвейером CI/CD](./integrate-ci-cd-pipeline.md).

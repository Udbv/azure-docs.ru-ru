---
title: Синхронизация службы каталогов с Azure Active Directory
description: Руководство по архитектуре для достижения синхронизации каталогов с Azure Active Directory.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 748f91b2fe77667969e9736f8084a9dd24018425
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "96172475"
---
# <a name="directory-synchronization"></a>Синхронизация каталогов

У многих организаций есть Гибридная инфраструктура, охватывающая как локальные, так и облачные компоненты. Синхронизация удостоверений пользователей между локальным и облачным каталогами позволяет пользователям получать доступ к ресурсам с помощью одного набора учетных данных. 

Синхронизация — это процесс 

* Создание объекта на основе определенных условий
* Обновление объекта
* Удаление объекта, когда условия больше не выполняются. 

Локальная подготовка включает подготовку из локальных источников (например, Active Directory) в Azure Active Directory (Azure AD). 

## <a name="use-when"></a>Используется, если

Необходимо синхронизировать данные удостоверений из локальных Active Directory сред в Azure AD.

![Схема архитектуры](./media/authentication-patterns/dir-sync-auth.png)

## <a name="components-of-system"></a>Компоненты системы

* **Пользователь**: доступ к приложению с помощью Azure AD.

* **Веб-браузер**. компонент, с которым взаимодействует пользователь, чтобы получить доступ к внешнему URL-адресу приложения.

* **Приложение**: веб-приложение, использующее Azure AD для проверки подлинности и авторизации.

* **Azure AD**: синхронизирует сведения об удостоверениях из локального каталога организации с помощью Azure AD Connect. 

* **Azure AD Connect**: средство для подключения локальных инфраструктур удостоверений к Microsoft Azure AD. Мастер и интерактивные возможности помогут вам развернуть и настроить необходимые компоненты, требуемые для подключения, включая синхронизацию и вход из Active Directory в Azure AD. 

* **Active Directory**. Active Directory — это служба каталогов, входящая в большинство операционных систем Windows Server. Серверы, на которых выполняются службы домен Active Directory Services (AD DS), называются контроллерами домена. Они выполняют проверку подлинности и авторизацию всех пользователей и компьютеров в домене.

## <a name="implement-directory-synchronization-with-azure-ad"></a>Реализация синхронизации службы каталогов с Azure AD

* [Что такое подготовка удостоверений?](../cloud-provisioning/what-is-provisioning.md) 

* [Средства интеграции каталогов гибридных удостоверений](../hybrid/plan-hybrid-identity-design-considerations-tools-comparison.md) 

* [План установки Azure AD Connect](../hybrid/how-to-connect-install-roadmap.md)
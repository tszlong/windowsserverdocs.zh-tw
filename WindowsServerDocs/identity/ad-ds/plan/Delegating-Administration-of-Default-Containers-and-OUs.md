---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: 委派預設容器與 OU 的管理
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1bce2ce312b3105d3c347da1e491601bf15364e7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947686"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>委派預設容器與 OU 的管理

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

每個 Active Directory 網域都包含一組標準的容器和組織單位， (Ou) ，這會在安裝 Active Directory Domain Services (AD DS) 時建立。 這些選項包括：

-   網域容器，做為階層的根容器

-   內建容器，其中包含預設的服務系統管理員帳戶

-   使用者容器，這是在網域中建立的新使用者帳戶和群組的預設位置

-   [電腦] 容器，這是在網域中建立之新電腦帳戶的預設位置

-   網域控制站 OU，這是網域控制站電腦帳戶的電腦帳戶預設位置

樹系擁有者會控制這些預設容器和 Ou。

## <a name="domain-container"></a>網域容器
網域容器是網域階層的根容器。 對原則或存取控制清單所做的變更 (此容器上的 ACL) 可能會影響整個網域。 請勿委派此容器的控制權;它必須由服務系統管理員控制。

## <a name="users-and-computers-containers"></a>使用者和電腦容器
當您執行從 Windows Server 2003 到 Windows Server 2008 的就地網域升級時，現有的使用者和電腦會自動放入 [使用者] 和 [電腦] 容器中。 如果您要建立新的 Active Directory 網域，[使用者和電腦] 容器就是網域中所有新使用者帳戶和非網域控制站電腦帳戶的預設位置。

> [!IMPORTANT]
> 如果您需要委派使用者或電腦的控制權，請勿修改 [使用者和電腦] 容器上的預設設定。 相反地，視需要建立新的 Ou () 並將使用者和電腦物件從其預設容器移至新的 Ou。 視需要委派新 Ou 的控制權。 我們建議您不要修改控制預設容器的人員。

此外，您也無法將群組原則設定套用到預設的 [使用者和電腦] 容器。 若要將群組原則套用至使用者和電腦，請建立新的 Ou，並將使用者和電腦物件移至這些 Ou。 將群組原則設定套用至新的 Ou。

（選擇性）您可以將放在預設容器中的物件建立重新導向至您選擇的容器。

## <a name="well-known-users-and-groups-and-built-in-accounts"></a>知名的使用者和群組，以及內建帳戶
根據預設，會在新的網域中建立數個知名的使用者和群組，以及內建帳戶。 我們建議您將這些帳戶的管理維持在服務系統管理員的控制之下。 請勿將這些帳戶的管理委派給不是服務管理員的個人。 下表列出需要維持在服務系統管理員控制之下的知名使用者和群組，以及內建帳戶。

|知名的使用者和群組|內建帳戶|
|--------------------------------|----------------------|
|Cert Publishers<p>網域控制站<p>Group Policy Creator Owners<p>KRBTGT<p>網域來賓<p>系統管理員<p>網域管理員<p>Schema Admins 僅 (樹系根域) <p>Enterprise Admins 僅 (樹系根域) <p>網域使用者|系統管理員<p>來賓<p>Guests<p>Account Operators<p>Administrators<p>Backup Operators<p>Incoming Forest Trust Builders<p>Print Operators<p>Pre-Windows 2000 Compatible Access<p>Server Operators<p>使用者|

## <a name="domain-controller-ou"></a>網域控制站 OU
當網域控制站新增至網域時，其電腦物件會自動新增至網域控制站 OU。 此 OU 套用了一組預設的原則。 為確保這些原則會一致地套用至所有網域控制站，建議您不要將網域控制站的電腦物件移出此 OU。 如果無法套用預設原則，可能會導致網域控制站無法正常運作。

根據預設，服務管理員會控制此 OU。 請勿將此 OU 的控制權委派給服務系統管理員以外的人員。




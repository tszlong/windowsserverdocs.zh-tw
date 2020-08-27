---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: 委派預設容器與 OU 的管理
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 0ddd4e6853dfaf08cb04157554209f6725b79936
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941458"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>委派預設容器與 OU 的管理

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

每個 Active Directory 網域都包含一組標準的容器和組織單位 (Ou) 在安裝 Active Directory Domain Services (AD DS) 期間建立。 這些選項包括：

-   網域容器，作為階層的根容器

-   包含預設服務系統管理員帳戶的內建容器

-   [使用者] 容器，這是在網域中建立之新使用者帳戶和群組的預設位置

-   電腦容器，這是在網域中建立之新電腦帳戶的預設位置

-   網域控制站 OU，這是網域控制站電腦帳戶之電腦帳戶的預設位置

樹系擁有者會控制這些預設容器和 Ou。

## <a name="domain-container"></a>網域容器
網域容器是網域階層的根容器。 對此容器上 (ACL) 的原則或存取控制清單所做的變更，可能會影響整個網域。 請勿委派此容器的控制權;它必須由服務管理員控制。

## <a name="users-and-computers-containers"></a>使用者和電腦容器
當您從 Windows Server 2003 就地升級至 Windows Server 2008 時，現有的使用者和電腦會自動放入 [使用者] 和 [電腦] 容器中。 如果您要建立新的 Active Directory 網域，[使用者和電腦] 容器是網域中所有新使用者帳戶和非網域控制站電腦帳戶的預設位置。

> [!IMPORTANT]
> 如果您需要將控制權委派給使用者或電腦，請勿修改 [使用者和電腦] 容器上的預設設定。 相反地，視需要建立新的 Ou () ，並將使用者和電腦物件從其預設容器移至新的 Ou。 視需要委派新 Ou 的控制權。 建議您不要修改預設容器的控制物件。

此外，您無法將群組原則設定套用至預設的使用者和電腦容器。 若要將群組原則套用到使用者和電腦，請建立新的 Ou，並將使用者和電腦物件移至這些 Ou。 將群組原則設定套用至新的 Ou。

（選擇性）您可以重新導向要放置在預設容器中的物件建立，以放置在您選擇的容器中。

## <a name="well-known-users-and-groups-and-built-in-accounts"></a>知名的使用者和群組和內建帳戶
依預設，會在新網域中建立數個知名的使用者和群組和內建帳戶。 建議您將這些帳戶的管理維持在服務系統管理員的控制之下。 請勿將這些帳戶的管理委派給非服務管理員的個人。 下表列出知名的使用者和群組，以及必須維持在服務系統管理員控制之下的內建帳戶。

|知名的使用者和群組|內建帳戶|
|--------------------------------|----------------------|
|Cert Publishers<p>網域控制站<p>Group Policy Creator Owners<p>KRBTGT<p>網域來賓<p>系統管理員<p>網域管理員<p>Schema Admins (僅限樹系根域) <p>Enterprise Admins (僅限樹系根域) <p>網域使用者|系統管理員<p>來賓<p>Guests<p>Account Operators<p>系統管理員<p>Backup Operators<p>Incoming Forest Trust Builders<p>Print Operators<p>Pre-Windows 2000 Compatible Access<p>Server Operators<p>使用者|

## <a name="domain-controller-ou"></a>網域控制站 OU
將網域控制站新增至網域時，會自動將其電腦物件新增至網域控制站 OU。 此 OU 已套用一組預設原則。 為了確保這些原則會一致地套用至所有網域控制站，建議您不要將網域控制站的電腦物件移出此 OU。 若無法套用預設原則，可能會導致網域控制站無法正常運作。

依預設，服務管理員會控制此 OU。 請勿將此 OU 的控制權委派給服務系統管理員以外的人員。




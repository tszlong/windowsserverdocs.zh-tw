---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: 委派預設容器與 OU 的管理
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d482854fd82b4bf0d0e61315d36e6222470ca55
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830889"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>委派預設容器與 OU 的管理

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

每個 Active Directory 網域包含一組標準的容器和 Active Directory 網域服務 (AD DS) 安裝期間所建立的組織單位 (Ou)。 包括以下各項：  
  
-   網域控制站，以做為階層的根容器  
  
-   內建的容器，保留預設的服務系統管理員帳戶  
  
-   在網域中建立使用者 容器，也就是新的使用者帳戶和群組的預設位置  
  
-   在網域中建立的電腦 容器，也就是新的電腦帳戶的預設位置  
  
-   網域控制站 OU，也就是網域控制站電腦帳戶的電腦帳戶的預設位置  
  
樹系擁有者可以控制這些預設容器和 Ou。  
  
## <a name="domain-container"></a>網域控制站  
網域容器為根容器之階層的網域。 變更原則或存取控制清單 (ACL) 此容器上的可以有全網域的影響。 無法委派控制此容器中;它必須由服務系統管理員所控制。  
  
## <a name="users-and-computers-containers"></a>使用者和電腦容器  
當您執行從 Windows Server 2003 的網域進行就地升級到 Windows Server 2008 上時，現有的使用者和電腦會自動放入的使用者和電腦 容器。 如果您要建立新的 Active Directory 網域、 使用者和電腦容器就會是所有新的使用者帳戶和網域中的非網域電腦帳戶的預設位置。  
  
> [!IMPORTANT]  
> 如果您要委派的使用者或電腦的控制權時，不會修改使用者和電腦上的預設設定的容器。 相反地，建立新的 Ou （視需要），並移動使用者和電腦物件從其預設容器，並為新的 Ou。 在新的 Ou，控制權，委派所需。 我們建議您不要修改使用者控制項的預設容器。  
  
此外，您無法套用群組原則設定來預設使用者和電腦容器。 若要將群組原則套用至使用者和電腦中，建立新的 Ou 並將使用者和電腦物件移至這些 Ou。 將群組原則設定套用到新的 Ou 中。  
  
（選擇性） 您可以重新導向建立位於預設容器放在您選擇的容器中的物件。  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>已知的使用者和群組和內建帳戶  
根據預設，數個已知的使用者和群組和內建帳戶會建立新的網域中。 我們建議管理這些帳戶會保留服務系統管理員控管。 請勿在不是服務系統管理員的個人中委派管理這些帳戶。 下表列出的已知的使用者和群組和需要的服務系統管理員控制中的內建帳戶。  
  
|已知的使用者和群組|內建帳戶|  
|--------------------------------|----------------------|  
|Cert Publishers<br /><br />網域控制站<br /><br />Group Policy Creator Owners<br /><br />KRBTGT<br /><br />Domain Guests<br /><br />Administrator<br /><br />Domain Admins<br /><br />結構描述系統管理員 （僅樹系根網域）<br /><br />企業系統管理員 （僅樹系根網域）<br /><br />網域使用者|Administrator<br /><br />Guest<br /><br />Guests<br /><br />Account Operators<br /><br />Administrators<br /><br />Backup Operators<br /><br />連入樹系信任產生器<br /><br />Print Operators<br /><br />Pre-Windows 2000 Compatible Access<br /><br />Server Operators<br /><br />使用者|  
  
## <a name="domain-controller-ou"></a>網域控制站 OU  
當網域控制站新增至網域時，他們的電腦物件會自動新增到網域控制站 OU。 這個 OU 有一組預設的原則套用到它。 若要確保這些原則會一致地套用到所有網域控制站，我們建議您不將移出這個 OU 的網域控制站電腦物件。 無法套用預設原則可能會導致網域控制站無法正常運作。  
  
根據預設，服務系統管理員控制這個 OU。 不委派控制這個 OU 的服務系統管理員以外的個人。  
  



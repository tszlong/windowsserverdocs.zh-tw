---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: "預設容器和 Ou 的管理委派"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2504208210c03193451d19478f3bc8c98ec98f23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>預設容器和 Ou 的管理委派

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

每個 Active Directory domain 包含一組標準容器和 Active Directory Domain Services (AD DS) 安裝期間建立組織單位 (Ou)。 其中包括下列動作：  
  
-   網域控制站，做為階層根容器  
  
-   含有預設服務的系統管理員帳號建容器，  
  
-   網域中建立之新使用者帳號及群組的預設位置的使用者容器  
  
-   容器的電腦，這是新電腦帳號的預設位置建立網域中  
  
-   網域控制站組織單位，也就是帳號網域控制站電腦帳號電腦的預設位置  
  
樹系擁有者控制這些預設容器和 Ou。  
  
## <a name="domain-container"></a>網域容器  
網域容器是根容器階層加入網域。 變更原則或此容器存取控制清單 (ACL) 可能有全網域影響。 不委派控制此容器。必須服務系統管理員，控制它。  
  
## <a name="users-and-computers-containers"></a>使用者和電腦容器  
當您執行的 Windows Server 2003 的就地網域升級到 Windows Server 2008 時，現有的使用者及電腦會自動放入的使用者及電腦容器。 如果您要建立新的 Active Directory domain 的使用者與電腦容器的所有新的帳號及非-網域控制站電腦帳號網域中的預設位置。  
  
> [!IMPORTANT]  
> 如果您需要委派使用者或電腦的控制，請不要修改的使用者及電腦的預設設定容器。 請建立新的 Ou （視） 並新 Ou 其預設容器間移動使用者與電腦物件。 所需的新 Ou，控制委派。 我們建議您修改由誰控制預設容器。  
  
此外，您無法適用於群組原則設定預設的使用者與電腦容器。 使用者和電腦適用於群組原則，以建立新的 Ou 並將其中使用者與電腦物件。 適用於群組原則設定的新 Ou。  
  
（選擇性） 您可以重新導向建立位於預設放在您選擇的容器至容器中的物件。  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>已知使用者和群組建帳號  
根據預設，幾個已知使用者和群組、 建帳號會建立新的網域中。 我們建議您管理這些帳號，仍會在控制服務系統管理員。 不委派給不服務系統管理員的個人這些帳號管理。 下表列出的知名使用者和群組和建帳號需要維持受控制的服務系統管理員。  
  
|已知使用者和群組|建帳號|  
|--------------------------------|----------------------|  
|憑證的發行者<br /><br />網域控制站<br /><br />群組原則 Creator 擁有者<br /><br />KRBTGT<br /><br />網域來賓<br /><br />系統管理員<br /><br />網域系統管理員 」<br /><br />架構系統管理員 （僅限樹系根網域）<br /><br />企業的系統管理員 （僅限樹系根網域）<br /><br />使用者網域|系統管理員<br /><br />客體<br /><br />來賓<br /><br />Account 電信業者<br /><br />系統管理員<br /><br />備份電信業者<br /><br />連入森林信任建造商<br /><br />列印電信業者<br /><br />Windows 2000 相容存取<br /><br />伺服器電信業者<br /><br />使用者|  
  
## <a name="domain-controller-ou"></a>網域控制站組織單位  
當網域控制站加入網域時，其電腦物件會自動新增到網域控制站組織單位。 這個組織單位已套用原則的預設設定。 若要確保這些原則一律套用到所有網域控制站，建議您無法將電腦物件的網域控制站退出這個組織單位。 套用預設原則可能會造成網域控制站無法正常運作。  
  
根據預設，服務管理員控制此組織單位。 不委派給服務系統管理員以外的人控制此組織單位。  
  



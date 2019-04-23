---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: 將 AD DS 整合至現有 DNS 基礎結構
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 62405ea9ee38bb3fa457b7731e26fbffb2594797
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891039"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>將 AD DS 整合至現有 DNS 基礎結構

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

如果您的組織已經有現有的網域名稱系統 (DNS) 伺服器服務，Active Directory 網域服務 (AD DS) 的擁有者的 DNS 必須使用您的組織要將 AD DS 整合到現有的基礎結構的 DNS 擁有者。 這牽涉到建立 DNS 伺服器和 DNS 用戶端設定。  
  
## <a name="creating-a-dns-server-configuration"></a>建立的 DNS 伺服器設定  
當 AD DS 整合現有的 DNS 命名空間中，我們建議您下列：  
  
-   樹系中每個網域控制站上安裝 DNS 伺服器服務。 如果其中一部 DNS 伺服器無法使用，這會提供容錯功能。 如此一來，網域控制站不需要依賴其他 DNS 伺服器來進行名稱解析。 因為所有的網域控制站具有一致的組態，這也可簡化管理環境。  
  
-   設定 Active Directory 樹系根網域控制站，裝載 Active Directory 樹系的 DNS 區域。  
  
-   設定裝載對應到其 Active Directory 網域的 DNS 區域的每個地區網域的網域控制站。  
  
-   設定包含 Active Directory 樹系尋找程式記錄的區域 (也就是 _msdcs.&lt。*forestname*區域) 來使用全樹系 DNS 應用程式目錄分割複寫到樹系中每部 DNS 伺服器。  
  
    > [!NOTE]  
    > 使用 Active Directory 網域服務安裝精靈 （我們建議此選項） 安裝 DNS 伺服器服務時，會自動執行所有先前的工作。 如需詳細資訊，請參閱 <<c0> [ 部署 Windows Server 2008 樹系根網域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
    > [!NOTE]  
    > AD DS 會使用全樹系尋找程式記錄，以便找到彼此，並讓用戶端尋找通用類別目錄伺服器的複寫協力電腦。 AD DS 會儲存 _msdcs 全樹系尋找程式記錄。*forestname*區域。 由於區域中的資訊必須廣為，此區域會透過全樹系 DNS 應用程式目錄分割複寫到樹系中的所有 DNS 伺服器。  
  
現有的 DNS 結構會維持不變。 您不需要移動任何伺服器或區域。 您只需要建立 Active Directory 整合 DNS 區域的委派，從現有的 DNS 階層。  
  
## <a name="creating-the-dns-client-configuration"></a>建立 DNS 用戶端設定  
若要設定 DNS 用戶端電腦上，AD DS 擁有者的 DNS 必須指定電腦命名配置和用戶端將如何尋找 DNS 伺服器。 下表列出我們建議的設定，這些設計元素。  
  
|設計元素|組態|  
|------------------|-----------------|  
|電腦命名|使用預設命名。 當 Windows 2000，Windows XP、 Windows Server 2003 時，Windows Server 2008 或 Windows Vista 的電腦加入網域，電腦會指派本身包含電腦的主機名稱的完整的網域名稱 (FQDN) 和作用中的名稱Directory 網域。|  
|用戶端解析程式設定|設定為指向網路上的任何 DNS 伺服器的用戶端電腦。|  
  
> [!NOTE]  
> Active Directory 用戶端和網域控制站可以動態地註冊其 DNS 名稱即使並未指向其名稱的授權 DNS 伺服器。  
  
如果組織之前，以靜態方式註冊的電腦 DNS，或如果組織先前部署的整合式的動態主機設定通訊協定 (DHCP) 解決方案中，電腦可能有不同的現有的 DNS 名稱。 如果用戶端電腦已註冊的 DNS 名稱，以加入網域升級到 Windows Server 2008 AD DS 時，他們將有兩個不同的名稱：  
  
-   現有的 DNS 名稱  
  
-   新的完整的網域名稱 (FQDN)  
  
用戶端仍可以位於任一個的名稱。 任何現有的 DNS、 DHCP 或 DNS/DHCP 整合的解決方案會保留不變。 新的主要名稱是自動建立，且更新透過動態更新。 它們會自動清除藉由清除功能。  
  
如果您想要利用 Kerberos 驗證連接到執行 Windows 2000、windows Server 2003 或 Windows Server 2008 的伺服器時，您必須確定用戶端連接到伺服器所使用的主要名稱。  
  



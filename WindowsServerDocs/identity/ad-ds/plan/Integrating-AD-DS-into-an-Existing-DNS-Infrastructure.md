---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: "整合現有的 DNS 基礎結構 AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ab8d92a237a6d1fb623d9f4bb7dcc88561edf742
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>整合現有的 DNS 基礎結構 AD DS

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您的組織已經有現有的網域名稱系統 」 (DNS) 伺服器服務，Active Directory Domain Services (AD DS) 擁有者 DNS 必須使用 DNS 擁有者為您的組織 AD DS 整合現有的基礎結構。 這牽涉到建立 DNS client 設定和 DNS 伺服器。  
  
## <a name="creating-a-dns-server-configuration"></a>建立 DNS 伺服器設定  
當使用現有的 DNS 命名空間整合 AD DS，我們建議您下列動作：  
  
-   森林中的每個網域控制站上安裝的 DNS 伺服器服務。 如果無法使用其中一個 DNS 伺服器，如此容錯。 如此一來，不需要網域控制站依賴的名稱解析其他 DNS 伺服器。 這也會簡化管理環境因為所有網域控制站統一設定。  
  
-   設定 Active Directory 森林根網域控制站管理 Active Directory 樹系 DNS 區域。  
  
-   設定為每個地區網域裝載對應至 Active Directory 網域 DNS 區域的網域控制站。  
  
-   設定含有 Active Directory 樹系定位器記錄區域 (也就是 _msdcs。*forestname*區域) 來使用樹系 DNS 應用程式 directory 磁碟分割複寫森林中的每個 DNS 伺服器。  
  
    > [!NOTE]  
    > DNS 伺服器服務的 Active Directory Domain Services 安裝精靈 （我們建議使用此選項） 安裝時，會自動執行所有先前的工作。 如需詳細資訊，請查看[部署 Windows Server 2008 森林根網域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
    > [!NOTE]  
    > AD DS 使用樹系定位記錄讓複寫合作夥伴尋找彼此以及讓戶端找不到通用伺服器。 AD DS 儲存的樹系定位器記錄 _msdcs。*forestname*區域。 必須廣泛提供區域中的資訊，因為此區域的樹系 DNS 應用程式 directory 磁碟分割透過會複寫森林中的所有 DNS 伺服器。  
  
現有的 DNS 結構會保持不變。 您不需要的任何伺服器或區域移動。 您只需要從您現有的 DNS 階層 Active Directory 整合 DNS 區域建立委派。  
  
## <a name="creating-the-dns-client-configuration"></a>建立 DNS client 設定  
若要設定 DNS client 電腦上，AD DS 擁有者 DNS 必須指定命名配置和戶端如何找出 DNS 伺服器的電腦。 下表列出我們建議的設定的設計的這些項目。  
  
|設計的項目|設定|  
|------------------|-----------------|  
|電腦命名|使用預設命名。 當 Windows 2000、 Windows XP、 Windows Server 2003 時、 Windows Server 2008 或 Windows vista 的電腦加入網域，電腦指派本身組成主機名稱電腦的完整的網域名稱 (FQDN) 和 Active Directory 網域名稱。|  
|Client 解析程式設定|設定 client 電腦指向任何網路上的 DNS 伺服器。|  
  
> [!NOTE]  
> Active Directory 戶端和網域控制站可以動態登記他們的 DNS 名稱即使您並未指向 DNS 伺服器其名稱的授權。  
  
如果組織之前，靜態登記電腦 DNS 或組織是否先前部署整合動態主機設定通訊協定 」 (DHCP) 方案電腦可能有不同的現有 DNS 名稱。 如果 client 電腦已經且已的 DNS 名稱，當他們加入的網域升級到 Windows Server 2008 AD DS，它們將會有兩個不同的名稱：  
  
-   現有的 DNS 名稱  
  
-   新的完整的網域名稱 (FQDN)  
  
仍然可以找到戶端由任一名稱。 任何的現有 DNS、 DHCP 或整合的 DNS 日 DHCP 方案保留。 新的主要名稱會自動建立和更新透過動態更新。 他們會自動清除透過清除。  
  
如果您想要利用 F:kerberos 驗證時連接到執行 Windows 2000、 Windows Server 2003 或 Windows Server 2008 的伺服器，您必須確定 client 連接到伺服器使用主要名稱。  
  



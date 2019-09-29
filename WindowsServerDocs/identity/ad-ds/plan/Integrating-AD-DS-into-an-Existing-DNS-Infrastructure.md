---
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: 將 AD DS 整合至現有 DNS 基礎結構
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f4bb480be4696f15f0a63c20ab47042264584d2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402558"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>將 AD DS 整合至現有 DNS 基礎結構

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您的組織已經有現有的網域名稱系統（DNS）伺服器服務，Active Directory Domain Services （AD DS）擁有者的 DNS 就必須與您組織的 dns 擁有者合作，將 AD DS 整合到現有的基礎結構中。 這牽涉到建立 DNS 伺服器和 DNS 用戶端設定。  
  
## <a name="creating-a-dns-server-configuration"></a>建立 DNS 伺服器設定  
將 AD DS 與現有的 DNS 命名空間整合時，建議您執行下列動作：  
  
-   在樹系中的每個網域控制站上安裝 DNS 伺服器服務。 如果其中一部 DNS 伺服器無法使用，這會提供容錯功能。 如此一來，網域控制站就不需要依賴其他 DNS 伺服器進行名稱解析。 這也會簡化管理環境，因為所有的網域控制站都有統一的設定。  
  
-   設定 Active Directory 樹系根域控制站，以裝載 Active Directory 樹系的 DNS 區域。  
  
-   設定每個地區網域的網域控制站，以裝載對應至其 Active Directory 網域的 DNS 區域。  
  
-   設定包含 Active Directory 全樹系定位器記錄的區域（也就是 _msdcs）。*forestname*區域）使用全樹系 DNS 應用程式目錄分割來複寫至樹系中的每一部 dns 伺服器。  
  
    > [!NOTE]  
    > 當 DNS 伺服器服務與 Active Directory Domain Services 安裝精靈一併安裝時（我們建議使用此選項），會自動執行所有先前的工作。 如需詳細資訊，請參閱[部署 Windows Server 2008 樹系根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
    > [!NOTE]  
    > AD DS 使用全樹系定位器記錄，讓複寫協力電腦可以找到彼此，並讓用戶端尋找通用類別目錄伺服器。 AD DS 會將全樹系定位器記錄儲存在 _msdcs 中。*forestname*區域。 由於區域中的資訊必須可廣泛使用，因此此區域會透過整個樹系 DNS 應用程式目錄分割，複寫到樹系中的所有 DNS 伺服器。  
  
現有的 DNS 結構會保持不變。 您不需要移動任何伺服器或區域。 您只需要從現有的 DNS 階層建立 Active Directory 整合 DNS 區域的委派。  
  
## <a name="creating-the-dns-client-configuration"></a>建立 DNS 用戶端設定  
若要在用戶端電腦上設定 DNS，AD DS 擁有者的 DNS 必須指定電腦命名配置，以及用戶端將如何找到 DNS 伺服器。 下表列出這些設計項目的建議設定。  
  
|Design 元素|組態|  
|------------------|-----------------|  
|電腦命名|使用預設命名。 當 Windows 2000、Windows XP、Windows Server 2003、Windows Server 2008 或 Windows Vista 電腦加入網域時，電腦會指派完整的功能變數名稱（FQDN），其中包含電腦的主機名稱和作用中的名稱目錄網域。|  
|用戶端解析程式設定|設定用戶端電腦指向網路上的任何 DNS 伺服器。|  
  
> [!NOTE]  
> Active Directory 用戶端和網域控制站可以動態登錄其 DNS 名稱，即使它們不是指向其名稱的授權 DNS 伺服器也一樣。  
  
如果組織先前以靜態方式在 DNS 中註冊電腦，或組織先前已部署整合式動態主機設定通訊協定（DHCP）解決方案，則電腦可能會有不同的現有 DNS 名稱。 如果您的用戶端電腦已經有已註冊的 DNS 名稱，當他們加入的網域升級至 Windows Server 2008 AD DS 時，就會有兩個不同的名稱：  
  
-   現有的 DNS 名稱  
  
-   新的完整功能變數名稱（FQDN）  
  
您仍然可以使用任何名稱來找到用戶端。 任何現有的 DNS、DHCP 或整合的 DNS/DHCP 解決方案都會保持不變。 新的主要名稱會透過動態更新自動建立和更新。 它們會透過清除方式自動清除。  
  
如果您想要在連接到執行 Windows 2000、Windows Server 2003 或 Windows Server 2008 的伺服器時利用 Kerberos 驗證，您必須確定用戶端使用主要名稱連接到伺服器。  
  



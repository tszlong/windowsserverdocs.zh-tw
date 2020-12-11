---
description: 深入瞭解：將 AD DS 整合至現有的 DNS 基礎結構
ms.assetid: 4981b32f-741e-4afc-8734-26a8533ac530
title: 將 AD DS 整合至現有 DNS 基礎結構
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: aba44d8797e6ef77f33afc973b51dfd664f68e71
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042806"
---
# <a name="integrating-ad-ds-into-an-existing-dns-infrastructure"></a>將 AD DS 整合至現有 DNS 基礎結構

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您的組織已有現有的網域名稱系統 (DNS) Server 服務，Active Directory Domain Services (AD DS) 擁有者的 DNS 必須與您組織的 DNS 擁有者合作，才能將 AD DS 整合至現有的基礎結構。 這牽涉到建立 DNS 伺服器和 DNS 用戶端設定。

## <a name="creating-a-dns-server-configuration"></a>建立 DNS 伺服器設定
將 AD DS 與現有的 DNS 命名空間整合時，建議您執行下列動作：

-   在樹系中的每個網域控制站上安裝 DNS 伺服器服務。 如果其中一部 DNS 伺服器無法使用，這會提供容錯功能。 如此一來，網域控制站就不需要依賴其他 DNS 伺服器進行名稱解析。 這也簡化了管理環境，因為所有的網域控制站都有統一的設定。

-   設定 Active Directory 樹系根網域控制站，以裝載 Active Directory 樹系的 DNS 區域。

-   設定每個地區網域的網域控制站，以裝載對應至其 Active Directory 網域的 DNS 區域。

-   設定包含 Active Directory 整個樹系定位器記錄的區域， (也就是 _msdcs。*forestname* 區域) 使用整個樹系的 dns 應用程式目錄分割，複寫到樹系中的每一部 dns 伺服器。

    > [!NOTE]
    > 使用 Active Directory Domain Services 安裝精靈安裝 DNS 伺服器服務時 (建議您) 使用此選項，則會自動執行所有先前的工作。 如需詳細資訊，請參閱 [部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10))。

    > [!NOTE]
    > AD DS 使用全樹系的定位器記錄，讓複寫夥伴可以彼此尋找，並讓用戶端尋找通用類別目錄伺服器。 AD DS 會將整個樹系定位器記錄儲存在 _msdcs 中。*forestname* 區域。 因為區域中的資訊必須廣泛可用，所以會透過整個樹系的 DNS 應用程式目錄分割，將此區域複寫到樹系中的所有 DNS 伺服器。

現有的 DNS 結構會保持不變。 您不需要移動任何伺服器或區域。 您只需要從現有的 DNS 階層建立 Active Directory 整合式 DNS 區域的委派。

## <a name="creating-the-dns-client-configuration"></a>建立 DNS 用戶端設定
若要在用戶端電腦上設定 DNS，AD DS 擁有者的 DNS 必須指定電腦命名配置，以及用戶端將如何尋找 DNS 伺服器。 下表列出我們針對這些設計項目建議的設定。

|設計元素|組態|
|------------------|-----------------|
|電腦命名|使用預設的命名。 當 Windows 2000、Windows XP、Windows Server 2003、Windows Server 2008 或 Windows Vista 電腦加入網域時，電腦會將完整的功能變數名稱指派 (FQDN) ，其中包含電腦的主機名稱和 Active Directory 網域的名稱。|
|用戶端解析程式設定|將用戶端電腦設定為指向網路上的任何 DNS 伺服器。|

> [!NOTE]
> Active Directory 的用戶端和網域控制站可以動態登錄其 DNS 名稱，即使它們沒有指向其名稱的授權 DNS 伺服器也一樣。

如果組織先前已在 DNS 中以靜態方式註冊電腦，或組織先前已部署整合式動態主機設定通訊協定 (DHCP) 解決方案，則電腦可能會有不同的現有 DNS 名稱。 如果您的用戶端電腦已經有已註冊的 DNS 名稱，則將其加入的網域升級為 Windows Server 2008 AD DS 時，將會有兩個不同的名稱：

-   現有的 DNS 名稱

-   新的完整功能變數名稱 (FQDN) 

用戶端仍然可以使用任何一個名稱來找出。 任何現有的 DNS、DHCP 或整合式 DNS/DHCP 解決方案都會保持不變。 新的主要名稱會自動建立，並透過動態更新進行更新。 系統會透過清除來自動清除它們。

如果您想要在連接到執行 Windows 2000、Windows Server 2003 或 Windows Server 2008 的伺服器時利用 Kerberos 驗證，您必須確定用戶端使用主要名稱連接到伺服器。


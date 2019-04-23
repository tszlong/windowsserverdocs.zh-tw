---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: 選取樹系根網域
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c669a5c8103fba52140147957823db978f8b6955
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871349"
---
# <a name="selecting-the-forest-root-domain"></a>選取樹系根網域

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您部署 Active Directory 樹系中的第一個網域稱為樹系根網域。 此網域會保留樹系根網域的 AD DS 部署生命週期。  
  
樹系根網域包含 Enterprise Admins 和 Schema Admins 群組。 這些服務系統管理員群組用來管理樹系層級的作業，例如新增和移除的網域，以及結構描述變更的實作。  
  
選取樹系根網域，牽涉到決定如果其中一個 Active Directory 網域中您網域的設計可以做為樹系根網域，或如果您要部署的專用樹系根網域。  
  
如需部署樹系根網域的相關資訊，請參閱 <<c0> [ 部署 Windows Server 2008 樹系根網域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>選擇地區或專用樹系根網域

如果您要套用單一領域模型，單一網域做為樹系根網域。 如果您要套用多個領域模型，您可以選擇部署的專用樹系根網域，或選取要做為樹系根網域的地區網域中。  
  
### <a name="dedicated-forest-root-domain"></a>專用的樹系根網域

專用樹系根網域是特別建立來做為樹系根網域。 它不包含任何樹系根網域的服務系統管理員帳戶以外的使用者帳戶。 此外，它不代表任何地理區域中您的網域結構。 樹系中的所有其他網域是專用的樹系根網域的子系。  

使用專用樹系根可提供下列優點：  

- 操作樹系服務系統管理員與網域服務系統管理員分開。 在單一網域環境中，Domain Admins 和內建的 Administrators 群組的成員可以使用標準的工具和程序進行本身的 Enterprise Admins 和 Schema Admins 群組的成員。 使用專用樹系根網域的樹系，Domain Admins 和地區網域中的內建系統管理員群組的成員不能自行樹系層級的服務系統管理員群組的成員使用標準的工具和程序。  
- 從其他網域中的操作變更的保護。 專用樹系根網域不代表特定的地理區域，在您的網域結構。 基於這個理由，它不會受到重組或重新命名或重新建構的網域會導致其他變更。  
- 當手寫筆，讓任何國家/地區或區域會顯示屬於另一個區域，可做為中性的根。 某些組織可能會想要避免外觀之一個國家/地區或區域是從屬於另一個國家/地區或區域的命名空間中。 當您使用的專用樹系根網域時，所有的地區網域可以是網域階層中的對等。  

在多重地區網域環境中使用的專用樹系根中，樹系根網域的複寫會對網路基礎結構的影響降到最低。 這是因為樹系根目錄只能裝載的服務系統管理員帳戶。 大部分的其他網域專屬的資料與樹系中的使用者帳戶會儲存在地區網域中。  
  
若要使用的專用樹系根網域的一項缺點是，它會建立額外的管理負荷來支援其他的網域。  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>地區網域為樹系根網域

如果您選擇不要部署的專用樹系根網域，您必須選取地區網域為樹系根網域中運作。 這個網域之父系網域的所有其他區域的網域，以及將您所部署的第一個網域。 樹系根網域包含使用者帳戶，並管理所管理的其他地區網域的方式相同。 主要的差別在於它也包含 Enterprise Admins 和 Schema Admins 群組。  
  
會建立 善用選擇地區網域函式，做為樹系根網域，，它不會建立額外的管理負荷，維護額外的網域。 選取適當的地區網域是樹系根目錄中，例如代表您總部的網域或具有最快的網路連線的區域。 如果很難您的組織選取地區網域的樹系根網域，您可以選擇改為使用專用樹系根模型。  
  
## <a name="assigning-the-forest-root-domain-name"></a>指派的樹系根網域名稱

樹系根網域名稱也是樹系的名稱。 樹系根名稱是 prefix.suffix 的前置詞和後置詞形式所組成的網域名稱系統 (DNS) 名稱。 例如，組織可能會有樹系根名稱 corp.contoso.com。 在此範例中，公司是前置詞，contoso.com 是後置詞。  
  
從您網路上的現有名稱的清單中選取後置詞。 前置詞，選取 有先前未使用過您的網路的新名稱。 藉由將新的前置詞附加至現有的尾碼，您會建立唯一的命名空間。 Active Directory 網域服務 (AD DS) 中建立新的命名空間，可確保任何現有 DNS 基礎結構不需要修改，以配合 AD DS。  
  
### <a name="selecting-a-suffix"></a>選取 後置詞

若要選取樹系根網域的尾碼：  
  
1. 如需組織已註冊的 DNS 尾碼在將裝載 AD DS 在網路上使用，請連絡 DNS 的擁有者。 請注意，在內部網路上使用的尾碼可能不同於外部使用的尾碼。 例如，組織可能會使用 contosopharma.com 網際網路和公司內部網路上的 contoso.com 上。  
  
2. 選取 後的置詞，以搭配 AD DS 的 DNS 擁有者，請參閱。 如果沒有適當的後置詞存在，向 Internet naming 授權單位的新名稱。  
  
我們建議您使用已向 Active Directory 命名空間中的網際網路授權單位的 DNS 名稱。 已註冊的名稱保證是全域唯一的。 如果稍後另一個組織註冊相同的 DNS 網域名稱 （或如果您的組織與合併、 取得，或取得另一家公司使用相同的 DNS 名稱），兩個基礎結構無法彼此互動。  
  
> [!CAUTION]  
> 請勿使用單一標籤 DNS 名稱。 如需詳細資訊，請參閱 < 設定 Windows 針對具有單一標籤 DNS 名稱的網域資訊 ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631))。 此外，我們不建議使用未註冊的尾碼，例如.local。  
  
### <a name="selecting-a-prefix"></a>選取 前置詞

如果您選擇的已註冊的尾碼，已在網路上使用，請使用下表中的前置詞的規則選取樹系根網域名稱的前置詞。 加入目前不是用來建立新的部屬名稱前置詞。 比方說，如果您的 DNS 根名稱為 contoso.com，您可以先建立 Active Directory 樹系根網域名稱 concorp.contoso.com，如果命名空間 concorp.contoso.com 已經不在網路上使用。 這個新的分支，命名空間專用於 AD DS，並可輕鬆整合，與現有的 DNS 實作。  
  
如果您選取來做為樹系根網域的地區網域，您可能需要選取新網域的前置詞。 因為樹系根網域名稱會影響所有樹系中的其他網域名稱，可能不適合根據地區的名稱。 如果您使用新的尾碼，目前不在網路上使用，您可以使用它做為樹系根網域名稱而不選擇其他前置詞。  
  
下表列出選取的已註冊的 DNS 名稱的前置詞的規則。  
  
|規則|說明|  
|--------|---------------|  
|選取不太可能會變成過期的前置詞。|避免使用名稱，例如產品線或可能會在未來變更的作業系統。 我們建議使用泛型名稱，例如公司或 ds。|  
|選取包含僅限網際網路標準字元的前置詞。|A-Z、 a-z、 0-9 和 （-），但不是完全的數值。|  
|包含 15 個字元以內的前置詞。|如果您選擇的前置長度為 15 個字元以內，NetBIOS 名稱等同於前置詞。|  
  
請務必使用組織的 DNS 擁有者以取得將用於 Active Directory 命名空間名稱的擁有權的 Active Directory DNS 擁有者。 如需有關如何設計 DNS 基礎結構才能支援 AD DS 的詳細資訊，請參閱[建立 DNS 基礎結構設計](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。  
  
## <a name="documenting-the-forest-root-domain-name"></a>加入註解的樹系根網域名稱

文件的 DNS 首碼和您選取樹系根網域的尾碼。 此時，找出哪些網域會是樹系根目錄。 您可以將樹系根網域名稱資訊加入您建立用來記錄您的計劃，新的和升級網域及您的網域名稱 「 網域計劃 」 工作表。 若要開啟它，請下載從 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)並開啟 「 網域規劃 」 (DSSLOGI_5.doc)。

---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: 選取樹系根網域
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3fddf5179e2944800d57568f0b8e52262c04cd43
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821901"
---
# <a name="selecting-the-forest-root-domain"></a>選取樹系根網域

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您在 Active Directory 樹系中部署的第一個網域稱為樹系根域。 此網域仍然是 AD DS 部署生命週期的樹系根域。  
  
樹系根域包含 Enterprise Admins 和 Schema Admins 群組。 這些服務系統管理員群組是用來管理樹系層級的作業，例如新增和移除網域，以及對架構進行變更的執行。  
  
選取樹系根域包含判斷網域設計中的其中一個 Active Directory 網域是否可以做為樹系根域，或者您是否需要部署專用樹系根域。  
  
如需部署樹系根域的相關資訊，請參閱[部署 Windows Server 2008 樹系根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>選擇區域或專用樹系根域

如果您要套用單一網域模型，單一網域會當做樹系根域。 如果您要套用多個領域模型，可以選擇部署專用樹系根域，或選取地區網域以作為樹系根域。  
  
### <a name="dedicated-forest-root-domain"></a>專用樹系根域

專用樹系根域是專門為了做為樹系根而建立的網域。 它不包含樹系根域的服務系統管理員帳戶以外的任何使用者帳戶。 此外，它不代表網域結構中的任何地理區域。 樹系中的其他所有網域都是專用樹系根域的子系。  

使用專用樹系根提供下列優點：  

- 樹系服務系統管理員與網域服務系統管理員的操作分離。 在單一網域環境中，Domain Admins 和內建系統管理員群組的成員可以使用標準工具和程式，使其成為 Enterprise Admins 和 Schema Admins 群組的成員。 在使用專用樹系根域的樹系中，網域系統管理員和區域網域內內建系統管理員群組的成員，無法使用標準工具和程式，使其成為樹系層級服務管理員群組的成員。  
- 保護其他網域中的操作變更。 專用樹系根域不代表您網域結構中的特定地理區域。 基於這個理由，它不會受到重組或其他會導致重新命名或重建網域的變更所影響。  
- 做為中性的根，讓國家或地區不會出現在另一個區域的從屬。 有些組織可能會想要避免某個國家或地區屬於命名空間中的另一個國家或地區的外觀。 當您使用專用樹系根域時，所有地區網域都可以是網域階層中的對等。  

在使用專用樹系根的多區域網域環境中，樹系根域的複寫對網路基礎結構的影響最小。 這是因為樹系根目錄只會裝載服務系統管理員帳戶。 樹系中的大部分使用者帳戶和其他網域特定的資料會儲存在地區網域中。  
  
使用專用樹系根域的其中一個缺點是，它會建立額外的管理額外負荷，以支援額外的網域。  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>做為樹系根域的地區網域

如果您選擇不部署專用樹系根域，則必須選取要做為樹系根域的地區網域。 此網域是所有其他地區網域的父系網域，而且將會是您部署的第一個網域。 樹系根域包含使用者帳戶，並以管理其他地區網域的相同方式來管理。 主要的差異在於它也包含 Enterprise Admins 和 Schema Admins 群組。  
  
選取區域網域做為樹系根域的優點是，它不會建立額外的管理額外負荷來維護其他網域的建立。 選取適當的地區網域作為樹系根目錄，例如代表您總部的網域，或具有最快速網路連線的區域。 如果您的組織很難以選取要作為樹系根域的地區網域，您可以選擇改為使用專用的樹系根模型。  
  
## <a name="assigning-the-forest-root-domain-name"></a>指派樹系根功能變數名稱稱

樹系根功能變數名稱稱也是樹系的名稱。 樹系根名稱是一個網域名稱系統（DNS）名稱，其中包含前置詞和後置詞，格式為 prefix. 尾碼。 例如，組織可能會有樹系的根名稱 corp.contoso.com。 在此範例中，corp 是前置詞，而 contoso.com 是尾碼。  
  
從網路上的現有名稱清單中選取尾碼。 在 [前置詞] 中，選取您先前未在網路上使用的新名稱。 將新的前置詞附加至現有的尾碼，即可建立唯一的命名空間。 為 Active Directory Domain Services （AD DS）建立新的命名空間，可確保不需要修改任何現有的 DNS 基礎結構來配合 AD DS。  
  
### <a name="selecting-a-suffix"></a>選取尾碼

若要選取樹系根域的尾碼：  
  
1. 請洽詢組織的 DNS 擁有者，以取得將主控 AD DS 的網路上所使用的已註冊 DNS 尾碼清單。 請注意，內部網路上使用的尾碼可能不同于外部使用的尾碼。 例如，組織可能會在網際網路上使用 contosopharma.com，並在公司內部網路上 contoso.com。  
  
2. 請洽詢 DNS 擁有者，以選取要與 AD DS 搭配使用的尾碼。 如果沒有合適的尾碼存在，請向網際網路命名授權單位註冊新名稱。  
  
我們建議您在 Active Directory 命名空間中使用已向網際網路授權單位註冊的 DNS 名稱。 只有已註冊的名稱保證是全域唯一的。 如果另一個組織稍後註冊相同的 DNS 功能變數名稱（或者，如果您的組織與使用相同 DNS 名稱的另一家公司進行合併、取得或），這兩個基礎結構無法彼此互動。  
  
> [!CAUTION]  
> 請勿使用單一標籤 DNS 名稱。 如需詳細資訊，請參閱使用單一標籤 DNS 名稱為網域設定 Windows 的相關資訊（[https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631)）。 此外，我們不建議使用未註冊的尾碼，例如 local。  
  
### <a name="selecting-a-prefix"></a>選取前置詞

如果您選擇已在網路上使用的已註冊尾碼，請使用下表中的前置詞規則來選取樹系根功能變數名稱稱的前置詞。 新增目前未使用的前置詞，以建立新的次級名稱。 例如，如果您的 DNS 根名稱是 contoso.com，您可以建立 Active Directory 樹系根功能變數名稱 concorp.contoso.com，如果 concorp.contoso.com 的命名空間尚未在網路上使用。 這個新的命名空間分支將專門用來 AD DS，而且可以輕鬆地與現有的 DNS 執行整合。  
  
如果您已選取要作為樹系根域的地區網域，則可能需要為網域選取新的首碼。 因為樹系根功能變數名稱稱會影響樹系中所有其他的功能變數名稱，所以可能不適合使用以區域內為基礎的名稱。 如果您使用的是目前不在網路上使用的新尾碼，您可以使用它做為樹系根功能變數名稱，而不需要選擇額外的前置詞。  
  
下表列出為已註冊的 DNS 名稱選取前置詞的規則。  
  
|規則|說明|  
|--------|---------------|  
|選取不太可能過時的前置詞。|避免未來可能會變更的名稱，例如產品線或作業系統。 我們建議使用一般名稱，例如 corp 或 ds。|  
|選取僅包含網際網路標準字元的首碼。|A-z、a-z、0-9 和（-），但不是完全數值。|  
|在前置詞中包含15個字元或更少。|如果您選擇15個字元或更少的前置長度，則 NetBIOS 名稱會與前置詞相同。|  
  
Active Directory DNS 擁有者必須與組織的 DNS 擁有者合作，才能取得將用於 Active Directory 命名空間的名稱擁有權。 如需設計 DNS 基礎結構以支援 AD DS 的詳細資訊，請參閱[建立 Dns 基礎結構設計](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。  
  
## <a name="documenting-the-forest-root-domain-name"></a>記載樹系根功能變數名稱稱

記錄您為樹系根域選取的 DNS 前置詞和尾碼。 此時，請識別哪個網域會是樹系根目錄。 您可以將樹系根功能變數名稱資訊新增至您所建立的「網域規劃」工作表，以記載新網域和升級網域的計畫，以及您的功能變數名稱。 若要開啟它，請從[適用于 Windows Server 2003 部署套件的作業輔助](https://go.microsoft.com/fwlink/?LinkID=102558)下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services .zip，並開啟「網域規劃」（DSSLOGI_5 .doc）。

---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: "選取 [樹系根網域"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d87273df1e42e1fa88df09e0b86d4c86ea75ee3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="selecting-the-forest-root-domain"></a>選取 [樹系根網域

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

部署 Active Directory 森林中的第一個網域稱為森林根網域。 這個網域會保持生活循環 AD DS 部署的樹系根網域。  
  
森林根網域包含企業系統管理員和架構系統管理員 」 群組。 這些服務的系統管理員群組用來管理森林層級作業，例如加入的網域和實作變更結構描述。  
  
選取 [樹系根網域包括判斷是否 Active Directory 中的網域網域設計的其中一個可做的樹系根網域，或如果您要部署的專用的樹系根網域。  
  
如部署根網域樹系資訊，請查看[部署 Windows Server 2008 森林根網域](https://technet.microsoft.com/library/cc731174.aspx)。  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>選擇一個地區或專用森林根網域  
如果您要套用的單一網域模型，單一網域森林根網域功能。 如果您要套用的多個網域模型，您可以選擇部署專用的樹系根網域，或選取要作為樹系根網域的地區網域。  
  
### <a name="dedicated-forest-root-domain"></a>專用的樹系根網域  
作為樹系根專門建立網域專用的樹系根網域。 它不包含任何帳號以外的樹系根網域服務系統管理員帳號。 同時，不能代表您的網域結構在任何地理區域。 森林中的所有其他網域的專用的樹系根網域中的子女。  
  
使用專用的樹系根提供下列優點：  
  
-   和網域服務系統管理員的樹系服務系統管理員運作分離。 在單一網域環境中，網域系統管理員 」 及建系統管理員群組成員可以使用標準工具和程序可讓企業系統管理員和架構管理員群組成員。 樹系使用專用的樹系根網域網域系統管理員 」 及建地區網域中的系統管理員群組成員能本身的樹系層級服務的系統管理員群組成員使用標準工具和程序。  
  
-   保護電腦免於操作其他網域中的變更。 專用的樹系根網域不能代表您的網域結構在特定地區。 基於這個原因，它不受企業或其他變更重新命名或重建的網域中的結果。  
  
-   做為中性根，才能讓任何國家或地區顯示為屬於另一個地區。 某些組織可能會想要避免外觀該某個國家或地區都是屬於其他國家或地區命名空間。 當您使用專用的樹系根網域時，所有的地區網域可階層網域中的同事。  
  
在多重地區網域環境中使用的專用的樹系根的樹系根網域複寫有網路基礎結構的影響降到最低。 這是因為樹系根只主控服務管理員帳號。 大部分的樹系和其他網域特定資料中帳號會儲存在區域的網域。  
  
使用專用的樹系根網域缺點是，它會建立支援其他網域其他管理成本。  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>森林根網域地區的網域  
如果您選擇不要部署專用的樹系根網域，您必須選取要作為樹系根網域的地區網域。 這個網域是家長網域其他地區網域中的所有，並將您要部署的第一個網域。 森林根網域包含帳號，並在相同的方式，管理其他地區網域管理。 主要不同的是，它也會包括企業系統管理員和架構系統管理員 」 群組。  
  
建立現狀樹系根網域，它並不會建立管理額外費用的維護額外的網域選取地區網域函式的優點。 選取適當的地區網域將樹系根，例如，表示您總部網域或地區的最快速的網路連接。 如果很難以您的組織選取地區網域森林根網域，您可以選擇改為使用專用的樹系根模型。  
  
## <a name="assigning-the-forest-root-domain-name"></a>指派森林根網域名稱  
森林根網域名稱還有樹系的名稱。 森林根名稱是包含前置詞與 prefix.suffix 的形式尾碼網域名稱系統 」 (DNS) 的名稱。 例如，的組織可能有 corp.contoso.com 的樹系根名稱。在此範例中，corp 前置詞，contoso.com 且尾碼。  
  
從您的網路名稱現有的清單中選取尾碼。 選取前置詞未使用您的網路先前的新名稱。 現有的尾碼附加新前置詞，您可以建立唯一命名空間。 建立新的命名空間 Active Directory Domain Services (AD DS) 可確保修改以符合 AD DS 不需要任何的現有 DNS 基礎結構。  
  
### <a name="selecting-a-suffix"></a>選取 [結尾  
選取 [樹系根尾碼：  
  
1.  連絡 DNS 擁有者的清單中使用的網路裝載 AD DS，且已 DNS 尾碼組織。 請注意，可能不同於尾碼使用外部尾碼連絡上使用。 例如，組織可能會使用 contosopharma.com 網際網路上企業連絡 contoso.com 上。  
  
2.  請選取使用的 AD DS 尾碼 DNS 擁有者。 如果不適合尾碼存在，請登記名稱與網際網路命名授權單位。  
  
我們建議您使用的 Active Directory 命名空間網際網路授權單位登記的 DNS 名稱。 只有且已的名稱全域唯一保證。 如果稍後另一個組織暫存器相同 DNS 網域名稱 （或者，如果您的組織混在一起，取得，或由其他公司會取得使用相同的 DNS 名稱），兩個基礎結構無法與另一個互動。  
  
> [!CAUTION]  
> 請勿使用單一標籤 DNS 名稱。 如需詳細資訊，查看 Windows 的單一標籤 DNS 名稱網域設定的相關資訊 ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631))。 此外，我們不建議使用解除的尾碼，例如.local。  
  
### <a name="selecting-a-prefix"></a>選取前置詞  
如果您選擇的且已的尾碼已在網路上使用，如下表所示使用前置詞規則選取前置詞的樹系根網域名稱。 新增不目前用來建立新的附屬名稱前置詞。 例如，如果您的 DNS 根名稱 contoso.com，您可以建立 Active Directory 森林根網域名稱 concorp.contoso.com 如果命名空間 concorp.contoso.com 尚未在網路上使用。 命名空間的這個新分支 」 將會專用到 AD DS，而且可以輕鬆地整合現有 DNS 實作。  
  
如果您選取地區網域為森林根網域運作，您可能需要選取新的網域前置詞。 森林根網域名稱會影響所有森林中的其他網域名稱，因為可能不適當地域根據的名稱。 如果您使用新的尾碼目前不使用網路，您可以使用它做森林根網域名稱而不需要選擇其他前置詞。  
  
下表列出的規則選取前置詞的且已 DNS 名稱。  
  
|規則|解釋|  
|--------|---------------|  
|選取 [前置詞不會變成過時。|避免名稱，例如 product 列或作業系統，可能會在未來的變更。 我們建議使用一般的名稱，例如 corp 或 ds。|  
|選取 [包含只網際網路標準字元前置詞。|A Z、 z，0-9 與 （-），但無法完全數字。|  
|包含的 15 字元或較少中前置詞。|如果您選擇首碼長度或較少的 15 字元，NetBIOS 名稱為前置詞相同。|  
  
請務必的 Active Directory DNS 擁有者搭配組織的 DNS 擁有者以取得中，將會使用 Active Directory 命名空間的名稱。 如需關於設計支援 AD DS DNS 基礎結構的資訊，請查看[建立設計 DNS 基礎架構](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。  
  
## <a name="documenting-the-forest-root-domain-name"></a>記載森林根網域名稱  
文件所選取的樹系根網域尾碼與 DNS 前置詞。 此時，找出所網域會樹系根。 您可以將森林根網域名稱資訊新增到 「 規劃網域 」 試算表，您要建立的全新和已升級網域和您的網域名稱計劃的文件。 若要打開它，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 從工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) 和開放「網域規劃」(DSSLOGI_5.doc).  
  



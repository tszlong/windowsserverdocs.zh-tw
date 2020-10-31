---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: 選取樹系根網域
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 63c405450cca315f4388feb13402b71f207117f2
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070200"
---
# <a name="selecting-the-forest-root-domain"></a>選取樹系根網域

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您在 Active Directory 樹系中部署的第一個網域稱為樹系根域。 此網域仍是 AD DS 部署生命週期的樹系根域。

樹系根域包含 Enterprise Admins 和 Schema Admins 群組。 這些服務系統管理員群組可用來管理樹系層級的作業，例如新增和移除網域，以及對架構所做的變更。

選取樹系根域牽涉到判斷網域設計中的其中一個 Active Directory 網域是否可作為樹系根域，或者您是否需要部署專用的樹系根域。

如需部署樹系根域的詳細資訊，請參閱 [部署 Windows Server 2008 樹系根域](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10))。

## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>選擇區域或專用樹系根域

如果您要套用單一網域模型，則單一網域的功能會是樹系根域。 如果您要套用多個網域模型，您可以選擇部署專用樹系根域，或選取要做為樹系根域的地區網域。

### <a name="dedicated-forest-root-domain"></a>專用樹系根域

專用的樹系根域是專門為了作為樹系根而建立的網域。 它不包含樹系根域的服務系統管理員帳戶以外的任何使用者帳戶。 此外，它不代表網域結構中的任何地理區域。 樹系中的所有其他網域都是專用樹系根域的子系。

使用專用的樹系根提供下列優點：

- 樹系服務系統管理員與網域服務系統管理員的操作分離。 在單一網域環境中，Domain Admins 和內建系統管理員群組的成員可以使用標準工具和程式，使自己成為 Enterprise Admins 和 Schema Admins 群組的成員。 在使用專用樹系根域的樹系中，網域系統管理員的成員和區域網域中的內建系統管理員群組，無法使用標準工具和程式，讓自己成為樹系層級服務管理員群組的成員。
- 保護其他網域中的操作變更。 專用的樹系根域不代表網域結構中的特定地理區域。 基於這個理由，不會受到重組或其他導致重新命名或重建網域的變更所影響。
- 做為中性根，讓任何國家或地區都不會出現在另一個區域。 某些組織可能會想要避免將某個國家或地區附屬於命名空間中的另一個國家或地區的外觀。 當您使用專用的樹系根域時，所有地區網域都可以是網域階層中的對等。

在使用專用樹系根的多重區域網域環境中，樹系根域的複寫會對網路基礎結構造成最大的影響。 這是因為樹系根目錄只會裝載服務系統管理員帳戶。 樹系中的大部分使用者帳戶和其他特定領域的資料都儲存在區域網域中。

使用專用樹系根域的一個缺點是，它會建立額外的管理額外負荷，以支援額外的網域。

### <a name="regional-domain-as-a-forest-root-domain"></a>作為樹系根域的地區網域

如果您選擇不部署專用樹系根域，則必須選取要做為樹系根域的地區網域。 此網域是所有其他地區網域的父系網域，而且將會是您部署的第一個網域。 樹系根域包含使用者帳戶，而且管理方式與其他地區網域的管理方式相同。 主要的差異在於它也包含 Enterprise Admins 和 Schema Admins 群組。

選取地區網域以做為樹系根域的優點是，它不會建立額外的管理額外負荷來維護額外的網域建立。 選取要作為樹系根的適當地區網域，例如代表您總部的網域，或具有最快網路連線的區域。 如果您的組織很難選取要成為樹系根域的地區網域，您可以選擇改用專用的樹系根模型。

## <a name="assigning-the-forest-root-domain-name"></a>指派樹系根功能變數名稱稱

樹系根功能變數名稱稱也是樹系的名稱。 樹系根名稱是網域名稱系統 (DNS) 名稱，其中包含前置詞和尾碼格式的首碼。 例如，組織可能會有樹系根名稱 corp.contoso.com。 在此範例中，corp 是前置詞，而 contoso.com 是尾碼。

從網路上現有的名稱清單中選取尾碼。 在 [首碼] 中，選取先前未在您的網路上使用的新名稱。 將新的前置詞附加至現有的尾碼之後，您就可以建立唯一的命名空間。 建立 Active Directory Domain Services (AD DS 的新命名空間) 可確保不需要修改任何現有的 DNS 基礎結構，以配合 AD DS。

### <a name="selecting-a-suffix"></a>選取尾碼

若要選取樹系根域的尾碼：

1. 請洽詢組織的 DNS 擁有者，以取得將裝載 AD DS 的網路上所使用的已註冊 DNS 尾碼清單。 請注意，內部網路上使用的尾碼可能與外部使用的尾碼不同。 例如，組織可能會在網際網路上使用 contosopharma.com，並在內部的公司網路上 contoso.com。

2. 請參閱 DNS 擁有者，以選取與 AD DS 搭配使用的尾碼。 如果沒有適當的尾碼存在，請使用網際網路命名授權單位來註冊新的名稱。

建議您在 Active Directory 命名空間中使用以網際網路授權單位註冊的 DNS 名稱。 只有已註冊的名稱才保證是全域唯一的。 如果另一個組織稍後註冊相同的 DNS 功能變數名稱 (或者，如果您的組織與另一家使用相同 DNS 名稱的公司取得或取得或取得) ，則這兩個基礎結構無法彼此互動。

> [!CAUTION]
> 請勿使用單一標籤 DNS 名稱。 如需詳細資訊，請參閱 [使用單一標籤 DNS 名稱設定的 Active Directory 網域的部署和操作](https://support.microsoft.com/help/300684/)。 此外，我們不建議使用未註冊的尾碼，例如 local。

### <a name="selecting-a-prefix"></a>選取前置詞

如果您選擇已在網路上使用的已註冊尾碼，請使用下表中的前置詞規則，選取樹系根功能變數名稱的前置詞。 新增目前未用來建立新次級名稱的前置詞。 例如，如果您的 DNS 根目錄名稱是 contoso.com，您可以建立 Active Directory 樹系根功能變數名稱 concorp.contoso.com （如果命名空間 concorp.contoso.com 尚未在網路上使用）。 命名空間的這個新分支將專門用來 AD DS，並可透過現有的 DNS 執行輕鬆地整合。

如果您選取要做為樹系根域的地區網域，您可能需要為該網域選取新的前置詞。 由於樹系根功能變數名稱會影響樹系中的所有其他功能變數名稱，因此可能不適合以地區為基礎的名稱。 如果您使用目前未在網路上使用的新尾碼，您可以使用它做為樹系根功能變數名稱，而不選擇額外的前置詞。

下表列出為已註冊的 DNS 名稱選取前置詞的規則。

| 規則     | 說明 |
| -------- | --------------- |
| 請選取不太可能過期的前置詞。 | 避免未來可能變更的名稱，例如產品線或作業系統。 我們建議使用一般名稱，例如 corp 或 ds。|
| 選取只包含網際網路標準字元的前置詞。 | A-z、a-z、0-9 和 ( ) ，但不是完全數值。 |
| 在前置詞中包含15個字元或更少。 | 如果您選擇的前置長度為15個字元或更少，則 NetBIOS 名稱與前置詞相同。 |

Active Directory DNS 擁有者必須與組織的 DNS 擁有者合作，才能取得將用於 Active Directory 命名空間的名稱擁有權，是很重要的。 如需設計 DNS 基礎結構以支援 AD DS 的詳細資訊，請參閱 [建立 Dns 基礎結構設計](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。

## <a name="documenting-the-forest-root-domain-name"></a>記載樹系根功能變數名稱稱

記錄您為樹系根域選取的 DNS 前置詞和尾碼。 此時，請找出哪個網域將會是樹系根。 您可以將樹系根功能變數名稱資訊新增至您所建立的「網域規劃」工作表，以記錄新網域和升級網域的計畫和功能變數名稱。 若要開啟它，請從 [Windows Server 2003 部署套件的工作輔助程式](https://microsoft.com/download/details.aspx?id=9608) 下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟 [網域規劃] ( # A1) 。

---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: 指派網域名稱
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ec66353802b31432d182c1538c0d2f5322f457af
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624396"
---
# <a name="assigning-domain-names"></a>指派網域名稱

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您必須將名稱指派給方案中的每個網域。 Active Directory Domain Services （AD DS）網域有兩種類型的名稱：網域名稱系統（DNS）名稱和 NetBIOS 名稱。 一般使用者都可以看到這兩個名稱。 Active Directory 網域的 DNS 名稱包含兩個部分：前置詞和後置詞。 建立功能變數名稱時，請先決定 DNS 首碼。 這是網域 DNS 名稱中的第一個標籤。 當您選取樹系根域的名稱時，會決定尾碼。 下表列出 DNS 名稱的前置詞命名規則。

|規則|說明|
|--------|---------------|
|選取不太可能過時的前置詞。|避免未來可能會變更的名稱，例如產品線或作業系統。 我們建議使用地理名稱。|
|選取僅包含網際網路標準字元的首碼。|A-z、a-z、0-9 和（-），但不是完全數值。|
|在前置詞中包含15個字元或更少。|如果您選擇15個字元或更少的前置長度，則 NetBIOS 名稱會與前置詞相同。|

如需詳細資訊，請參閱[電腦、網域、網站和 ou 的 Active Directory 中的命名慣例](https://support.microsoft.com/help/909264/)。

> [!NOTE]
> 雖然 Windows Server 2008 和 Windows Server 2003 中的 Dcpromo.exe 可以讓您建立單一標籤的 DNS 網域名稱，但是因為一些原因，您不應該將單一標籤的 DNS 名稱用於網域。 在 Windows Server 2008 R2 中，Dcpromo.exe 不允許您為網域建立單一標籤的 DNS 名稱。 如需詳細資訊，請參閱[部署和操作使用單一標籤 DNS 名稱設定的 Active Directory 網域](https://support.microsoft.com/help/300684/)。

如果網域的目前 NetBIOS 名稱不適合代表區域，或無法滿足前置詞命名規則，請選取新的前置詞。 在此情況下，網域的 NetBIOS 名稱與網域的 DNS 首碼不同。

針對您部署的每個新網域，選取適用于該區域且滿足前置詞命名規則的前置詞。 我們建議網域的 NetBIOS 名稱與 DNS 首碼相同。

記錄您為樹系中每個網域選取的 DNS 前置詞和 NetBIOS 名稱。 您可以將 DNS 和 NetBIOS 名稱資訊新增至您所建立的「網域規劃」工作表，以記錄新網域和升級網域的計畫。 若要開啟它，請從[適用于 Windows Server 2003 部署套件的作業輔助](https://microsoft.com/download/details.aspx?id=9608)下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services .zip，並開啟「網域規劃」（DSSLOGI_5 .doc）。

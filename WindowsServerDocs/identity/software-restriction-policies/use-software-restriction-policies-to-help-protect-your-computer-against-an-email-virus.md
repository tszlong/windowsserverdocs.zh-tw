---
title: 使用軟體限制原則來協助保護電腦免於電子郵件病毒
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02f23979-f832-4e46-bdea-21fd77db35b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 41b4c2399a86ef96d34b62295eda4a1ce9300609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850669"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>使用軟體限制原則來協助保護電腦免於電子郵件病毒

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題提供如何設定應用程式控制原則來協助保護電腦免於電子郵件病毒開始使用 Windows Server 2008 和 Windows Vista 中使用軟體限制原則 (SRP) 的資訊。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 您可以使用軟體限制原則來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 這些會與 Microsoft Active Directory 網域服務和群組原則整合，但也可以在獨立電腦上設定。 如需 SRP 的起始點，請參閱[軟體限制原則](software-restriction-policies.md)。

從 Windows Server 2008 R2 和 Windows 7 開始，Windows AppLocker 可以用於取代或搭配 SRP 應用程式控制策略的一部分。 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>設定以協助防範電子郵件病毒的破壞的 SRP

1.  檢閱軟體限制原則，以了解 SRP 的運作方式的最佳作法。

    -   [最佳作法](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [軟體限制原則的運作方式](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  開啟 [軟體限制原則]。

    -   [為您的本機電腦](administer-software-restriction-policies.md#BKMK_1)

    -   [對於網域，站台或組織單位，而且您會在成員伺服器上或在已加入網域的工作站上](administer-software-restriction-policies.md#BKMK_2)

3.  如果您尚未定義軟體限制原則，建立新的軟體限制原則。

    -   [若要建立新的軟體限制原則](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  建立電子郵件程式用來執行電子郵件附件的資料夾的路徑規則，並將安全性層級**不允許**。

    -   [使用路徑規則](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  指定要套用規則的檔案類型。

    -   [若要加入或刪除指定的檔案類型](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  修改原則設定，讓它們套用至的使用者和您想要的群組：

    -   指定使用者或您不要群組原則物件的 (GPO) 的群組来套用原則設定。

    -   排除的特定原則設定群組原則中的軟體限制原則的本機系統管理員，仍有其他的群組原則套用到系統管理員。

        -   [若要防止軟體限制原則套用至本機系統管理員](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  測試原則。



---
title: 使用軟體限制原則來協助保護電腦免於電子郵件病毒
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4c691255683eb37eecdbeaa55c094b7ce5c4e26d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357646"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>使用軟體限制原則來協助保護電腦免於電子郵件病毒

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供的資訊說明如何使用軟體限制原則（SRP）來設定應用程式控制原則，以協助保護您的電腦免于從 Windows Server 2008 和 Windows Vista 開始的電子郵件病毒。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 您可以使用軟體限制原則來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 這些會與 Microsoft Active Directory Domain Services 和群組原則整合，但也可以在獨立電腦上設定。 如需 SRP 的起點，請參閱[軟體限制原則](software-restriction-policies.md)。

從 Windows Server 2008 R2 和 Windows 7 開始，Windows AppLocker 可以用來取代或與 SRP 搭配使用，以進行部分的應用程式控制策略。 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>設定 SRP 以協助防範電子郵件病毒

1.  請參閱軟體限制原則的最佳作法，以瞭解 SRP 的運作方式。

    -   [最佳作法](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [軟體限制原則的工作方式](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  開啟 [軟體限制原則]。

    -   [針對您的本機電腦](administer-software-restriction-policies.md#BKMK_1)

    -   [適用于網域、網站或組織單位，而且您位於已加入網域的成員伺服器或工作站上。](administer-software-restriction-policies.md#BKMK_2)

3.  如果您先前尚未定義軟體限制原則，請建立新的軟體限制原則。

    -   [若要建立新的軟體限制原則](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  為您的電子郵件程式用來執行電子郵件附件的資料夾建立路徑規則，然後將安全性等級設為 [不**允許**]。

    -   [使用路徑規則](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  指定要套用規則的檔案類型。

    -   [若要新增或刪除指定的檔案類型](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  修改原則設定，使其適用于您想要的使用者和群組：

    -   指定您不想要套用群組原則物件（GPO）原則設定的使用者或群組。

    -   將本機系統管理員從群組原則中特定原則設定的軟體限制原則中排除，並繼續將其餘的群組原則套用至系統管理員。

        -   [防止軟體限制原則套用至本機系統管理員](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  測試原則。



---
title: 使用軟體限制原則來協助保護電腦免於電子郵件病毒
description: Windows Server 安全性
ms.topic: article
ms.assetid: 02f23979-f832-4e46-bdea-21fd77db35b2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 502d9a097928c6a9b828ebc3b9d5b3544d456388
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640230"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>使用軟體限制原則來協助保護電腦免於電子郵件病毒

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供的資訊說明如何使用軟體限制原則 (SRP) 來設定應用程式控制原則，以協助保護您的電腦免于從 Windows Server 2008 和 Windows Vista 開始的電子郵件病毒。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是以群組原則為依據的功能，可以識別在網域的電腦上執行的軟體程式，並控制執行這些程式的能力。 您可以使用軟體限制原則來建立高限制性的電腦設定，您可以只允許執行特別識別出的應用程式。 這些都與 Microsoft Active Directory Domain Services 和群組原則整合，但也可以在獨立電腦上進行設定。 如需 SRP 的起點，請參閱 [軟體限制原則](software-restriction-policies.md)。

從 Windows Server 2008 R2 和 Windows 7 開始，您可以使用 Windows AppLocker 來取代或搭配 SRP 使用，以取得應用程式控制策略的一部分。

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>設定 SRP 以防止電子郵件病毒

1.  請參閱軟體限制原則的最佳作法，以瞭解 SRP 的運作方式。

    -   [最佳做法](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [軟體限制原則的運作方式](/previous-versions/windows/it-pro/windows-server-2003/cc786941(v=ws.10))

2.  開啟 [軟體限制原則]。

    -   [針對本機電腦](administer-software-restriction-policies.md#BKMK_1)

    -   [如果是網域、網站或組織單位，而且您是在成員伺服器上，或在已加入網域的工作站上](administer-software-restriction-policies.md#BKMK_2)

3.  如果您先前未定義軟體限制原則，請建立新的軟體限制原則。

    -   [建立新的軟體限制原則](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  為您的電子郵件程式用來執行電子郵件附件的資料夾建立路徑規則，然後將安全性等級設定為 [不 **允許**]。

    -   [使用路徑規則](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  指定要套用規則的檔案類型。

    -   [新增或刪除指定的檔案類型](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  修改原則設定，使其適用于您想要的使用者和群組：

    -   指定您不想要套用群組原則物件 (GPO) 原則設定的使用者或群組。

    -   在群組原則中，從特定原則設定的軟體限制原則中排除本機系統管理員，但仍將其餘的群組原則套用至系統管理員。

        -   [防止軟體限制原則套用至本機系統管理員](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  測試原則。

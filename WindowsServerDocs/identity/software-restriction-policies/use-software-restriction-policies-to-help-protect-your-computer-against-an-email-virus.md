---
title: "為了協助保護您的電腦免受電子郵件病毒使用軟體限制原則"
description: "Windows Server 安全性"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>為了協助保護您的電腦免受電子郵件病毒使用軟體限制原則

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供如何設定應用程式控制項原則來協助保護您的電腦免受與 Windows Server 2008 和 Windows Vista 的電子郵件病毒開始使用軟體限制原則 (SRP) 的資訊。

## <a name="introduction"></a>簡介
軟體限制原則 (SRP) 是群組原則的功能辨識中加入網域的電腦上執行的軟體程式，以及控制執行這些程式的能力。 您可以使用軟體限制原則來建立高度限制的電腦，您可讓只專門辨識應用程式執行設定。 這些整合在一起 Microsoft Active Directory Domain Services 及群組原則，但是您也可以在獨立的電腦上設定。 針對 SRP 的起點，請查看[軟體限制原則](software-restriction-policies.md)。

開始使用 Windows Server 2008 R2 和 Windows 7、 Windows AppLocker 可用於而不是或 SRP 搭配您的應用程式控制項策略的一部分。 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>設定可協助抵禦電子郵件病毒的 SRP

1.  檢視軟體限制原則，以了解如何運作 SRP 最佳做法。

    -   [最佳做法](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [軟體限制原則的運作方式](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  打開軟體限制原則。

    -   [適用於您的電腦](administer-software-restriction-policies.md#BKMK_1)

    -   [網域網站或組織單位，而且您的成員伺服器或已經加入網域工作站](administer-software-restriction-policies.md#BKMK_2)

3.  如果您尚未定義軟體限制原則，建立新的軟體限制原則。

    -   [若要建立新的軟體限制原則](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  建立路徑規則執行電子郵件附件，您的電子郵件程式會使用的資料夾，然後將安全性設定層級到**不允許]**。

    -   [工作的路徑規則](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  指定規則適用於的檔案類型。

    -   [若要新增或 delete 指定的檔案類型](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  修改原則設定，讓使用者和群組您想要適用於：

    -   指定的使用者或群組您不要群組原則物件的 (GPO) 來套用原則設定。

    -   本機系統管理員排除群組原則中的特定原則設定的軟體限制原則與仍然有其他的群組原則套用到系統管理員。

        -   [若要防止軟體限制原則套用到本機系統管理員](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  測試原則。



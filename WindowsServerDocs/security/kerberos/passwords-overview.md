---
title: 密碼概觀
description: Windows Server 安全性
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 1b917691f931836605cfe044c725f5200925d2ca
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621886"
---
# <a name="passwords-overview"></a>密碼概觀

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題適用于 IT 專業人員，說明 Windows 作業系統中所使用的密碼，以及如何在認證管理原則中使用密碼的相關檔和討論。

## <a name="feature-description"></a><a name="BKMK_OVER"></a>功能描述
現今的作業系統和應用程式都是以密碼為架構，即使您使用智慧卡或生物特徵辨識系統，所有帳戶仍會有密碼，但在某些情況下仍可使用。 某些帳戶（特別是用來執行服務的帳戶）甚至不能使用智慧卡和生物特徵辨識權杖，因此必須使用密碼來進行驗證。 Windows 會使用密碼編譯雜湊來保護密碼。

如需 Windows 密碼的詳細資訊，請參閱 [密碼技術總覽](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994558(v=ws.10))。

## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用
在 Windows 及許多其他作業系統中，驗證使用者身分識別最常見的方法是使用秘密複雜密碼或密碼。 保護您的網路環境會要求所有使用者都必須使用強式密碼。 這有助於避免惡意使用者透過手動方法或使用工具來猜測弱式密碼的威脅，以取得遭盜用的使用者帳戶的認證。 尤其是系統管理帳戶的情況。 當您定期變更複雜密碼時，它會降低密碼攻擊危及該帳戶的可能性。

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>新功能和變更的功能
在 Windows Server 2012 和 Windows 8 中，圖片密碼是新的。 圖片密碼是與一系列手勢結合的使用者選取影像的組合。 已加入網域的電腦上已停用圖片密碼功能 \- 。 如需圖片密碼的詳細資訊連結， [請參閱](#BKMK_LINKS) 下方的。

Windows Server 2012 和 Windows 8 中的密碼功能沒有任何變更。 未新增任何新的群組原則設定。 不過，認證和密碼管理中的增強功能和增強功能 \( \) ，例如使用圖片密碼、認證保險箱，以及使用 Microsoft 帳戶登入 Windows 8 （先前稱為 WINDOWS Live ID）。

## <a name="deprecated-functionality"></a><a name="BKMK_DEP"></a>已被取代的功能
Windows Server 2012 和 Windows 8 中未淘汰任何密碼功能。

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>軟體需求
在企業環境中，通常會使用 Active Directory Domain Services 來管理密碼。 您也可以使用 [本機安全性設定]、[帳戶原則] 和 [密碼原則] 中的設定，在本機電腦上管理密碼。

## <a name="see-also"></a><a name="BKMK_LINKS"></a>另請參閱
下表列出密碼功能、技術和認證管理的其他資源。

|內容類型|參考|
|--------|-------|
|**案例文件**|[保護您的數位識別身分](https://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**作業**|[Active Directory 使用者及電腦](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754217(v=ws.11))|
|**疑難排解**|[瞭解您的密碼何時過期 \- Active Directory PowerShell Blog](https://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**安全性**| Windows Server 2008 R2 和 Windows 7 [威脅與對策指南：帳戶原則](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125920(v=ws.10))<p>[變更和建立強式密碼](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)的指引|
|**工具及設定**|[Microsoft 下載中心上的 Windows 和 Windows Server 群組原則設定參考](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**社群資源**|[保護您的數位識別身分](https://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<p>[使用 Windows Live ID 登入 Windows 8](https://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<p>[使用圖片密碼登入](/archive/blogs/b8/signing-in-with-a-picture-password)<p>[優化圖片密碼安全性](/archive/blogs/b8/optimizing-picture-password-security)|
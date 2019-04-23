---
title: 密碼概觀
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6c1b8d56b5c0da738e7dae5c0072be81040f90d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869599"
---
# <a name="passwords-overview"></a>密碼概觀

>適用於：Windows Server （半年通道），Windows Server 2016

本主題適用於 IT 專業人員說明 Windows 作業系統和文件和討論有關使用密碼認證管理策略中的連結中所使用的密碼。

## <a name="BKMK_OVER"></a>功能描述
作業系統和應用程式目前架構的轉變密碼，而且即使您使用智慧卡或生物特徵辨識系統，所有帳戶仍都有密碼，並仍可以使用在某些情況下。 部分帳戶，值得注意的是用來執行服務的帳戶智慧卡和生物特徵辨識的語彙基元，甚至無法使用，並因此必須使用密碼來進行驗證。 Windows 會保護使用加密編譯雜湊的密碼。

如需有關 Windows 密碼的詳細資訊，請參閱[密碼技術概觀](https://technet.microsoft.com/library/hh994558(WS.10).aspx)。

## <a name="BKMK_APP"></a>實際的應用程式
在 Windows 和許多其他作業系統，來驗證使用者的身分識別的最常見方法是使用複雜密碼。 保護您的網路環境需要強式密碼可供所有使用者。 這有助於防止惡意使用者猜測弱式密碼，無論是透過手動方法，或藉由使用工具，來取得遭盜用的使用者帳戶的認證的威脅。 這特別適用於系統管理帳戶。 當您定期變更複雜密碼時，它會降低危害該帳戶的密碼攻擊的可能性。

## <a name="BKMK_NEW"></a>新功能和變更
在 Windows Server 2012 和 Windows 8，圖片密碼是新功能。 圖片密碼是使用者的所選映像，搭配一系列的筆勢的組合。 圖片密碼功能已停用網域\-加入的電腦。 圖片密碼的詳細資訊的連結所述[另請參閱](#BKMK_LINKS)如下。

已在 Windows Server 2012 和 Windows 8 中的密碼功能不會變更。 已不新增任何新的群組原則設定。 不過，改進和增強功能已在認證\(和密碼\)管理，例如使用圖片密碼、 認證保險箱和具有 Microsoft 帳戶登入 Windows 8，先前稱為 Windows Live ID.

## <a name="BKMK_DEP"></a>已被取代的功能
沒有密碼的功能已被取代，Windows Server 2012 和 Windows 8 中。

## <a name="BKMK_SOFT"></a>軟體需求
在企業環境中，通常會與 Active Directory 網域服務管理密碼。 也可以使用本機安全性設定]、 [帳戶原則，密碼原則中設定本機電腦上管理密碼。

## <a name="BKMK_LINKS"></a>另請參閱
下表列出密碼功能的其他資源的技術和認證管理。

|內容類型|參考|
|--------|-------|
|**案例文件**|[保護您的數位身分識別](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**操作**|[Active Directory 使用者和電腦](https://technet.microsoft.com/library/cc754217.aspx)|
|**疑難排解**|[了解您的密碼到期時\-Active Directory PowerShell 部落格](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**安全性**| Windows Server 2008 R2 和 Windows 7[威脅與對策指南：帳戶原則](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />指引[變更，並建立強式密碼](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**工具及設定**|[Windows 和 Microsoft 下載中心上的 Windows Server 的群組原則設定參考](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**社群資源**|[保護您的數位身分識別](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[在 Windows 8 與 Windows Live ID 登入](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[登入的圖片密碼](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[最佳化圖片密碼安全性](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|



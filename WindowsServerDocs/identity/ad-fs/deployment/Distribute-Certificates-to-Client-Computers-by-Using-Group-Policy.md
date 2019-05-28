---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: 使用群組原則發佈給用戶端電腦的憑證
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 11cdd9c75ca588ebeac9387e6512fee439621bf8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192165"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>使用群組原則發佈給用戶端電腦的憑證


您可以使用下列程序，將資料放入適當的安全通訊端層\(SSL\)憑證\(或對等項目憑證鏈結至信任的根\)針對帳戶同盟伺服器，資源同盟伺服器，並在帳戶夥伴樹系中，使用群組原則的每部用戶端電腦的網頁伺服器。  
  
中的成員資格**Domain Admins**或是**Enterprise Admins**，或同等權限，在 Active Directory 網域服務中\(AD DS\) ，至少需要完成此程序。  檢閱詳細使用適當帳戶和群組成員[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=83477\)。   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>將憑證分送到用戶端電腦使用群組原則  
  
1.  帳戶夥伴組織的樹系中的網域控制站，啟動**群組原則管理**貼齊\-中。  
  
2.  找不到現有的群組原則物件\(GPO\)或建立新的 GPO 包含的憑證設定。 請確定 GPO 的網域、 網站或組織單位相關聯\(OU\)適當的使用者和電腦帳戶所在的位置。  
  
3.  右\-按一下 [GPO]，然後按一下**編輯**。  
  
4.  在主控台樹狀目錄中，開啟**電腦組態\\原則\\Windows 設定\\安全性設定\\公開金鑰原則**，以滑鼠右鍵\-按一下**受信任的根憑證授權單位**，然後按一下**匯入**。  
  
5.  在 [**歡迎使用憑證匯入精靈**頁面上，按一下**下一步]** 。  
  
6.  在上**匯入檔案**頁面上，輸入適當的憑證檔案的路徑\(，例如\\ \\fs1\\c$\\fs1.cer\)，然後按一下**下一步** 。  
  
7.  在上**憑證存放區**頁面上，按一下**將所有憑證都放入以下的存放**，然後按一下**下一步** 。  
  
8.  在 **完成憑證匯入精靈**頁面上，確認您所提供的資訊正確無誤後，，然後按一下**完成**。  
  
9. 重複步驟 2 至 6，針對每一個同盟伺服器陣列中加入其他憑證。  

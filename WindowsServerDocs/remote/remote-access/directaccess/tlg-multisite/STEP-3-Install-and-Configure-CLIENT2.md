---
title: 步驟 3 的安裝和設定 CLIENT2
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d19f204e139433d11cf674c4ec39a134cde7eefa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813169"
---
# <a name="step-3-install-and-configure-client2"></a>步驟 3 的安裝和設定 CLIENT2

>適用於：Windows Server （半年通道），Windows Server 2016

CLIENT2 是 Windows 7&reg;電腦，可用來示範回溯相容性的 Windows Server 2016 伺服器上執行的遠端存取。  
  
1. 在 CLIENT2 上安裝作業系統。 安裝 Windows&reg; 7 Enterprise 或 Windows&reg;在 CLIENT2 上的 7 Ultimate。  
  
2. CLIENT2 加入 CORP 網域。 CLIENT2 加入 corp.contoso.com 網域中。  
  
## <a name="to-install-the-operating-system-on-client2"></a>在 CLIENT2 上安裝作業系統  
  
1.  啟動 Windows 7 安裝。  
  
2.  當系統提示您提供使用者名稱時，輸入**User1**。 當系統提示您輸入電腦名稱時，輸入**CLIENT2**。  
  
3.  當系統提示您輸入密碼時，輸入強式密碼兩次。  
  
4.  當系統提示您的保護設定時，按一下**使用建議的設定**。  
  
5.  當系統提示您為您的電腦目前的位置時，按一下**的工作網路**。  
  
6.  CLIENT2 連接至具有網際網路存取的網路，並執行 Windows Update，以安裝適用於 Windows 7 的最新的更新，然後中斷網際網路連線。  
  
7.  連線到公司網路子網路的 CLIENT2。  
  
## <a name="user-account-control"></a>使用者帳戶控制  
當您設定 Windows 7 作業系統時，您必須按一下 **繼續**上**使用者帳戶控制**(UAC) 對話方塊中，針對某些工作。 數個組態工作需要 UAC 核准。 當系統提示您時，隨時按一下**繼續**授權這些變更。  
  
## <a name="to-join-client2-to-the-corp-domain"></a>CLIENT2 加入 CORP 網域  
  
1.  按一下 [開始] ，在 [電腦] 上按一下滑鼠右鍵，然後按一下 [內容] 。  
  
2.  在上**系統**頁面上，於**電腦名稱、 網域及工作群組設定**區域中，按一下 **變更設定**。  
  
3.  在 [系統內容] 的 [電腦名稱] 索引標籤上，按一下 [變更]。  
  
4.  上**電腦名稱/網域變更** 對話方塊中，按一下**網域**，型別**corp.contoso.com**，然後按一下**確定**。  
  
5.  當系統提示您輸入使用者名稱和密碼時，輸入使用者名稱和 User1 網域帳戶的密碼，然後按一下**確定**。  
  
6.  當您看見對話方塊並顯示 corp.contoso.com 網域的歡迎頁面時，按一下 [確定]。  
  
7.  當您看到對話方塊，提示您重新啟動電腦，請按一下**確定**。  
  
8.  在 **系統屬性** 對話方塊中，按一下**關閉**，，您會看到一個對話方塊，提示您重新啟動電腦，請按一下 **立即重新啟動**。  
  
9. 在電腦重新啟動之後，以 corp\user1 的身分登入。

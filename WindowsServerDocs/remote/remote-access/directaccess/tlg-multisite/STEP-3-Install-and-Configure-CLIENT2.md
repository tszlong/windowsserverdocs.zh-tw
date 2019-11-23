---
title: 步驟3安裝和設定 CLIENT2
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b2d8cb1538de35a97ae83888d26abd7ad7bc8b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388324"
---
# <a name="step-3-install-and-configure-client2"></a>步驟3安裝和設定 CLIENT2

>適用於：Windows Server (半年通道)、Windows Server 2016

CLIENT2 是一部 Windows 7&reg; 的電腦，用來示範在 Windows Server 2016 伺服器上執行之遠端存取的回溯相容性。  
  
1. 在 CLIENT2 上安裝作業系統。 在 CLIENT2 上安裝 Windows&reg; 7 Enterprise 或 Windows&reg; 7 旗艦版。  
  
2. 將 CLIENT2 加入 CORP 網域。 將 CLIENT2 加入 corp.contoso.com 網域。  
  
## <a name="to-install-the-operating-system-on-client2"></a>在 CLIENT2 上安裝作業系統  
  
1.  開始安裝 Windows 7。  
  
2.  當系統提示您輸入使用者名稱時，請輸入**User1**。 當系統提示您輸入電腦名稱稱時，請輸入**CLIENT2**。  
  
3.  當系統提示您輸入密碼時，請輸入強式密碼兩次。  
  
4.  當系統提示您輸入保護設定時，請按一下 [**使用建議的設定**]。  
  
5.  當系統提示您輸入電腦目前的位置時，請按一下 [**工作網路**]。  
  
6.  將 CLIENT2 連線至具有網際網路存取權的網路，並執行 Windows Update 以安裝 Windows 7 的最新更新，然後中斷網際網路連線。  
  
7.  將 CLIENT2 連線到公司網路子網。  
  
## <a name="user-account-control"></a>使用者帳戶控制  
當您設定 Windows 7 作業系統時，您必須在某些工作的 [**使用者帳戶控制**（UAC）] 對話方塊上，按一下 [**繼續**]。 有數個設定工作需要 UAC 核准。 當系統提示您時，請務必按一下 [**繼續**] 以授權這些變更。  
  
## <a name="to-join-client2-to-the-corp-domain"></a>將 CLIENT2 加入 CORP 網域  
  
1.  按一下 [開始]，在 [電腦]上按一下滑鼠右鍵，然後按一下 [內容]。  
  
2.  在 [**系統**] 頁面的 [**電腦名稱稱、網域及工作組設定**] 區域中，按一下 [**變更設定**]。  
  
3.  在 [系統內容] 的 [電腦名稱] 索引標籤上，按一下 [變更]。  
  
4.  在 [**電腦名稱稱/網域變更**] 對話方塊中，按一下 [**網域**]，輸入**Corp.contoso.com**，然後按一下 **[確定]** 。  
  
5.  當系統提示您輸入使用者名稱和密碼時，請輸入 User1 網域帳戶的使用者名稱和密碼，然後按一下 **[確定]** 。  
  
6.  當您看見對話方塊並顯示 corp.contoso.com 網域的歡迎頁面時，按一下 [確定]。  
  
7.  當您看到提示您重新開機電腦的對話方塊時，請按一下 **[確定**]。  
  
8.  在 [**系統**內容] 對話方塊上，按一下 [**關閉**]，當您看到提示您重新開機電腦的對話方塊時，請按一下 [**立即重新開機**]。  
  
9. 在電腦重新開機之後，以 CORP\User1. 的身分登入

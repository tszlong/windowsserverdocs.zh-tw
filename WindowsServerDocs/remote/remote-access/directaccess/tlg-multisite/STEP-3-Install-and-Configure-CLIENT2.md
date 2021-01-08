---
title: 步驟3安裝和設定 CLIENT2
description: 瞭解如何安裝和設定 CLIENT2。
manager: brianlic
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: e64b54edec32ea7944c04d088fa0a4ac983f98b2
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040468"
---
# <a name="step-3-install-and-configure-client2"></a>步驟3安裝和設定 CLIENT2

>適用於：Windows Server (半年度管道)、Windows Server 2016

CLIENT2 是 Windows 7 &reg;  電腦，用來示範在 Windows Server 2016 伺服器上執行的遠端存取回溯相容性。

1. 在 CLIENT2 上安裝作業系統。 &reg;在 CLIENT2 上安裝 windows 7 Enterprise 或 windows &reg; 7 旗艦版。

2. 將 CLIENT2 聯結至 CORP 網域。 將 CLIENT2 聯結至 corp.contoso.com 網域。

## <a name="to-install-the-operating-system-on-client2"></a>在 CLIENT2 上安裝作業系統

1.  開始安裝 Windows 7。

2.  當系統提示您輸入使用者名稱時，輸入 **User1**。 當系統提示您輸入電腦名稱稱時，請輸入 **CLIENT2**。

3.  當系統提示您輸入密碼時，請輸入強式密碼兩次。

4.  當系統提示您提供保護設定時，請按一下 [ **使用建議的設定**]。

5.  當系統提示您輸入電腦的目前位置時，請按一下 [ **工作網路**]。

6.  將 CLIENT2 連線到可存取網際網路的網路，然後執行 Windows Update 安裝 Windows 7 的最新更新，然後中斷與網際網路的連線。

7.  將 CLIENT2 連線到公司網路子網。

## <a name="user-account-control"></a>使用者帳戶控制
當您設定 Windows 7 作業系統時，您必須在某些工作的 [**使用者帳戶控制**] ([UAC) ] 對話方塊中按一下 [**繼續**]。 有幾個設定工作需要 UAC 核准。 當系統提示您時，請務必按一下 [ **繼續** ]，以授權這些變更。

## <a name="to-join-client2-to-the-corp-domain"></a>將 CLIENT2 加入 CORP 網域

1.  按一下 [開始]，在 [電腦] 上按一下滑鼠右鍵，然後按一下 [內容]。

2.  在 [ **系統** ] 頁面的 [ **電腦名稱稱、網域及工作組設定** ] 區域中，按一下 [ **變更設定**]。

3.  在 [系統內容] 的 [電腦名稱] 索引標籤上，按一下 [變更]。

4.  在 [ **電腦名稱稱/網域變更** ] 對話方塊中，按一下 [ **網域**]，輸入 **Corp.contoso.com**，然後按一下 **[確定]**。

5.  當系統提示您輸入使用者名稱和密碼時，輸入 User1 網域帳戶的使用者名稱和密碼，然後按一下 **[確定]**。

6.  當您看見對話方塊並顯示 corp.contoso.com 網域的歡迎頁面時，按一下 [確定]。

7.  當您看到提示您重新開機電腦的對話方塊時，按一下 **[確定]**。

8.  在 [ **系統** 內容] 對話方塊中，按一下 [ **關閉**]，當您看到提示您重新開機電腦的對話方塊時，按一下 [ **立即重新開機**]。

9. 電腦重新開機之後，以 CORP\User1. 登入。

---
title: "電腦加入新的 Windows Server Essentials server1"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cdfa9504-9881-4265-b308-c7ee8721bfaa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1a67cda9e4b04e8d861232b48f45915fb2b460d1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="join-computers-to-the-new-windows-server-essentials-server1"></a>電腦加入新的 Windows Server Essentials server1

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

##  <a name="BKMK_JoinComputers"></a>   
 移轉程序的下一個步驟是 client 電腦加入新的 Windows Server Essentials 網路和更新的群組原則設定。  
  
> [!NOTE]
>  如果 client 的電腦已經加入來源伺服器，您必須第一次解除連接器電腦上的軟體 client 之前，您可以將電腦連接到目的伺服器。  
  
 Client 電腦連接到伺服器的程序也適用於已加入網域，或非加入網域的電腦。  
  
-   瀏覽] **http://***目的地-伺服器名稱***連接 /**並安裝 Windows Server 連接器軟體如如果這是新的電腦。  
  
> [!NOTE]
>  Windows Server 連接器軟體不支援執行 Windows XP 或 Windows Vista 的電腦。 如果您執行的是 Windows XP 或 Windows Vista 已經加入網域的電腦，您可以略過此步驟。  
  
### <a name="ensure-that-group-policy-has-updated"></a>確定已更新 [群組原則  
  
> [!NOTE]
>  這是選擇性的步驟，並只需要如果來源伺服器設定使用資料夾重新導向例如自訂群組原則設定。  
  
 來源伺服器和目的地伺服器仍然 online 時，您應該確定設定已複寫目的地伺服器 client 電腦群組原則。 在每個 client 的電腦上，執行下列步驟：  
  
1.  打開在命令提示字元視窗。  
  
2.  在命令提示字元中，輸入**GPRESULT /R**，然後按 ENTER 鍵。  
  
3.  檢視其輸出套用群組原則中的區段:，確定它會列出目的伺服器，例如**DestinationSrv.Domain.local**。 例如：  
  
    ```  
    USER SETTINGS  
    --------------  
        CN=User,OU=Users,DC=DOMAIN,DC=Local  
        Last time Group Policy was applied: 1/24/2011 at 1:26:27 PM  
        Group Policy was applied from:      DestinationSrv.Domain.local  
        Group Policy slow link threshold:   500 kbps  
        Domain Name:                        Domain  
        Domain Type:                        Windows 2011  
  
    ```  
  
4.  如果未列出目的伺服器，在命令提示字元中，輸入**gpupdate /force**，然後按 enter 鍵來重新整理群組原則設定。 然後再試一次執行先前的程序。  
  
5.  如果仍未出現目的伺服器，可能會在群組原則設定錯誤或套用至特定 client 電腦發生錯誤。 如果未出現目的伺服器，請執行下列步驟：  
  
    1.  按一下**[開始]**，按一下 [**執行**，輸入**rsop.msc**（設定原則結果），然後按 ENTER 鍵。  
  
    2.  展開樹上的 [X 與，直到您取得節點。  
  
    3.  以滑鼠右鍵按一下 [] 節點，然後按一下**檢視錯誤**群組原則設定失敗列出的電腦上的原因相關資訊。

---
title: 將電腦加入新的 Windows Server Essentials 的伺服器
description: 說明如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d94de050-3300-4323-a5ea-c824cb9cecc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 48703ed78ee7d604e67be06b540d4206617d4578
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318987"
---
# <a name="join-computers-to-the-new-windows-server-essentials-network1"></a>將電腦加入新的 Windows Server Essentials 的伺服器

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

##  <a name="BKMK_JoinComputers"></a>   
 遷移程式的下一個步驟是將用戶端電腦加入新的 Windows Server Essentials 網路，並更新群組原則設定。  
  
### <a name="domain-joined-client-computers"></a>加入網域的用戶端電腦  
 請瀏覽到 **http://** <em>destination-servername</em> **/connect**，以如同新電腦的方式安裝 Windows Server 連接器軟體。 不論是已加入網域或未加入網域的用戶端電腦，安裝程序均相同。  
  
> [!NOTE]
>  Windows Server 連接器軟體不支援執行 Windows XP 或 Windows Vista 的電腦。 如果您有執行 Windows XP 或 Windows Vista 的電腦已經加入網域，您可以略過此步驟。  
  
### <a name="non-domain-joined-client-computers"></a>未加入網域的用戶端電腦  
 請瀏覽到 **http://** <em>destination-servername</em> **/connect**，以如同新電腦的方式安裝 Windows Server 連接器軟體。 不論是已加入網域或未加入網域的用戶端電腦，安裝程序均相同。  
  
> [!NOTE]
>  Windows Server 連接器軟體不支援執行 Windows XP 或 Windows Vista 的電腦。 如果您有執行 Windows XP 或 Windows Vista 的電腦已經加入網域，您可以略過此步驟。  
  
### <a name="ensure-that-group-policy-has-updated"></a>確定已更新群組原則  
  
> [!NOTE]
>  這是選用步驟，只有在使用自訂群組原則設定 (例如資料夾重新導向) 來設定來源伺服器時才需要。  
  
 當來源伺服器與目的地伺服器仍保持線上狀態時，您應該確保群組原則設定已從目的地伺服器複寫到用戶端電腦上。 請在每部用戶端電腦上執行下列步驟：  
  
1.  開啟 [命令提示字元] 視窗。  
  
2.  在命令提示字元中輸入 **GPRESULT /R**，然後按 Enter。  
  
3.  檢查套用的群組原則區段所產生的輸出：並確定它會列出目的地伺服器，例如**列示 destinationsrv.domain.local**。 例如，  
  
    ```  
    USER SETTINGS  
    --------------  
        CN=User,OU=Users,DC=DOMAIN,DC=Local  
        Last time Group Policy was applied: 1/24/2011 at 1:26:27 PM  
        Group Policy was applied from:      DestinationSrv.Domain.local  
        Group Policy slow link threshold:   500 kbps  
        Domain Name:                        Domain  
        Domain Type:                        Windows 2008  
  
    ```  
  
4.  如果未列出目的地伺服器，請在命令提示字元中，輸入 **gpupdate /force**，然後按 ENTER 重新整理群組原則設定。 然後重新執行先前的程序。  
  
5.  如果目的地伺服器仍未出現，可能是群組原則設定中發生錯誤，或將群組原則設定套用至此特定用戶端電腦時發生錯誤。 如果目的地伺服器沒有出現，請執行下列步驟：  
  
    1.  按一下 [開始]、[執行]，輸入 **rsop.msc** (原則的結果集)，然後按 ENTER。  
  
    2.  展開具有 X 的樹狀結構，直到到達節點為止。  
  
    3.  以滑鼠右鍵按一下節點，並按一下 [檢視錯誤]，了解群組原則設定為何在列示的電腦上失敗。

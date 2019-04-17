---
title: "廣告樹系修復-抓取操作主要角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adfs
ms.openlocfilehash: 7ca6e3746586feeb3573b1ad6ba02831d1f5addd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>廣告樹系修復-抓取作業主角  

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 使用下列程序抓取作業主角 （也稱為彈性的單一主機操作 (FSMO) 角色）。 您可以使用 Ntdsutil.exe，會自動安裝所有網域控制站在命令列工具。  
  
## <a name="to-seize-an-operations-master-role"></a>若要抓取作業主角  
  
1.  在命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：  
  
    ```  
    ntdsutil  
    ```  
  
2.  在**ntdsutil:**提示，輸入下列命令，然後按 ENTER 鍵：  
  
    ```  
    roles  
    ```  
  
3.  在**FSMO 維護：**提示，輸入下列命令，然後按 ENTER 鍵：  
  
    ```  
    connections  
    ```  
  
4.  在**伺服器連接：**提示，輸入下列命令，然後按 ENTER 鍵：  
  
    ```  
    Connect to server ServerFQDN  
    ```  
  
     其中*ServerFQDN*是完整的網域名稱 (FQDN)，此 dc，例如：**連接伺服器 nycdc01.example.com**。  
  
     如果*ServerFQDN*未成功，使用 NetBIOS DC 的名稱。  
  
5.  在**伺服器連接：**提示，輸入下列命令，然後按 ENTER 鍵：  
  
    ```  
    quit  
    ```  
  
6.  根據您要抓取的角色， **FSMO 維護：**提示輸入適當的命令下表中所述，然後按 ENTER 鍵。  
  
    |角色|認證|命令|  
    |----------|-----------------|-------------|  
    |網域命名主機|企業系統管理員|**抓取命名主機**|  
    |架構主機|架構系統管理員|**抓取架構主機**|  
    |基礎結構主機**請注意：**您抓取基礎結構主角之後，您可能會收到一則錯誤稍後如果您需要執行 Adprep /Rodcprep。 如需詳細資訊，查看知識庫文章[949257](https://support.microsoft.com/kb/949257)。|網域系統管理員 」|**抓取基礎結構主機**|  
    |Pdc 模擬器|網域系統管理員 」|**抓取 pdc**|  
    |RID 的主要|網域系統管理員 」|**抓取 rid 的主機**|  
  
     在確認此要求之後，嘗試轉移 Active Directory 或 AD DS。 轉送失敗時，顯示的一些資訊時發生錯誤，並 Active Directory 或 AD DS 繼續進行抓取。 抓取完成後，就會出現的角色與輕量型 Directory 存取通訊協定 (LDAP) 的名稱會保留目前每個角色伺服器清單。 您也可以執行**Netdom 查詢 FSMO**在已提升權限的命令提示字元驗證目前角色位置。  
  
    > [!NOTE]
    >  如果這部電腦不是 RID 主機失敗之前，您嘗試抓取 RID 主角電腦就會與複寫合作夥伴接受此角色前進行同步處理。 不過，因為隔離電腦時執行此步驟，它就會失敗與協力廠商同步處理中。 因此，會顯示對話方塊中，詢問您是否要繼續使用許可與合作夥伴同步無法這台電腦操作。 按一下**[是]**。  
  
## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)

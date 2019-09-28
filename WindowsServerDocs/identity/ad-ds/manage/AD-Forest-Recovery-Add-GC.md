---
title: AD 樹系復原-新增 GC
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: f82033dd042847c7c735423c25756b936b137230
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369336"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>AD 樹系復原-新增 GC

>適用於：Windows Server 2016、Windows Server 2012 及 2012 R2、Windows Server 2008 和 2008 R2

請使用下列程式將通用類別目錄新增至 DC。  
  
## <a name="to-add-the-global-catalog"></a>新增通用類別目錄  
  
1. 按一下 [**開始**]，指向 [**所有程式**]，指向 [系統**管理工具**]，然後按一下 [ **Active Directory 網站和服務**]。  
2. 在主控台樹中，展開 [ **Sites** ] 容器，然後選取包含目標伺服器的適當網站。  
3. 展開 [**伺服器**] 容器，然後展開您要新增通用類別目錄之 DC 的伺服器物件。  
4. 以滑鼠右鍵按一下 [ **NTDS 設定**]，然後按一下 [**屬性**]。  
5. 選取 [**通用類別目錄**] 核取方塊。  
![Add GC @ no__t-1

## <a name="to-add-the-global-catalog-using-repadmin"></a>使用 Repadmin 新增通用類別目錄  

- 開啟提升許可權的命令提示字元，輸入下列命令，然後按 ENTER 鍵：  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

下列方法可加速將通用類別目錄新增至根域中 DC 的程式：  

- 在理想的情況下，根域中的 DC 應該是非根域中還原之 Dc 的複寫協力電腦。 若是如此，請確認知識一致性檢查程式（KCC）已為來源 DC 和根 DC 中的分割區建立對應的**repsFrom**物件。 您可以藉由執行**repadmin/showreps/v**命令來確認此項。 

- 如果未建立任何**repsFrom**物件，請為設定分割區建立此物件。 如此一來，根域中的 DC 就可以判斷非根域中的 dc 已被刪除。 您可以使用下列命令來執行這項操作：  

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE  
   ```

   *SourceDomainControllerCNAME*的格式為：  

   ```
  
   sourceDCGuid._msdcs.root domain  
   ```

   例如，contoso.com 網域之設定分割區的 repadmin/add 命令可能是：  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- 如果**repsFrom**物件存在，請嘗試將根域中的 dc 與非根域中的 dc 同步，如下所示：  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   其中*DestinationDomainController*是根域中的 dc，而*SourceDomainController*是非根域中還原的 dc。 

- 根域 DNS 伺服器應具有來源 DC 的別名（CNAME）資源記錄。 請確定父 DNS 區域包含子領域中正確 Dc （已從備份還原的 Dc）的委派資源記錄（名稱伺服器（NS）和主機（A）資源記錄）。 
- 請確定根域中的 DC 正在聯繫非根域中的正確金鑰發佈中心（KDC）。 若要測試這種情況，請在命令提示字元中輸入下列命令，然後按 ENTER 鍵：  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)  

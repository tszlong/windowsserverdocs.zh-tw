---
title: AD 樹系復原-新增 GC
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 156a4a64d9c3bb8261bd603b72ae11b81ff1d152
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825629"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>AD 樹系復原-新增 GC

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

您可以使用下列程序，將通用類別目錄新增至 DC。  
  
## <a name="to-add-the-global-catalog"></a>若要新增通用類別目錄  
  
1. 按一下 **開始**，指向**所有程式**，指向**系統管理工具**，然後按一下**Active Directory 站台及服務**。  
2. 在主控台樹狀目錄中，依序展開**站台**容器，然後選取適當的站台，其中包含目標伺服器。  
3. 依序展開**伺服器**容器，然後展開您要新增通用類別目錄 dc 的 伺服器物件。  
4. 以滑鼠右鍵按一下**NTDS 設定**，然後按一下**屬性**。  
5. 選取 **通用類別目錄**核取方塊。  
![新增 GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)

## <a name="to-add-the-global-catalog-using-repadmin"></a>若要新增使用 Repadmin 通用類別目錄  

- 開啟提升權限的命令提示字元中，輸入下列命令，然後按 ENTER:  

   ```  
   repadmin.exe /options DC_NAME +IS_GC  
   ```  

以下是如何加速到根網域中的 DC 新增通用類別目錄的程序：  

- 在理想情況下，在根網域中的 DC 應該還原的網域控制站複寫協力電腦，在非根目錄的網域中。 如果是這樣，確認已建立對應的知識一致性檢查程式 (KCC) **repsFrom**來源 DC 和 DC 的根目錄中的資料分割物件。 您可以藉由執行確認**repadmin /showreps /v**命令。 

- 如果沒有任何**repsFrom**物件建立，請建立此組態分割的物件。 如此一來，在根網域中的 DC 可以判斷哪一個網域控制站的非根網域中已被刪除。 您可以使用下列命令來這麼做：  

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE  
   ```

   格式*SourceDomainControllerCNAME*是：  

   ```
  
   sourceDCGuid._msdcs.root domain  
   ```

   比方說，repadmin / 加入 contoso.com 網域的設定資料分割可能會命令：  

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
   ```

- 如果**repsFrom**物件存在，嘗試同步處理的非根網域中的 DC 根網域中的 DC，如下所示：  

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
   ```

   何處*DestinationDomainController*是根網域中的 DC 並*SourceDomainController*是根網域中還原的網域控制站。 

- 根網域的 DNS 伺服器應該有來源 DC 的別名 (CNAME) 資源記錄。 請確認父系 DNS 區域包含委派資源記錄 （名稱伺服器 (NS) 和主機 (A) 資源記錄） 正確的網域控制站 (Dc 已從備份還原) 的子區域中。 
- 請確定根網域中的 DC 正在連絡正確金鑰發佈中心 (KDC) 的非根網域中。 若要測試時，在命令提示字元中，輸入下列命令，並再按 ENTER 鍵：  

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force  
   ```

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)  

---
title: "廣告樹系修復-新增 GC"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 3749fd99737f61c55871f613b9feaa21090d3bae
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---adding-the-gc"></a>廣告樹系修復-新增 GC 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 通用新增到 DC 使用下列程序。  
  
## <a name="to-add-the-global-catalog"></a>若要新增的通用  
  
1.  按一下**[開始]**，指向 [**所有程式**，指向 [**系統管理工具]**，，然後按一下**Active Directory 網站和服務**。  
2.  在主控台中，展開**網站**容器、，然後選取包含目標伺服器的適當網站。  
3.  展開**伺服器]**容器，然後針對您要新增的通用網域控制站展開伺服器物件。  
4.  以滑鼠右鍵按一下**NTDS 設定**，然後按**屬性**。  
5.  選取 [**通用**核取方塊。  
![新增 GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)
  
## <a name="to-add-the-global-catalog-using-repadmin"></a>若要新增通用使用 Repadmin  
  
1.  打開提升權限的命令提示字元中，輸入下列命令，並按下 ENTER:  
  
    ```  
    repadmin.exe /options DC_NAME +IS_GC  
    ```  
  
 加快通用加入 DC 根網域中的程序的方法如下：  
  
-   最好根網域中的 DC 應該複寫合作夥伴還原網域控制站的非根網域中。 若是如此，請確認知識一致性檢查程式 (KCC) 適當的對應建立的**repsFrom**來源 DC 和的磁碟分割中根俠物件。 您可以執行確認**repadmin /showreps /v**命令。  
  
-   如果有任何**repsFrom**物件建立、建立磁碟分割設定為這個物件。 如此一來，根網域中的俠可以判斷已經非根網域中的網域控制站。 您可以使用下列命令：  
  
    ```  
    repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME  
    ```  
  
    ```  
    repadmin /options DSA -Disable_NTDSCONN_XLATE  
    ```  
  
     適用於格式*SourceDomainControllerCNAME*是：  
  
    ```  
  
    sourceDCGuid._msdcs.root domain  
    ```  
  
     例如，可能是設定 contoso.com 網域的磁碟分割 repadmin /add 命令：  
  
    ```  
    repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com  
    ```  
  
-   如果**repsFrom**物件的話，請試著同步根網域中的 DC 與 DC 非根網域中，如下所示：  
  
    ```  
    Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID  
    ```  
  
     其中*DestinationDomainController*是 DC 根網域中的，*SourceDomainController*是還原網域控制站非根網域中的。  
  
-   根網域 DNS 伺服器應該會有來源俠資源記錄別名 (CNAME)。 請確定的父系 DNS 區域包含委派資源（名稱伺服器（奈秒）和主機 (A) 資源記錄）正確網域控制站 (已從備份還原 Dc) 中的子女區域。  
  
-   請務必根網域中俠洽詢往來正確金鑰 Distribution 中心 (KDC) 非根網域中。 若要進行測試，在命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：  
  
    ```  
    nltest /dsgetdc:nonroot domain name /KDC /Force  
    ```  
## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)  

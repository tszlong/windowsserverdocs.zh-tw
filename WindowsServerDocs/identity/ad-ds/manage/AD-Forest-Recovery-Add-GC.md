---
description: 深入瞭解： AD 樹系復原-新增 GC
title: AD 樹系復原-新增 GC
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: ed64cf4830fb47da58190721f5b1fe056bc4e4a2
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043006"
---
# <a name="ad-forest-recovery---adding-the-gc"></a>AD 樹系復原-新增 GC

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用下列程式，將通用類別目錄新增至 DC。

## <a name="to-add-the-global-catalog"></a>新增通用類別目錄

1. 按一下 [ **開始**]，指向 [ **所有程式**]，指向 [系統 **管理工具**]，然後按一下 [ **Active Directory 網站和服務**]。
2. 在主控台樹中，展開 [ **網站** ] 容器，然後選取包含目標伺服器的適當網站。
3. 展開 [ **伺服器** ] 容器，然後展開您要新增通用類別目錄之 DC 的伺服器物件。
4. 以滑鼠右鍵按一下 [ **NTDS 設定**]， **然後按一下 [** 內容]。
5. 選取 [ **通用類別目錄** ] 核取方塊。
![新增 GC](media/AD-Forest-Recovery-Add-GC/addgc1.png)

## <a name="to-add-the-global-catalog-using-repadmin"></a>使用 Repadmin 新增通用類別目錄

- 開啟提升許可權的命令提示字元，輸入下列命令，然後按 ENTER 鍵：

   ```
   repadmin.exe /options DC_NAME +IS_GC
   ```

以下是加快將通用類別目錄新增至根域中之 DC 的程式的方法：

- 在理想的情況下，根域中的 DC 應該是非根域中已還原之 Dc 的複寫夥伴。 如果是的話，請確認知識一致性檢查程式 (KCC) 已針對根 DC 中的來源 DC 和磁碟分割建立對應的 **repsFrom** 物件。 您可以執行 **repadmin/showreps/v** 命令來確認這一點。

- 如果未建立任何 **repsFrom** 物件，請為設定分割區建立此物件。 如此一來，根域中的 DC 就可以判斷在非根域中刪除了哪些 Dc。 您可以使用下列命令來執行這項操作：

   ```
   repadmin /add ConfigurationNamingContext DestinationDomainController SourceDomainControllerCNAME
   ```

   ```
   repadmin /options DSA -Disable_NTDSCONN_XLATE
   ```

   *SourceDomainControllerCNAME* 的格式為：

   ```

   sourceDCGuid._msdcs.root domain
   ```

   例如，contoso.com 網域的設定磁碟分割的 repadmin/add 命令可能是：

   ```
   repadmin /add cn=configuration,DC=contoso,DC=com DC01 937ef930-7356-43c8-88dc-8baaaa781cf6._msdcs.dDSP17A22.contoso.com
   ```

- 如果 **repsFrom** 物件存在，請嘗試將根域中的 dc 與非根域中的 dc 同步，如下所示：

   ```
   Repadmin /sync DomainNamingContext DestinationDomainController SourceDomainControllerGUID
   ```

   其中 *DestinationDomainController* 是根域中的 dc，而 *SourceDomainController* 是在非根域中還原的 dc。

- 根域 DNS 伺服器的別名 (CNAME) 來源 DC 的資源記錄。 確定父 DNS 區域包含 (名稱伺服器的委派資源記錄 (NS) 和主機 () 資源記錄) 針對正確的 Dc (已從子領域中的備份) 還原的 Dc。
- 請確定根域中的 DC 正在連接非根域中的正確金鑰發佈中心 (KDC) 。 若要測試這種情況，請在命令提示字元中輸入下列命令，然後按 ENTER 鍵：

   ```
   nltest /dsgetdc:nonroot domain name /KDC /Force
   ```

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)

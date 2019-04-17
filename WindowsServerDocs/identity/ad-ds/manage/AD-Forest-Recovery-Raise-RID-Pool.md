---
title: "廣告樹系修復引發移除集區"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adfs
ms.openlocfilehash: e6b5dc8b9c0b701fe2cd1b0c88f7edc22802c393
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>廣告樹系修復-引發提供 RID 集區的值 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2
 
 使用下列程序提高值的相關 ID (RID) 集區 RID 操作主機，將會配置之後 DC 還原。 提高提供 RID 集區的值，您就可以確保不俠該配置 RID 的已用來還原網域備份後建立的安全性原則。  
 
## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>有關 Active Directory 移除集區與 rIDAvailablePool
 每個網域有物件**DATA-CN = RID 管理員 $DATA-CN = 系統特區**=<*domain_name*>。 此物件具有屬性名為**rIDAvailablePool**。 此屬性的值維護整個網域中的全域 RID 空間。 值是大型整數與上下部分。 上半定義的安全性原則，可以針對每個網域 （0x3FFFFFFF 或超過 1 億） 配置。 下方為 Rid 有尚未配置網域中的數字。  
  
> [!NOTE]
>  在 Windows Server 2016 和 2012年，可以配置的安全性原則的數目增加超過 2 億到。 如需詳細資訊，請查看[排除管理發行](https://technet.microsoft.com/library/jj574229.aspx)。  
  
-   範例值： 4611686014132422708  
  
-   低一部分： 2100 （開頭配置的下一步 RID 集區）  
  
-   上方： 1073741823 （總數 Rid，您可以建立網域中）  
  
 當您提高大整數的值時，也會增加少部分的值。 例如，您新增 100000 範例 4611686014132422708 4611686014132522708 總和的值，新少部分 102100。 這表示而配置 RID 主機的下一步 RID 集區的開頭 102100 而不是 2100年。  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator--"></a>若要提高提供 RID 集區 adsiedit 和小算盤使用的值 '  
1.  打開伺服器管理員中，按一下**工具**，按一下 [ **Adsi**。    
2.  以滑鼠右鍵按一下，選取**連接到**，並連接執行預設命名操作，按**[確定]**。
![編輯 ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. 瀏覽至下列分辨的名稱路徑： **DATA-CN = RID 管理員 $DATA-CN = 系統特區 =<domain name>**。
![編輯 ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3.  以滑鼠右鍵按一下並選取的屬性 DATA-CN = RID 管理員 $。  
4.  請選取屬性**rIDAvailablePool**，按一下 [**編輯**，然後將大型整數複製到剪貼簿。
![編輯 ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5.  小算盤，[開始] 和**檢視**功能表上，選取**工程型] 模式**。  6.  新增 100000 目前的值。  
![編輯 ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7.  使用 ctrl c 或**複製**命令的**編輯**功能表中，將值複製到剪貼簿。  
8.  Adsiedit 的 [編輯] 對話方塊，以貼到這個新的值。 
![編輯 ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. 按一下**[確定]**對話方塊中和**套用**更新屬性表中**rIDAvailablePool**屬性。  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>提高可使用 LDP RID 集區的值  
  
1.  在命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：  
  
     **ldp**  
  
2.  按一下**連接**，按一下 [**連接**、 輸入 RID 管理員名稱，然後再按**[確定]**。  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3.  按一下**連接**，按一下 [**繫結**、 選取**繫結的認證**並輸入您的系統管理認證，然後按一下 [ **[確定]**。  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4.  按一下**檢視**，按一下 [**樹**，然後輸入下列分辨的名稱路徑： DATA-CN = RID 管理員 $DATA-CN = 系統特區 =*的網域名稱*  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5.  按一下**瀏覽]**，然後按**修改**。  
6.  新增 100000 目前**rIDAvailablePool**值，而且然後輸入到總和**的值**。  
7.  在**Dn**，輸入`cn=RID Manager$,cn=System,dc=` *< 網域 name\ >*。  
8.  在**編輯項目屬性**，輸入`rIDAvailablePool`。  
9. 選取 [**取代**操作，然後再按一下為**輸入**。 </br>
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. 按一下**執行**若要執行的作業。  按一下**關閉**。
11. 若要驗證變更，按一下 [**檢視**，按一下 [**樹**，然後輸入下列分辨的名稱路徑： DATA-CN = RID 管理員 $DATA-CN = 系統特區 =*網域名稱*。    查看**rIDAvailablePool**屬性。  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
 

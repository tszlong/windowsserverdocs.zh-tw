---
title: AD 樹系復原引發 RID 集區
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adds
ms.openlocfilehash: c8f91226e10ea6681933d5a5dc00b92f5ab2179c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862979"
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>AD 樹系復原-引發的可用 RID 集區值 

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

使用下列程序，計算值的相關識別碼 (RID) 集區會配置 RID 操作主機之後該 DC 會還原。 藉由引發可用的 RID 集區的值，您可以確保任何 DC 會配置 RID，所建立的備份來還原網域之後為安全性主體。 

## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>關於 Active Directory RID 集區和 rIDAvailablePool

每個網域都有一個物件**CN = RID 管理員 $，CN = System，DC**=<*domain_name*>。 此物件具有屬性的具名**rIDAvailablePool**。 這個屬性值會維護整個網域的全域 RID 空間。 值為上限和下限的組件的大整數。 上半部會定義可配置給每個網域 （0x3FFFFFFF 或剛好超過 1 億個） 的安全性主體的數目。 下半部是網域中已配置的 Rid 的編號。 
  
> [!NOTE]
> 在 Windows Server 2016 和 2012年中，可配置的安全性主體的數目會增加至剛好超過 2 億個。 如需詳細資訊，請參閱 <<c0> [ 管理 RID 發行](https://technet.microsoft.com/library/jj574229.aspx)。 
  
- 範例值：4611686014132422708  
- 低位部份：2100 （下一步 的 RID 集區配置的開頭）  
- 上半部：1073741823 （可以在網域中建立的 Rid 的總數）  
  
當您增加的最大的整數值時，您會增加低部分的值。 比方說，如果您新增 100,000 4611686014132422708 4611686014132522708 加總的範例值時，新的低部分是 102100。 這表示下一步 而配置的 RID 主機的 RID 集區的開頭而不是 2100年 102100。 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator"></a>計算使用 adsiedit 和計算機的可用 RID 集區的值

1. 開啟 伺服器管理員中，按一下**工具**然後按一下**Adsi**。
2. 按一下滑鼠右鍵，選取**連接到**，並連接執行預設命名內容，按一下 **確定**。
   ![Adsi 編輯器](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. 瀏覽至下列的辨別的名稱路徑：**CN = RID 管理員 $，CN = System，DC =<domain name>**。
   ![Adsi 編輯器](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3. 以滑鼠右鍵按一下，並選取的屬性，為 CN = RID 管理員 $。 
4. 選取的屬性**rIDAvailablePool**，按一下**編輯**，然後將大整數值複製到剪貼簿。
   ![Adsi 編輯器](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5. 啟動計算機，以及從**檢視**功能表上，選取**工程型 模式**。 
6. 100,000 加入目前的值。
   ![Adsi 編輯器](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7. 使用 ctrl + c，或**複製**命令**編輯** 功能表中，將值複製到剪貼簿。 
8. 在 adsiedit 的 編輯 對話方塊中，貼上這個新值。 
   ![Adsi 編輯器](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. 按一下  **確定**在對話方塊中，並**套用**中的屬性工作表，來更新**rIDAvailablePool**屬性。 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>若要引發的可用線上使用 LDP 的 RID 集區值  
  
1. 在命令提示字元中輸入下列命令，然後按 ENTER：  
   **ldp**  
2. 按一下 **連接**，按一下**Connect**輸入 RID 管理員的名稱，然後按一下 **確定**。 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3. 按一下 **連接**，按一下**繫結**，選取**利用認證繫結**並輸入您的系統管理認證，然後按一下**確定**。 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4. 按一下 **檢視**，按一下**樹狀目錄**然後輸入下列的辨別的名稱路徑：CN = RID 管理員 $，CN = System，DC =*網域名稱*  
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5. 按一下 **瀏覽**，然後按一下**修改**。 
6. 加入至目前的 100,000 **rIDAvailablePool**值，然後輸入到總和**值**。 
7. 在  **Dn**，型別`cn=RID Manager$,cn=System,dc=` *< 網域名稱\>*。 
8. 在 **編輯項目屬性**，輸入`rIDAvailablePool`。 
9. 選取 **取代**當做作業，然後按一下**Enter**。
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. 按一下 **執行**執行該作業。 按一下 **關閉**。
11. 若要驗證變更，請按一下**檢視**，按一下**樹狀目錄**，然後輸入下列的辨別的名稱路徑： CN = RID 管理員 $，CN = System，DC =*網域名稱*。   請檢查**rIDAvailablePool**屬性。 
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)

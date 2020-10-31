---
title: AD 樹系復原-引發 RID 集區
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.openlocfilehash: 618bef60e4f432cda70daeeeeabd8da7c858950a
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067820"
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>AD 樹系復原-提高可用 RID 集區的值

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用下列程式，在還原 DC 之後，將 RID 操作主機將配置的相對識別碼 (RID) 集區的值提高。 藉由提高可用 RID 集區的值，您可以確保沒有 DC 針對在用來還原網域的備份之後所建立的安全性主體配置 RID。

## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>關於 Active Directory RID 集區和 rIDAvailablePool

每個網域都有物件 **CN = RID 管理員 $，CN = System，DC** =< *domain_name* >。 此物件具有名為 **rIDAvailablePool** 的屬性。 這個屬性值會維護整個網域的全域 RID 空間。 此值是具有上層和較低部分的大型整數。 上半部定義可以針對每個網域 (0x3FFFFFFF 或超過 1000000000) 配置的安全性主體數目。 下半部是在網域中配置的 Rid 數目。

> [!NOTE]
> 在 Windows Server 2016 和2012中，可配置的安全性主體數量會增加至超過2000000000。 如需詳細資訊，請參閱 [管理 RID 發行](./managing-rid-issuance.md)。

- 範例值：4611686014132422708
- 低部分： 2100 (要配置的下一個 RID 集區的開頭) 
- 上半部： 1073741823 (可在網域中建立的 Rid 總數) 

當您增加大整數的值時，會增加低部分的值。 例如，如果您將100000新增至4611686014132422708的範例值（總和為4611686014132522708），則新的低部分是102100。 這表示 RID 主機將配置的下一個 RID 集區，將以102100為開頭，而不是2100。

### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator"></a>使用 adsiedit 和計算機來提高可用 RID 集區的值

1. 開啟伺服器管理員、按一下 [ **工具** ]，然後按一下 [ **ADSI 編輯器** ]。
2. 以滑鼠右鍵按一下，選取 **[連接到** ]，然後按一下 [連接到預設命名內容]，再按一下 **[確定]** 。
   ![ADSI 編輯器](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png)
3. 流覽至下列辨別名稱路徑： **CN = RID 管理員 $，cn = System，DC = <domain name>** 。
   ![ADSI 編輯器](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png)
3. 以滑鼠右鍵按一下並選取 [CN = RID 管理員 $] 的屬性。
4. 選取 [屬性] **rIDAvailablePool** ，按一下 [ **編輯** ]，然後將大型整數值複製到剪貼簿。
   ![ADSI 編輯器](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)
5. 啟動計算機，然後從 [ **View** ] 功能表選取 [ **科學模式]** 。
6. 將100000新增至目前的值。
   ![ADSI 編輯器](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png)
7. 使用 ctrl-c 或 [ **編輯** ] 功能表中的 [ **複製** ] 命令，將值複製到剪貼簿。
8. 在 adsiedit 的 [編輯] 對話方塊中，貼上這個新值。
   ![ADSI 編輯器](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png)
9. 在對話方塊中按一下 **[確定** ] **，並在** 屬性工作表中套用以更新 **rIDAvailablePool** 屬性。

### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>使用 LDP 提高可用 RID 集區的值

1. 在命令提示字元中輸入下列命令，然後按 ENTER： **ldp**
2. 按一下 **[** 連線]，按一下 **[連線]** ，輸入 RID 管理員的名稱，然後按一下 **[確定]** 。
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3. 按一下 [連線] **，按一下** [系結 **]，選取** [ **使用認證** 系結] 並輸入系統管理認證，然後按一下 **[確定]** 。
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4. 按一下 [ **視圖** ]，再按一下 [ **樹狀結構** ]，然後輸入下列辨別名稱路徑： CN = RID Manager $，CN = System，DC = *domain name* 
    ![ LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5. 按一下 **[流覽]** ，然後按一下 [ **修改** ]。
6. 將100000新增至目前的 **rIDAvailablePool** 值，然後在 [ **值** ] 中輸入 sum。
7. 在 [ **Dn** ] 中，輸入 `cn=RID Manager$,cn=System,dc=` *\><功能變數名稱* 。
8. 在 [ **編輯專案] 屬性** 中，輸入 `rIDAvailablePool` 。
9. 選取 [ **取代** 為操作]，然後按一下 [ **輸入** ]。
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png)
10. 按一下 [ **執行** ] 以執行操作。 按一下 [關閉]。
11. 若要驗證變更，請按一下 [ **View** ]，再按一下 [ **樹狀目錄** ]，然後輸入下列辨別名稱路徑： cn = RID Manager $，CN = System，DC = *domain name* 。   檢查 **rIDAvailablePool** 屬性。
   ![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)

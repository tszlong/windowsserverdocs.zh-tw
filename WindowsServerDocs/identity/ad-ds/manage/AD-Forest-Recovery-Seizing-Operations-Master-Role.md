---
title: AD 樹系復原-佔用操作主機角色
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.openlocfilehash: dc9c435d45e15af627a259c73dcdd81a7689cbe6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943721"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>AD 樹系復原-佔用操作主機角色

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

使用下列程式來抓取操作主機角色 (也稱為彈性單一主機操作， (FSMO) 角色) 。 您可以使用 Ntdsutil.exe，這是自動安裝在所有 Dc 上的命令列工具。

## <a name="to-seize-an-operations-master-role"></a>若要轉移操作主機角色

1. 在命令提示字元中輸入下列命令，然後按 ENTER：

   ```
   ntdsutil
   ```

2. 在**ntdsutil：** 提示字元中，輸入下列命令，然後按 enter 鍵：

   ```
   roles
   ```

3. 在 [ **FSMO 維護：** ] 提示字元中，輸入下列命令，然後按 enter：

   ```
   connections
   ```

4. 在 [**伺服器連接：** ] 提示字元中，輸入下列命令，然後按 enter：

   ```
   Connect to server ServerFQDN
   ```

   其中， *ServerFQDN*是此 DC (FQDN) 的完整功能變數名稱，例如： **connect to server nycdc01.example.com**。

   如果*ServerFQDN*不成功，請使用 DC 的 NetBIOS 名稱。

5. 在 [**伺服器連接：** ] 提示字元中，輸入下列命令，然後按 enter：

   ```
   quit
   ```

6. 根據您要執行的角色而定，在 [ **FSMO 維護：** 提示] 中輸入適當的命令，如下表所述，然後按 enter。

|角色|認證|Command|
|----------|-----------------|-------------|
|網域命名主機|企業系統管理員|**佔用命名主機**|
|架構主機|Schema Admins|**佔用架構主機**|
|基礎結構主機**附注：** 在您取得結構主機角色之後，如果您需要執行 Adprep/Rodcprep.，稍後可能會收到錯誤 如需詳細資訊，請參閱知識庫文章[949257](https://support.microsoft.com/kb/949257)。|網域管理員|**佔用基礎結構主機**|
|PDC 模擬器主機|網域管理員|**佔用 pdc**|
|RID 主機|網域管理員|**佔用 rid 主機**|

確認要求之後，請 Active Directory 或 AD DS 嘗試轉移角色。 當傳輸失敗時，會出現一些錯誤資訊，並 Active Directory 或 AD DS 繼續進行強制執行。 在完成建立之後，會出現一份角色清單和輕量型目錄存取協定 (LDAP) 目前保存每個角色之伺服器的名稱。 您也可以在提升許可權的命令提示字元中執行**Netdom QUERY FSMO** ，以確認目前的角色持有者。

> [!NOTE]
> 如果這部電腦不是失敗之前的 RID 主機，而且您嘗試佔用 RID 主機角色，則電腦會在接受此角色之前，先嘗試與複寫協力電腦進行同步處理。 不過，因為當電腦已隔離時，會執行此步驟，所以與夥伴同步處理將會失敗。 因此，會出現一個對話方塊，詢問您是否要繼續操作，儘管這部電腦無法與合作夥伴進行同步處理。 按一下 [是] 。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)

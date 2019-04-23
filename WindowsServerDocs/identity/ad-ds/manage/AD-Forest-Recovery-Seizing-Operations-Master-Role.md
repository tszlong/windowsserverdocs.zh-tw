---
title: AD 樹系復原-拿取操作主機角色
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: 1994d49652ee9eb10f6afc73cf5b4630b4718e77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858459"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>AD 樹系復原-拿取操作主機角色  

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

您可以使用下列程序來拿取操作主機角色 （也稱為彈性單一主機操作 (FSMO) 角色）。 您可以使用 Ntdsutil.exe，會自動安裝所有的網域控制站的命令列工具。  
  
## <a name="to-seize-an-operations-master-role"></a>若要拿取操作主機角色  
  
1. 在命令提示字元中輸入下列命令，然後按 ENTER：  

   ```  
   ntdsutil  
   ```  

2. 在**ntdsutil:** 提示，輸入下列命令，然後按 ENTER 鍵：  

   ```  
   roles  
   ```  

3. 在**FSMO 維護：** 提示，輸入下列命令，然後按 ENTER 鍵：  

   ```  
   connections  
   ```  

4. 在**伺服器連線：** 提示，輸入下列命令，然後按 ENTER 鍵：  

   ```  
   Connect to server ServerFQDN  
   ```  

   何處*ServerFQDN*是完整的網域名稱 (FQDN)，此 dc，例如：**連線到伺服器 nycdc01.example.com**。  

   如果*ServerFQDN*沒有成功，使用 DC 的 NetBIOS 名稱。  

5. 在**伺服器連線：** 提示，輸入下列命令，然後按 ENTER 鍵：  

   ```  
   quit  
   ```  

6. 依據的角色，您想要拿取，在**FSMO 維護：** 提示，輸入適當的命令，在下表中所述，然後按 ENTER。  
  
|[角色]|認證|命令|  
|----------|-----------------|-------------|  
|網域命名主機|Enterprise Admins|**拿取命名主機**|  
|架構主機|Schema Admins|**拿取架構主機**|  
|基礎結構主機**附註：** 拿取基礎結構主機角色之後，您可能會收到錯誤稍後如果您需要執行 Adprep /Rodcprep。 如需詳細資訊，請參閱知識庫文章[949257](https://support.microsoft.com/kb/949257)。|Domain Admins|**拿取基礎結構主機**|  
|PDC 模擬器主機|Domain Admins|**拿取 pdc**|  
|RID 主機|Domain Admins|**拿取 rid 的主機**|  

確認要求後，Active Directory 或 AD DS 會嘗試將角色轉移。 當傳輸失敗時，會出現一些錯誤的資訊，和 Active Directory 或 AD DS 會繼續進行拿取。 拿取完成之後，便會出現的角色之伺服器的目前保留每個角色的輕量型目錄存取通訊協定 (LDAP) 名稱的清單。 您也可以執行**Netdom Query FSMO**以提高權限的命令提示字元，若要確認目前的角色持有者。  
  
> [!NOTE]
> 如果此電腦不是在失敗前的 RID 主機，而您嘗試拿取 RID 主機角色，電腦會嘗試接受此角色之前的複寫協力電腦同步處理。 不過，因為電腦是隔離時，會執行此步驟中，它將會失敗與夥伴進行同步處理。 因此，會出現對話方塊，詢問您是否要繼續進行此作業，儘管這台電腦無法與夥伴同步處理。 按一下 [ **是**]。  
  
## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)

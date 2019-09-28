---
title: 租使用者的受防護 Vm-使用 Windows Azure 套件部署受防護的 VM
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ec9f12990e7e16aebb208edfe0d97d6671623da1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403539"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>租使用者的受防護 Vm-使用 Windows Azure 套件部署受防護的 VM

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

如果您的主機服務提供者支援它，您可以使用 Windows Azure 套件來部署受防護的 VM。

完成下列步驟：

1. 訂閱 Windows Azure 套件中提供的一或多個方案。

2. 使用 Windows Azure 套件建立受防護的 VM。

    [使用受防護的虛擬機器](https://technet.microsoft.com/library/mt720674.aspx)，如下列主題所述：

   - [建立防護資料](https://technet.microsoft.com/library/mt720672.aspx)（並上傳防護資料檔案，如主題中的第二個程式所述）。
    
     > [!NOTE]
     > 在建立防護資料時，您將會下載您的守護者金鑰檔案，這會是 UTF-8 格式的 XML 檔案。 請勿將檔案變更為 UTF-16。
    
   - [建立受防護的虛擬機器](https://technet.microsoft.com/library/mt720673.aspx)-使用 [**快速建立**]、[透過受防護的範本] 或透過一般範本。
    
       > [!WARNING]
       > 如果您[使用一般範本建立受防護的虛擬機器](https://technet.microsoft.com/library/mt720673.aspx#Anchor_2)，請務必注意，VM 會以無*遮罩*方式布建。 這表示範本磁片不會針對防護資料檔案中的受信任磁片清單進行驗證，也不會在用來布建 VM 的防護資料檔案中提供秘密。 如果有受防護的範本可供使用，最好使用受防護的範本部署受防護的 VM，以提供密碼的端對端保護。
    
   - [將第2代虛擬機器轉換成受防護的虛擬機器](https://technet.microsoft.com/library/mt720670.aspx)
    
       > [!NOTE]
       > 如果您將虛擬機器轉換成受防護的虛擬機器，現有的檢查點和備份就不會加密。 您應該盡可能刪除舊的檢查點，以防止存取舊的解密資料。

## <a name="see-also"></a>另請參閱

- [適用于受防護主機和受防護 Vm 的主機服務提供者設定步驟](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)

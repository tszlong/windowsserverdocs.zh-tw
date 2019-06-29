---
title: 受防護的 Vm，可供租用戶-使用 Windows Azure Pack 部署受防護的 VM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4cb5ed3433e04b7a5fe7004e517eb4cc38c7eb53
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469655"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>受防護的 Vm，可供租用戶-使用 Windows Azure Pack 部署受防護的 VM

>適用於：Windows Server （半年通道），Windows Server 2019，Windows Server 2016

如果您的主機服務提供者支援它，您可以使用 Windows Azure Pack 部署受防護的 VM。

完成下列步驟：

1. 訂閱 Windows Azure Pack 中提供的一或多個方案。

2. 使用 Windows Azure Pack，以建立受防護的 VM。

    [使用受防護的虛擬機器](https://technet.microsoft.com/library/mt720674.aspx)，其中所述的下列主題：

   - [建立虛擬機器防護資料](https://technet.microsoft.com/library/mt720672.aspx)（和主題的第二個程序中所述上, 傳防護資料檔案）。
    
     > [!NOTE]
     > 在建立虛擬機器防護資料，您將下載守護者金鑰檔，將會以 utf-8 格式的 XML 檔案。 請勿變更此檔案為 utf-16。
    
   - [建立受防護的虛擬機器](https://technet.microsoft.com/library/mt720673.aspx)-透過**快速建立**、 透過受防護的範本，或透過一般的範本。
    
       > [!WARNING]
       > 如果您[使用一般的範本建立受防護的虛擬機器](https://technet.microsoft.com/library/mt720673.aspx#Anchor_2)，請務必請注意，VM 會佈建*受防護*。 這表示範本磁碟未驗證的受信任的磁碟，在虛擬機器防護資料檔案中，清單進行比對，也用來佈建 VM 的防護資料檔案中的祕密。 如果受防護的範本功能，最好是部署受防護的範本與受防護的 VM，以提供端對端保護您的祕密。
    
   - [將第 2 代虛擬機器轉換成受防護的虛擬機器](https://technet.microsoft.com/library/mt720670.aspx)
    
       > [!NOTE]
       > 如果您將虛擬機器轉換成受防護的虛擬機器時，現有的檢查點和備份不會加密。 您應該刪除舊的檢查點時可能無法存取您的舊、 解密資料。

## <a name="see-also"></a>另請參閱

- [裝載服務提供者設定步驟，針對受防護主機和受防護的 Vm](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [受防護網狀架構與受防護的 VM](guarded-fabric-and-shielded-vms-top-node.md)

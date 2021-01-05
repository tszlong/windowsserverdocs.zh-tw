---
title: 管理 Windows Server Essentials 中的備份與還原
description: 瞭解如何對您的網路電腦執行定期備份，並從 Windows Server Essentials 中的備份還原資料。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 89e67327786186a4fdd94a760d29afb47c5a2b66
ms.sourcegitcommit: e00e789dff216dbade861e61365f078b758a5720
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/23/2020
ms.locfileid: "97755284"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的備份與還原

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

 Windows Server Essentials 提供可靠的方法來定期備份您的伺服器和網路電腦。 如果發生資料遺失，您可以從伺服器上的成功備份還原資料，而不需要還原整部電腦。 必要時，您可以將整個系統還原到您的伺服器或網路中的用戶端電腦。 下表說明可供您使用的不同備份選項及其優點。

|備份功能|描述|優點|
|--------------------|-----------------|----------------|
|伺服器備份|會備份執行 Windows Server Essentials 的伺服器。 資料會備份到外部 USB 磁碟機。<br /><br /> 如需詳細資訊，請參閱 [管理伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md) 和 [還原或修復您的伺服器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md)。|-可以還原伺服器上的檔案和資料夾。<br /><br /> -可以執行伺服器的完整系統還原。|
|用戶端電腦備份|會備份網路中的用戶端電腦。 資料會備份在執行 Windows Server Essentials 的伺服器上。<br /><br /> 如需詳細資訊，請參閱 [管理用戶端備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) 和 [從現有的用戶端電腦備份還原完整的系統](Restore-a-full-system-from-an-existing-client-computer-backup.md)。|-可以從伺服器還原檔案和資料夾。<br /><br /> -可以執行用戶端電腦的完整系統還原。|
| Microsoft Azure 備份|會在伺服器上執行檔案或資料夾的線上備份。 當您使用 Azure 備份來備份伺服器資料時，資訊會在上傳至網際網路上的安全資料中心之前，使用您的複雜密碼進行加密。<br /><br /> 如需詳細資訊，請參閱 [管理線上備份](Manage-Online-Backup-in-Windows-Server-Essentials.md)。|-可以從伺服器還原檔案和資料夾。<br /><br /> -使用增量備份時，只會將對檔案所做的變更傳送到雲端。<br /><br /> -備份會儲存在 Microsoft Azure 並位於異地，以減少保護和保護現場備份媒體的需求。|

## <a name="additional-references"></a>其他參考資料

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)

-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

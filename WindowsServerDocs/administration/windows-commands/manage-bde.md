---
title: manage-bde
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ca23e5f4499672f1e4bfcca6b9ad27f4e84039b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373772"
---
# <a name="manage-bde"></a>manage-bde



用來開啟或關閉 BitLocker、指定解除鎖定機制、更新修復方法，以及解除鎖定受 BitLocker 保護的資料磁片磁碟機。 這個命令列工具可以用來取代**BitLocker 磁碟機加密**控制台專案。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[Manage-bde: status](manage-bde-status.md)|提供電腦上所有磁片磁碟機的相關資訊，不論它們是否受到 BitLocker 保護。|
|[Manage-bde: on](manage-bde-on.md)|加密磁片磁碟機並開啟 BitLocker。|
|[Manage-bde: off](manage-bde-off.md)|解密磁片磁碟機並關閉 BitLocker。 解密完成時，會移除所有金鑰保護裝置。|
|[Manage-bde: pause](manage-bde-pause.md)|暫停加密或解密。|
|[Manage-bde: resume](manage-bde-resume.md)|繼續加密或解密。|
|[Manage-bde: lock](manage-bde-lock.md)|防止存取受 BitLocker 保護的資料。|
|[Manage-bde: unlock](manage-bde-unlock.md)|允許使用修復密碼或修復金鑰來存取受 BitLocker 保護的資料。|
|[Manage-bde: autounlock](manage-bde-autounlock.md)|管理資料磁片磁碟機的自動解除鎖定。|
|[Manage-bde: protectors](manage-bde-protectors.md)|管理加密金鑰的保護方法。|
|[Manage-bde: tpm](manage-bde-tpm.md)|設定電腦的信賴平臺模組（TPM）。 執行 Windows 8 或**win8_server_2**的電腦不支援此命令。 若要管理這些電腦上的 TPM，請使用 TPM 管理 MMC 嵌入式管理單元或適用于 Windows PowerShell 的 TPM 管理 Cmdlet。|
|[Manage-bde: setidentifier](manage-bde-setidentifier.md)|將磁片磁碟機上的磁片磁碟機識別碼欄位設定為 [為**您的組織提供唯一識別碼**] 群組原則設定中指定的值。|
|[Manage-bde:ForceRecovery](manage-bde-forcerecovery.md)|重新開機時，強制將受 BitLocker 保護的磁片磁碟機進入修復模式。 此命令會從磁片磁碟機刪除所有 TPM 相關的金鑰保護裝置。 當電腦重新開機時，只會使用修復密碼或修復金鑰來解除鎖定磁片磁碟機。|
|[Manage-bde: changepassword](manage-bde-changepassword.md)|修改資料磁片磁碟機的密碼。|
|[Manage-bde: changepin](manage-bde-changepin.md)|修改作業系統磁片磁碟機的 PIN。|
|[Manage-bde: changekey](manage-bde-changekey.md)|修改作業系統磁片磁碟機的啟動金鑰。|
|[Manage-bde:KeyPackage](manage-bde-keypackage.md)|產生磁片磁碟機的金鑰封裝。|
|[Manage-bde: upgrade](manage-bde-upgrade.md)|升級 BitLocker 版本。|
|[Manage-bde:WipeFreeSpace](manage-bde-wipefreespace.md)|抹除磁片磁碟機上的可用空間。|
|-? 或/？|在命令提示字元中顯示簡短說明。|
|-help 或-h|在命令提示字元中顯示完整的說明。|

## <a name="BKMK_Examples"></a>典型

下列範例會顯示電腦上的磁片磁碟機，並識別它們是否受 BitLocker 保護，以及目前的加密狀態。
```
manage-bde -status
```
下列範例說明如何使用修復密碼的選項，在 C 磁片磁碟機上啟用 BitLocker。 修復密碼將由 BitLocker 產生，並顯示在畫面上，讓您可以記錄。
```
manage-bde –on C: -recoverypassword
```
下列範例說明如何使用修復密碼來解除鎖定受 BitLocker 保護的磁片磁碟機。
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [使用命令列啟用 BitLocker](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)

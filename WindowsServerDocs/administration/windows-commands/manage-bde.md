---
title: manage-bde
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8923177b03f378f8252c532ec386f1808e516e1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874499"
---
# <a name="manage-bde"></a>manage-bde



指定用來開啟或關閉 BitLocker，解除鎖定機制，更新復原方法，並解除鎖定受 BitLocker 保護的資料磁碟機。 這個命令列工具可用的位置**BitLocker 磁碟機加密**控制台項目。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm] 
[–SetIdentifier] [-ForceRecovery] [–changepassword] [–changepin] [–changekey] [-KeyPackage] [–upgrade] [-WipeFreeSpace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[管理 bde： 狀態](manage-bde-status.md)|不論它們受 BitLocker 保護，請在電腦上，提供所有磁碟機的相關資訊。|
|[管理 bde： 上](manage-bde-on.md)|加密磁碟機，並開啟 BitLocker。|
|[管理 bde： 關閉](manage-bde-off.md)|解密該磁碟機，並關閉 BitLocker。 解密完成時，會移除所有金鑰保護裝置。|
|[管理 bde： 暫停](manage-bde-pause.md)|暫停加密或解密。|
|[管理 bde： 繼續](manage-bde-resume.md)|會繼續加密或解密。|
|[管理 bde： 鎖定](manage-bde-lock.md)|禁止存取受 BitLocker 保護的資料。|
|[管理 bde： 解除鎖定](manage-bde-unlock.md)|可讓受 BitLocker 保護的資料，使用修復密碼或復原金鑰的存取權。|
|[管理 bde： 自動鎖定](manage-bde-autounlock.md)|管理自動解除鎖定資料磁碟機。|
|[管理 bde： 保護裝置](manage-bde-protectors.md)|管理加密金鑰的保護方法。|
|[管理 bde: tpm](manage-bde-tpm.md)|設定電腦的受信任的平台模組 (TPM)。 此命令不支援在執行 Windows 8 的電腦上或**win8_server_2**。 若要管理這些電腦上的 TPM，請使用 TPM 管理 MMC 嵌入式管理單元或 TPM 管理 cmdlet 適用於 Windows PowerShell。|
|[管理 bde: setidentifier](manage-bde-setidentifier.md)|設定中指定的值在磁碟機上的磁碟機識別碼欄位**為您的組織提供的唯一識別碼**群組原則設定。|
|[管理 power shell:ForceRecovery](manage-bde-forcerecovery.md)|強制受 BitLocker 保護磁碟機進入修復模式在重新啟動。 此命令會刪除所有的 TPM 相關金鑰保護裝置，從磁碟機。 當電腦重新啟動時，只能復原密碼或修復金鑰可以用來解除鎖定磁碟機。|
|[管理 bde: changepassword](manage-bde-changepassword.md)|修改資料磁碟機的密碼。|
|[管理 bde: changepin](manage-bde-changepin.md)|修改作業系統磁碟機的 pin 碼。|
|[管理 bde: changekey](manage-bde-changekey.md)|修改作業系統磁碟機的啟動金鑰。|
|[管理 power shell:KeyPackage](manage-bde-keypackage.md)|會產生磁碟機的金鑰封裝。|
|[管理 bde： 升級](manage-bde-upgrade.md)|升級 BitLocker 版本。|
|[管理 power shell:WipeFreeSpace](manage-bde-wipefreespace.md)|清除磁碟機上的可用空間。|
|-? 或 /？|顯示在命令提示字元中，簡短說明。|
|-help 或-h|顯示在命令提示字元完成說明。|

## <a name="BKMK_Examples"></a>範例

下列範例會顯示電腦上的磁碟機，並識別它們是受 BitLocker 保護的是否和目前的加密狀態。
```
manage-bde -status
```
下列範例說明如何啟用 BitLocker 的修復密碼選項的 C 磁碟機上。 將產生的 BitLocker 修復密碼，並將其顯示在畫面上，以便您可以記錄它。
```
manage-bde –on C: -recoverypassword
```
下列範例說明使用修復密碼解除鎖定受 BitLocker 保護的磁碟機。
```
manage-bde –unlock E: -recoverypassword 111111-222222-333333-444444-555555-666666-777777-888888
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [使用命令列啟用 BitLocker](https://technet.microsoft.com/library/dd894351(v=ws.10).aspx)

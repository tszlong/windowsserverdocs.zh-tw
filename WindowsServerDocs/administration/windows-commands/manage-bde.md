---
title: manage-bde
description: Manage-bde 命令的參考文章，可開啟或關閉 BitLocker、指定解除鎖定機制、更新修復方法，以及解除鎖定受 BitLocker 保護的資料磁片磁碟機。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 276a7841-7289-48d4-a57d-bc7c300affbb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7e248f507ca6d38248bc931cb3d1b98aa385c88
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956990"
---
# <a name="manage-bde"></a>manage-bde

開啟或關閉 BitLocker、指定解除鎖定機制、更新修復方法，以及解除鎖定受 BitLocker 保護的資料磁片磁碟機。

> [!NOTE]
> 這個命令列工具可以用來取代**BitLocker 磁碟機加密**控制台專案。

## <a name="syntax"></a>語法

```
manage-bde [-status] [–on] [–off] [–pause] [–resume] [–lock] [–unlock] [–autounlock] [–protectors] [–tpm]
[–setidentifier] [-forcerecovery] [–changepassword] [–changepin] [–changekey] [-keypackage] [–upgrade] [-wipefreespace] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| [manage-bde 狀態](manage-bde-status.md) | 提供電腦上所有磁片磁碟機的相關資訊，不論它們是否受到 BitLocker 保護。 |
| [管理-manage-bde on](manage-bde-on.md) | 加密磁片磁碟機並開啟 BitLocker。 |
| [manage-bde 關閉](manage-bde-off.md) | 解密磁片磁碟機並關閉 BitLocker。 解密完成時，會移除所有金鑰保護裝置。 |
| [manage-bde 暫停](manage-bde-pause.md) | 暫停加密或解密。 |
| [manage-bde resume](manage-bde-resume.md) | 繼續加密或解密。 |
| [manage-bde 鎖定](manage-bde-lock.md) | 防止存取受 BitLocker 保護的資料。 |
| [manage-bde 解除鎖定](manage-bde-unlock.md) | 允許使用修復密碼或修復金鑰來存取受 BitLocker 保護的資料。 |
| [manage-bde 再次](manage-bde-autounlock.md) | 管理資料磁片磁碟機的自動解除鎖定。 |
| [manage-bde 保護裝置](manage-bde-protectors.md) | 管理加密金鑰的保護方法。 |
| [manage-bde tpm](manage-bde-tpm.md) | 設定電腦的信賴平臺模組（TPM）。 執行 Windows 8 或**win8_server_2**的電腦不支援此命令。 若要管理這些電腦上的 TPM，請使用 TPM 管理 MMC 嵌入式管理單元或適用于 Windows PowerShell 的 TPM 管理 Cmdlet。 |
| [manage-bde setidentifier](manage-bde-setidentifier.md)   | 將磁片磁碟機上的磁片磁碟機識別碼欄位設定為 [為**您的組織提供唯一識別碼**] 群組原則設定中指定的值。 |
| [manage-bde ForceRecovery](manage-bde-forcerecovery.md) | 重新開機時，強制將受 BitLocker 保護的磁片磁碟機進入修復模式。 此命令會從磁片磁碟機刪除所有 TPM 相關的金鑰保護裝置。 當電腦重新開機時，只會使用修復密碼或修復金鑰來解除鎖定磁片磁碟機。 |
| [manage-bde changepassword](manage-bde-changepassword.md) | 修改資料磁片磁碟機的密碼。 |
| [manage-bde changepin](manage-bde-changepin.md) | 修改作業系統磁片磁碟機的 PIN。 |
| [manage-bde changekey](manage-bde-changekey.md) | 修改作業系統磁片磁碟機的啟動金鑰。 |
| [manage-bde KeyPackage](manage-bde-keypackage.md) | 產生磁片磁碟機的金鑰封裝。 |
| [manage-bde 升級](manage-bde-upgrade.md) | 升級 BitLocker 版本。 |
| [manage-bde WipeFreeSpace](manage-bde-wipefreespace.md) | 抹除磁片磁碟機上的可用空間。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整的說明。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [使用命令列啟用 BitLocker](/previous-versions/windows/it-pro/windows-7/dd894351(v=ws.10))

---
title: manage-bde 解除鎖定
description: Manage-bde unlock 命令的參考主題，它會使用修復密碼或修復金鑰來解除鎖定受 BitLocker 保護的磁片磁碟機。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67d4c0ec78870af45f0b98f2ab04d85b19e92af9
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222159"
---
# <a name="manage-bde-unlock"></a>manage-bde 解除鎖定

使用修復密碼或修復金鑰來解除鎖定受 BitLocker 保護的磁片磁碟機。

## <a name="syntax"></a>語法

```
manage-bde -unlock {-recoverypassword <password>|-recoverykey <pathtoexternalkeyfile>} <drive> [-certificate {-cf pathtocertificatefile | -ct certificatethumbprint} {-pin}] [-password] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -msfve-recoverypassword | 指定將使用修復密碼來解除鎖定磁片磁碟機。 您也可以使用 **-rp**做為此命令的縮寫版本。 |
| `<password>` | 代表可以用來解除鎖定磁片磁碟機的修復密碼。 |
| -recoverykey | 指定將使用外部修復金鑰檔案來解除鎖定磁片磁碟機。 您也可以使用 **-rk**做為此命令的縮寫版本。 |
| `<pathtoexternalkeyfile>` | 代表可以用來解除鎖定磁片磁碟機的外部修復金鑰檔案。 |
| `<drive>` | 表示後面接著冒號的磁碟機號。 |
| -憑證 | 用來解除鎖定磁片區的 BitLocker 憑證的本機使用者憑證位於 [本機使用者] 憑證存放區中。 您也可以使用 **-cert**做為此命令的縮寫版本。 |
| -cf`<pathtocertificatefile>` | 憑證檔案的路徑。 |
| -ct`<certificatethumbprint>` | 憑證指紋，可以選擇性地包含 PIN 碼（-pin）。 |
| -password | 提供密碼解除鎖定磁片區的提示。 您也可以使用 **-pw**做為此命令的縮寫版本。 |
| -computername | 指定 manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整的說明。 |

### <a name="examples"></a>範例

若要使用已儲存至另一個磁片磁碟機上備份檔案夾的修復金鑰檔案來解除鎖定磁片磁碟機 E，請輸入：

```
manage-bde –unlock E: -recoverykey F:\Backupkeys\recoverykey.bek
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)

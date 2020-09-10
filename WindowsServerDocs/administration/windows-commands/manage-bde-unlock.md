---
title: manage-bde 解除鎖定
description: Manage-bde unlock 命令的參考文章，此命令會使用修復密碼或修復金鑰解除鎖定受 BitLocker 保護的磁片磁碟機。
ms.topic: reference
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 23b7a593b450265b2547acd6fea0bbe8241e993b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635239"
---
# <a name="manage-bde-unlock"></a>manage-bde 解除鎖定

使用修復密碼或修復金鑰解除鎖定受 BitLocker 保護的磁片磁碟機。

## <a name="syntax"></a>語法

```
manage-bde -unlock {-recoverypassword <password>|-recoverykey <pathtoexternalkeyfile>} <drive> [-certificate {-cf pathtocertificatefile | -ct certificatethumbprint} {-pin}] [-password] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -ms-fve-recoverypassword | 指定將使用修復密碼來解除鎖定磁片磁碟機。 您也可以使用 **-rp** 作為此命令的縮寫版本。 |
| `<password>` | 表示可以用來解除鎖定磁片磁碟機的修復密碼。 |
| -recoverykey | 指定將使用外部修復金鑰檔案來解除鎖定磁片磁碟機。 您也可以使用 **-rk** 作為此命令的縮寫版本。 |
| `<pathtoexternalkeyfile>` | 代表可以用來解除鎖定磁片磁碟機的外部修復金鑰檔案。 |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -憑證 | BitLocker 憑證用來解除鎖定磁片區的本機使用者憑證位於本機使用者憑證存放區中。 您也可以使用 **-cert** 作為此命令的縮寫版本。 |
| -cf `<pathtocertificatefile>` | 憑證檔案的路徑。 |
| -ct `<certificatethumbprint>` | 憑證指紋，可選擇性地包含 PIN ( pin) 。 |
| -password | 顯示用來解除鎖定磁片區的密碼提示。 您也可以使用 **-pw** 作為此命令的縮寫版本。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要使用已儲存至另一個磁片磁碟機備份檔案夾的修復金鑰檔來解除鎖定 E 磁片磁碟機，請輸入：

```
manage-bde –unlock E: -recoverykey F:\Backupkeys\recoverykey.bek
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)

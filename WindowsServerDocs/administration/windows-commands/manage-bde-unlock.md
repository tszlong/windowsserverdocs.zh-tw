---
title: manage-bde 解除鎖定
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e26952ca8c2b20cb0cb8efa167fca81e27b692a0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839735"
---
# <a name="manage-bde-unlock"></a>manage-bde：解除鎖定



使用修復密碼或修復金鑰來解除鎖定受 BitLocker 保護的磁片磁碟機。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>參數

|參數|值|描述|
|---------|-----|-----------|
|-msfve-recoverypassword||指定將使用修復密碼來解除鎖定磁片磁碟機。縮寫：-rp|
||\<密碼 >|代表可以用來解除鎖定磁片磁碟機的修復密碼。|
|-recoverykey||指定將使用外部修復金鑰檔案來解除鎖定磁片磁碟機。 縮寫：-rk|
||\<PathToExternalKeyFile >|代表可以用來解除鎖定磁片磁碟機的外部修復金鑰檔案。|
||\<磁片磁碟機 >|表示後面接著冒號的磁碟機號。|
|-憑證||要 unclock 磁片區的 BitLocker 憑證的本機使用者憑證位於位置使用者憑證存放區中。 縮寫：-cert|
||<-cf PathToCertificateFile >|Cerficate 檔案的路徑|
||<-ct CertificateThumbprint >|憑證指紋，可以選擇性地包含 PIN 碼（-pin）。|
|-password||提供密碼解除鎖定磁片區的提示。 縮寫：-pw|
|-computername||指定 Manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 縮寫：-cn|
||\<名稱 >|代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或/？||在命令提示字元中顯示簡短說明。|
|-help 或-h||在命令提示字元中顯示完整的說明。|

## <a name="examples"></a><a name=BKMK_Examples></a>典型

下列範例說明如何使用 **-unlock**命令，將已儲存至另一個磁片磁碟機上備份檔案夾的修復金鑰檔案解除鎖定磁片磁碟機 E。
```
manage-bde –unlock E: -recoverykey F:\Backupkeys\recoverykey.bek
```

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)
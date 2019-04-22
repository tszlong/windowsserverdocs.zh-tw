---
title: 管理 bde 解除鎖定
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7852bf7d-9102-40be-adcb-71e8f4dfde72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a86267890449be2048221940e5955e49f30f99f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814599"
---
# <a name="manage-bde-unlock"></a>管理 bde： 解除鎖定



使用修復密碼或修復金鑰來解除鎖定受 BitLocker 保護的磁碟機。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde -unlock {-recoverypassword <Password>|-recoverykey <PathToExternalKeyFile>} <Drive> [-certificate {-cf PathToCertificateFile | -ct CertificateThumbprint} {-pin}] [-password] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

|參數|值|描述|
|---------|-----|-----------|
|-recoverypassword||指定的修復密碼，將會用來解除鎖定磁碟機。縮寫： rp|
||\<密碼 >|代表可以用來解除鎖定磁碟機的修復密碼。|
|-recoverykey||指定外部的修復金鑰檔案，可用來解除鎖定磁碟機。 Abbreviation: -rk|
||\<PathToExternalKeyFile>|代表外部的修復金鑰檔案，可用來解除鎖定磁碟機。|
||\<Drive>|表示磁碟機代號，後面接著冒號。|
|-certificate||BitLocker 將憑證加入 unclock 磁碟區的本機使用者憑證位於位置的使用者憑證存放區。 Abbreviation: -cert|
||<-cf PathToCertificateFile>|才能在檔案路徑|
||<-ct CertificateThumbprint>|這可能會選擇性地包含 PIN 的憑證指紋 (-pin)。|
|-password||提供密碼才能解除鎖定磁碟區的提示。 縮寫： 密碼|
|-computername||指定 bde.exe 用以修改不同的電腦上的 BitLocker 保護。 縮寫： cn|
||\<名稱 >|表示要修改 BitLocker 保護之電腦的名稱。 可接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或 /？||顯示在命令提示字元中，簡短說明。|
|-help 或-h||顯示在命令提示字元完成說明。|

## <a name="BKMK_Examples"></a>範例

下列範例說明如何利用 **-解除鎖定**命令來解除鎖定磁碟機 E 與復原的金鑰檔已在另一個磁碟機上儲存備份的資料夾。
```
manage-bde –unlock E: -recoverykey "F:\Backupkeys\recoverykey.bek"
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)
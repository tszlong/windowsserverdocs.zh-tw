---
title: repair-bde
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e2bce0c0a0f12a3a171c161a669c903044e8b4b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813539"
---
# <a name="repair-bde"></a>repair-bde



如果使用 BitLocker 來加密磁碟機，存取會加密在嚴重損毀的硬碟上的資料。 Power shell 可以重新建構的磁碟機的重要部分，並搶救可復原的資料，只要以有效的復原密碼或修復金鑰用來解密資料。 如果磁碟機上的 BitLocker 中繼資料已損毀，您必須能夠提供除了修復密碼或修復金鑰的備份金鑰封裝。 此金鑰封裝備份到 Active Directory 網域服務 (AD DS) 如果您使用預設的 AD DS 的備份。 與此金鑰封裝並修復密碼或修復金鑰，您可以解密受 BitLocker 保護的磁碟機的部分，如果磁碟已損毀。 每個金鑰封裝只適用於具有對應的磁碟機識別元的磁碟機。 您可以使用[Active Directory 的 BitLocker 修復密碼檢視器](https://technet.microsoft.com/library/dd875531(v=ws.10).aspx)從 AD DS 中取得此金鑰封裝。

> [!NOTE]
> BitLocker 修復密碼檢視器就會包含做為其中一個可使用 Windows Server 2012 中的 管理伺服器安裝的選用的管理功能。

Power shell 命令列工具有下列限制：
-   Power shell 無法修復加密或解密處理期間失敗的磁碟機。
-   Power shell 假設，如果磁碟機有任何加密，然後將磁碟機已完整加密。

如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<InputVolume>|識別您想要修復的 BitLocker 加密磁碟機的磁碟機代號。 磁碟機代號必須包含冒號;例如：**C:**。|
|\<OutputVolumeorImage>|識別要將內容儲存到修復磁碟機的磁碟機。 將覆寫輸出磁碟機上的所有資訊。|
|-rk|識別應該用來解除鎖定磁碟區的修復金鑰的位置。 此命令可能也會指定為 **-recoverykey**。|
|-rp|識別應該用來解除鎖定磁碟區的數字修復密碼。 此命令可能也會指定為 **-recoverypassword**。|
|-pw|識別應該用來解除鎖定磁碟區的密碼。 此命令可能也會指定為 **-密碼**|
|-kp|識別可用來解除鎖定磁碟區修復金鑰封裝。 此命令可能也會指定為 **-keypackage**。|
|-lf|指定 repair-bde 錯誤、 警告和資訊訊息會儲存檔案的路徑。 此命令可能也會指定為 **-記錄檔**。|
|-f|強制即使無法鎖定要卸載磁碟區。 此命令可能也會指定為 **-強制**。|
|-? 或 /？|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

如果未指定金鑰封裝的路徑，**修復 bde**會搜尋金鑰封裝的磁碟機。 不過，在硬碟機已損毀，如果**修復 bde**可能無法找到的套件，並會提示您提供的路徑。

## <a name="BKMK_Examples"></a>範例

下列範例會嘗試修復磁碟機 C，並將內容寫入 C 磁碟機中至 D 磁碟機使用修復金鑰檔案 (RecoveryKey.bek) 儲存在磁碟機 F 上，並將這項嘗試的結果寫入至記錄檔上的檔案 (log.txt) 磁碟機 Z。
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
下列範例會嘗試修復磁碟機 C，並將內容寫入磁碟機 C 上至 D 磁碟機使用指定的 48 位數的修復密碼。 有六位數的八個區塊應該輸入修復密碼與連字號分隔每個區塊。
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
下列範例會強制要卸載的 C 磁碟機，並接著會嘗試將修復磁碟機 C，並將內容寫入磁碟機 C 上至 D 磁碟機使用修復金鑰封裝並修復金鑰檔案 (RecoveryKey.bek) 儲存在磁碟機 f。
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
下列範例會嘗試修復磁碟機 C 和 C 磁碟機中的將內容寫入至 D 磁碟機，您必須輸入密碼以解除鎖定磁碟機 c： 出現提示時：
```
repair-bde C: D: -pw
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
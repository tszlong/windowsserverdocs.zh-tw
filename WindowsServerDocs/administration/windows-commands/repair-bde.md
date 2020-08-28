---
title: repair-bde
description: Repair 命令的參考文章，如果磁片磁碟機是使用 BitLocker 進行加密，則可以嘗試重建嚴重損毀磁片磁碟機的重要部分，並搶救可復原的資料。
ms.topic: reference
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5154a88778dbc3877e3075c813dae06937c1322
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038355"
---
# <a name="repair-bde"></a>repair-bde

如果磁片磁碟機已使用 BitLocker 加密，且具有有效的修復密碼或修復金鑰可進行解密，則會嘗試重建嚴重損毀磁片磁碟機的重要部分，並搶救可復原的資料。

> [!IMPORTANT]
> 如果磁片磁碟機上的 BitLocker 中繼資料資料已損毀，除了修復密碼或修復金鑰之外，您還必須能夠提供備份金鑰套件。 如果您使用 Active Directory Domain Services 的預設金鑰備份設定，則會在此備份您的金鑰套件。 您可以使用 [bitlocker：使用 Bitlocker 修復密碼檢視器](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-use-bitlocker-recovery-password-viewer) 從 AD DS 取得金鑰套件。
>
> 使用金鑰封裝和修復密碼或修復金鑰，您可以解密受 BitLocker 保護之磁片磁碟機的部分，即使磁片已損毀。 每個金鑰套件只適用于具有對應磁片磁碟機識別碼的磁片磁碟機。

## <a name="syntax"></a>語法

```
repair-bde <inputvolume> <outputvolumeorimage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<inputvolume>` | 識別您想要修復的 BitLocker 加密磁片磁碟機的磁碟機號。 磁碟機號必須包含冒號;例如： **C：**。 如果未指定金鑰封裝的路徑，此命令會在磁片磁碟機中搜尋金鑰套件。 當硬碟損毀時，此命令可能找不到套件，並會提示您提供路徑。 |
| `<outputvolumeorimage>` | 識別要儲存修復之磁片磁碟機內容的磁片磁碟機。 輸出磁片磁碟機上的所有資訊都會遭到覆寫。 |
| -rk | 識別應該用來解除鎖定磁片區的修復金鑰位置。 此命令也可以指定為 **-recoverykey**。 |
| -rp | 識別用來解除鎖定磁片區的數值修復密碼。 此命令也可以指定為 **-ms-fve-recoverypassword**。 |
| -pw | 識別用來解除鎖定磁片區的密碼。 此命令也可以指定為 **-password** |
| -kp | 識別可以用來解除鎖定磁片區的修復金鑰套件。 此命令也可以指定為 **-keypackage**。 |
| -lf | 指定將儲存 Repair 錯誤、警告和資訊訊息之檔案的路徑。 此命令也可指定為 **-logfile**。 |
| -f | 強制卸載磁片區，即使該磁片區無法鎖定也一樣。 此命令也可以指定為 **-force**。 |
| -? 或/？ | 在命令提示字元顯示 [說明]。 |

### <a name="limitations"></a>限制

此命令有下列限制：

- 此命令無法修復加密或解密過程中失敗的磁片磁碟機。

- 此命令假設如果磁片磁碟機具有任何加密，則磁片磁碟機已完全加密。

## <a name="examples"></a>範例

若要嘗試修復磁片磁碟機 C：，以將磁片磁碟機 C：的內容寫入磁片磁碟機 D：使用修復金鑰檔 (RecoveryKey. bek) 儲存在磁片磁碟機 F：，並將嘗試的結果寫入記錄檔 ( # A0) 的磁片磁碟機 Z：，請輸入：

```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```

若要嘗試修復磁片磁碟機 C：並將內容從磁片磁碟機 C：寫入磁片磁碟機 D：若要使用指定的48位數的修復密碼，請輸入：

```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```

>[!NOTE]
> 修復密碼的類型應為八個數字，並以連字號分隔每個區塊。

若要強制卸載磁片磁碟機 C：，請嘗試修復磁片磁碟機 C：，然後從磁片磁碟機 C：將內容寫入磁片磁碟機 D：使用修復金鑰封裝和修復金鑰檔 (RecoveryKey bek) 儲存在磁片磁碟機 F：上，輸入：

```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```

若要嘗試修復磁片磁碟機 C：並將內容從磁片磁碟機 C：寫入磁片磁碟機 D：，您必須輸入密碼才能解除鎖定磁片磁碟機 C： (當系統提示) 時，請輸入：

```
repair-bde C: D: -pw
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

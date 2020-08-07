---
title: repair-bde
description: '* * * * 的參考文章'
ms.topic: article
ms.assetid: 534dca1a-05f7-4ea8-ac24-4fe5f14f988a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1ba55b5a1689ecfc6ebe8fb6ab3d02b717e7d38
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883765"
---
# <a name="repair-bde"></a>repair-bde



如果使用 BitLocker 加密磁片磁碟機，則會在嚴重損壞的硬碟上存取加密的資料。 只要有效的修復密碼或復原金鑰用來解密資料，Repair 就可以重建磁片磁碟機的重要部分，並回收可復原的資料。 如果磁片磁碟機上的 BitLocker 中繼資料已損毀，除了修復密碼或修復金鑰以外，您還必須能夠提供備份金鑰封裝。 如果您使用 AD DS 備份的預設設定，則會在 Active Directory Domain Services (AD DS) 中備份此金鑰套件。 使用此金鑰套件和修復密碼或修復金鑰，您可以在磁片損毀時，解密受 BitLocker 保護之磁片磁碟機的某些部分。 每個金鑰封裝僅適用于具有對應磁片磁碟機識別碼的磁片磁碟機。 您可以使用[BitLocker 修復密碼檢視器來進行 Active Directory](/previous-versions/windows/it-pro/windows-7/dd875531(v=ws.10)) ，以從 AD DS 取得此金鑰封裝。

> [!NOTE]
> BitLocker 修復密碼檢視器包含在 Windows Server 2012 上，可使用伺服器管理安裝的選用管理功能之一。

Repair 命令列工具有下列限制：
-   Repair 無法修復加密或解密過程中失敗的磁片磁碟機。
-   Repair 假設如果磁片磁碟機具有任何加密，則磁片磁碟機已完全加密。



## <a name="syntax"></a>語法

```
repair-bde <InputVolume> <OutputVolumeorImage> [-rk] [–rp] [-pw] [–kp] [–lf] [-f] [{-?|/?}]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<InputVolume>|識別您想要修復的 BitLocker 加密磁片磁碟機的磁碟機號。 磁碟機號必須包含冒號;例如： **C：**。|
|\<OutputVolumeorImage>|識別用來儲存修復的磁片磁碟機內容的磁片磁碟機。 輸出磁片磁碟機上的所有資訊都會遭到覆寫。|
|-rk|識別應用來解除鎖定磁片區的修復金鑰位置。 此命令也可以指定為 **-recoverykey**。|
|-rp|識別用來解除鎖定磁片區的數值修復密碼。 此命令也可以指定為 **-msfve-recoverypassword**。|
|-pw|識別用來解除鎖定磁片區的密碼。 此命令也可以指定為 **-password**|
|-kp|識別可用於解除鎖定磁片區的修復金鑰封裝。 此命令也可以指定為 **-keypackage**。|
|-lf|指定檔案的路徑，該檔案會儲存 Repair-manage-bde 錯誤、警告和資訊訊息。 此命令也可以指定為 **-logfile**。|
|-f|強制卸載磁片區，即使它無法鎖定也一樣。 此命令也可以指定為 **-force**。|
|-? 或/？|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

如果未指定金鑰封裝的路徑，則**repair**會在磁片磁碟機中搜尋金鑰套件。 不過，如果硬碟已損毀，則**repair**可能找不到套件，並會提示您提供路徑。

## <a name="examples"></a>範例

若要嘗試修復 C 磁片磁碟機，並使用修復金鑰檔案將內容從 C 磁片磁碟機寫入磁片磁碟機 D， (RecoveryKey bek) 儲存在磁片磁碟機 F 上，並將此嘗試的結果寫入至磁片磁碟機 Z 上 ( # A0) 的記錄檔。
```
repair-bde C: D: -rk F:\RecoveryKey.bek –lf Z:\log.txt
```
若要嘗試修復 C 磁片磁碟機，並使用指定的48位數的修復密碼，將磁片磁碟機 C 的內容寫入磁片磁碟機 D。 修復密碼應輸入六個數字的八個區塊中，並以連字號分隔每個區塊。
```
repair-bde C: D: -rp 111111-222222-333333-444444-555555-666666-777777-888888
```
若要強制卸載磁片磁碟機 C，然後嘗試修復 C 磁片磁碟機，並將磁片磁碟機 c 上的內容寫入磁片磁碟機 D，並使用修復金鑰封裝和復原金鑰檔案 (RecoveryKey bek) 儲存在磁片磁碟機 F 上。
```
repair-bde C: D: -kp F:\RecoveryKeyPackage -rk F:\RecoveryKey.bek -f
```
若要嘗試修復 C 磁片磁碟機，並將磁片磁碟機 c 的內容寫入磁片磁碟機 D，當出現提示時，您必須輸入密碼來解除鎖定磁片磁碟機 C：
```
repair-bde C: D: -pw
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

---
title: 管理-manage-bde on
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1785c10ade5f7aace8595d8d0972fb1fa315232b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840011"
---
# <a name="manage-bde-on"></a>manage-bde： on



加密磁片磁碟機並開啟 BitLocker。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde –on <Drive> {[-recoveryPassword <NumericalPassword>]|[-recoverykey <PathToExternalDirectory>]|[-startupkey <PathToExternalKeyDirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <PathToExternalKeyDirectory>]|[-tpmandstartupkey <PathToExternalKeyDirectory>]|[-password]|[-ADAccountOrGroup <Domain\Account>]}
[-UsedSpaceOnly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <FileSystemType>] [-ForceEncryptionType <type>] [-RemoveVolumeShadowCopies][-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<磁片磁碟機 >|表示後面接著冒號的磁碟機號。|
|-msfve-recoverypassword|新增數位密碼保護裝置。 您也可以使用 **-rp**做為此命令的縮寫版本。|
|\<NumericalPassword >|表示修復密碼。|
|-recoverykey|新增用於復原的外部金鑰保護裝置。 您也可以使用 **-rk**做為此命令的縮寫版本。|
|\<PathToExternalDirectory >|表示修復金鑰的目錄路徑。|
|-startupkey|新增用於啟動的外部金鑰保護裝置。 您也可以使用 **-sk**作為此命令的縮寫版本。|
|\<PathToExternalKeyDirectory >|表示啟動金鑰的目錄路徑。|
|-憑證|新增資料磁片磁碟機的公用金鑰保護裝置。 您也可以使用 **-cert**做為此命令的縮寫版本。|
|-tpmanDPIn|新增適用于作業系統磁片磁碟機的信賴平臺模組（TPM）和個人識別碼（PIN）保護裝置。 您也可以使用 **-tp**做為此命令的縮寫版本。|
|-tpmandstartupkey|新增作業系統磁片磁碟機的 TPM 和啟動金鑰保護裝置。 您也可以使用 **-嘖**做為此命令的縮寫版本。|
|-tpmanDPInandstartupkey|新增作業系統磁片磁碟機的 TPM、PIN 和啟動金鑰保護裝置。 您也可以使用 **-tpsk**做為此命令的縮寫版本。|
|-password|新增資料磁片磁碟機的密碼金鑰保護裝置。 您也可以使用 **-pw**做為此命令的縮寫版本。|
|-ADAccountOrGroup|新增磁片區以 SID 為基礎的身分識別保護裝置。 如果使用者或電腦具有適當的認證，磁片區將會自動解除鎖定。 指定電腦帳戶時，請將 **$** 附加至電腦名稱稱，並指定 **– service** ，以指出解除鎖定應出現在 BitLocker 伺服器的內容中，而不是使用者。 您也可以使用 **-sid**作為此命令的縮寫版本。|
|-UsedSpaceOnly|將加密模式設定為 [僅加密已使用的空間]。 包含已使用空間的磁片區區段將會加密，但可用空間將不會。 如果未指定此選項，磁片區上所有已使用的空間和可用空間將會加密。 您也可以使用**來**做為此命令的縮寫版本。|
|-encryptionMethod|設定加密演算法和金鑰大小。 您也可以使用 **-em**作為此命令的縮寫版本。|
|-skiphardwaretest|在不進行硬體測試的情況下開始加密。 您也可以使用 **-s**作為此命令的縮寫版本。|
|-discoveryvolumetype|指定要用於探索資料磁片磁碟機的檔案系統。 探索資料磁片磁碟機是一種隱藏磁片磁碟機，已新增至 FAT 格式且受 BitLocker 保護的卸載式資料磁片磁碟機，其中包含 BitLocker To Go 讀取工具，因此可以使用 Windows Vista 或 Windows XP 作業系統來觀看受 BitLocker 保護的磁片磁碟機。|
|-ForceEncryptionType|強制 BitLocker 使用軟體或硬體加密。 您可以指定**硬體**或**軟體**做為加密類型。 如果已選取**硬體**參數，但磁片磁碟機不支援硬體加密，則 manage-bde 會傳回錯誤。 如果群組原則設定禁止指定的加密類型，manage-bde 會傳回錯誤。 您也可以使用 **-fet**做為此命令的縮寫版本。|
|-RemoveVolumeShadowCopies|強制 deletikon 磁片區陰影複製的磁片區。 執行此命令之後，您將無法使用先前的系統還原點來還原此磁片區。 您也可以使用 **-rvsc**做為此命令的縮寫版本。|
|\<FileSystemType >|指定可與探索資料磁片磁碟機搭配使用的檔案系統： FAT32、預設值或無。|
|-computername|指定使用 manage-bde 來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。|
|\<名稱 >|代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或/？|在命令提示字元中顯示簡短說明。|
|-help 或-h|在命令提示字元中顯示完整的說明。|

## <a name="examples"></a><a name=BKMK_Examples></a>典型

下列範例說明如何使用 **-on**命令來開啟磁片磁碟機 C 的 BitLocker，並將修復密碼新增至磁片磁碟機。
```
manage-bde –on C: -recoverypassword
```
下列範例說明如何使用 **-on**命令來開啟磁片磁碟機 C 的 BitLocker、將修復密碼新增至磁片磁碟機，以及將修復金鑰儲存至磁片磁碟機 E。
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
下列範例說明如何使用 **-on**命令，使用外部金鑰保護裝置（例如 USB 金鑰）來開啟磁片磁碟機的 BitLocker，以將作業系統磁片磁碟機解除鎖定。 如果您將 BitLocker 與沒有 TPM 的電腦搭配使用，則需要此方法。
```
manage-bde -on C: -startupkey E:\
```
下列範例說明如何使用 **-on**命令來開啟資料磁片磁碟機 E 的 BitLocker，並新增密碼金鑰保護裝置。 在輸入此命令之後，manage-bde 會提示您輸入密碼。
```
manage-bde –on E: -pw
```
下列範例說明如何使用 **-on**命令來開啟作業系統磁片磁碟機 C 的 BitLocker，並使用以硬體為基礎的加密。
```
manage-bde –on C: -fet Hardware
```

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)
---
title: 管理 bde 上
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b50cad64025e85824a8f0a27d773ffb614491fe5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841179"
---
# <a name="manage-bde-on"></a>管理 bde： 上



加密磁碟機，並開啟 BitLocker。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
manage-bde –on <Drive> {[-recoveryPassword <NumericalPassword>]|[-recoverykey <PathToExternalDirectory>]|[-startupkey <PathToExternalKeyDirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <PathToExternalKeyDirectory>]|[-tpmandstartupkey <PathToExternalKeyDirectory>]|[-password]|[-ADAccountOrGroup <Domain\Account>]}
[-UsedSpaceOnly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <FileSystemType>] [-ForceEncryptionType <type>] [-RemoveVolumeShadowCopies][-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Drive>|表示磁碟機代號，後面接著冒號。|
|-recoverypassword|將數字密碼保護裝置。 您也可以使用 **-rp**為此命令縮寫版。|
|\<NumericalPassword>|表示將修復密碼。|
|-recoverykey|新增復原外部金鑰保護裝置。 您也可以使用 **-rk**為此命令縮寫版。|
|\<PathToExternalDirectory>|表示修復金鑰的目錄路徑。|
|-startupkey|將 啟動外部金鑰保護裝置。 您也可以使用 **-sk**為此命令縮寫版。|
|\<PathToExternalKeyDirectory>|表示啟動金鑰的目錄路徑。|
|-certificate|新增資料磁碟機的公用金鑰保護裝置。 您也可以使用 **-cert**為此命令縮寫版。|
|-tpmandpin|新增受信任的平台模組 (TPM) 和個人識別碼數字 (PIN) 保護裝置的作業系統磁碟機。 您也可以使用 **-tp**為此命令縮寫版。|
|-tpmandstartupkey|新增作業系統磁碟機的 TPM 和啟動金鑰保護裝置。 您也可以使用 **-嘖嘖**為此命令縮寫版。|
|-tpmandpinandstartupkey|新增 TPM、 PIN 和作業系統磁碟機的啟動金鑰保護裝置。 您也可以使用 **-tpsk**為此命令縮寫版。|
|-password|新增資料磁碟機的密碼金鑰保護裝置。 您也可以使用 **-pw**為此命令縮寫版。|
|-ADAccountOrGroup|新增磁碟區的 SID 為基礎的身分識別保護裝置。 如果使用者或電腦具有適當的認證，將會自動解除鎖定磁碟區。 指定電腦帳戶時，附加**$** 電腦名稱，並指定 **– 服務**表示解除鎖定應該在 BitLocker 伺服器，而不是內容使用者。 您也可以使用 **-sid**為此命令縮寫版。|
|-UsedSpaceOnly|將加密模式設定為只使用的空間加密。 包含已使用的空間的磁碟區的區段將會加密，但可用的空間將不會。 如果未指定此選項，所有已使用空間，並將加密磁碟區上的可用空間... 您也可以使用 **-使用**為此命令縮寫版。|
|-encryptionMethod|設定加密演算法和金鑰大小。 您也可以使用 **-m**為此命令縮寫版。|
|-skiphardwaretest|開始加密而不需硬體的測試。 您也可以使用 **-s**為此命令縮寫版。|
|-discoveryvolumetype|指定要用於探索的資料磁碟機的檔案系統。 探索資料磁碟機是隱藏的磁碟機新增至包含 BitLocker To Go 讀取工具，讓 Windows Vista 或 Windows XP 作業系統可以用來檢視受 BitLocker 保護的磁碟機格式化為 FAT 的、 受 BitLocker 保護的卸除式資料磁碟機。|
|-ForceEncryptionType|強制使用軟體或硬體加密的 BitLocker。 您可以指定**硬體**或是**軟體**做為加密類型。 如果**硬體**選取參數，但磁碟機不支援硬體加密，管理 bde 會傳回錯誤。 如果群組原則設定會禁止指定的加密類型，管理 bde 就會傳回錯誤。 您也可以使用 **-fet**為此命令縮寫版。|
|-RemoveVolumeShadowCopies|強制的磁碟區陰影複製磁碟區的 deletikon。 您無法還原使用先前的系統還原點執行此命令之後此磁碟區。 您也可以使用 **-rvsc**為此命令縮寫版。|
|\<FileSystemType>|指定哪些檔案系統可與探索資料磁碟機：FAT32、 default 或 none。|
|-computername|指定 manage-bde 要用來修改不同的電腦上的 BitLocker 保護。 您也可以使用 **-cn**為此命令縮寫版。|
|\<名稱 >|表示要修改 BitLocker 保護之電腦的名稱。 可接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或 /？|顯示在命令提示字元中，簡短說明。|
|-help 或-h|顯示在命令提示字元完成說明。|

## <a name="BKMK_Examples"></a>範例

下列範例說明如何利用 **-在**命令來開啟 BitLocker 磁碟機 C，並新增到磁碟機的修復密碼。
```
manage-bde –on C: -recoverypassword
```
下列範例說明如何利用 **-在**命令來開啟 BitLocker 磁碟機 C，加上磁碟機的修復密碼，並將修復金鑰儲存到 e 磁碟機。
```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```
下列範例說明如何利用 **-在**命令，以開啟 BitLocker 磁碟機 C 來解除鎖定作業系統磁碟機中使用外部金鑰保護裝置 （例如 usb 隨身碟）。 如果您使用未配備 TPM 的電腦中的 BitLocker 需要這個方法。
```
manage-bde -on C: -startupkey E:\
```
下列範例說明如何利用 **-在**命令來開啟 BitLocker 資料磁碟機 E，並新增密碼金鑰保護裝置。 管理 power shell 會提示您在輸入此命令後輸入的密碼。
```
manage-bde –on E: -pw
```
下列範例說明如何利用 **-在**命令來開啟 BitLocker 作業系統磁碟機 C，並使用硬體式加密。
```
manage-bde –on C: -fet Hardware
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)
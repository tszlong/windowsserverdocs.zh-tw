---
title: 管理-manage-bde on
description: 適用于 manage-bde on 命令的參考文章，它會加密磁片磁碟機並開啟 BitLocker。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b06c8a37524544201bf9f37a446a8d227f878ee4
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935453"
---
# <a name="manage-bde-on"></a>管理-manage-bde on

加密磁片磁碟機並開啟 BitLocker。

## <a name="syntax"></a>語法

```
manage-bde –on <drive> {[-recoverypassword <numericalpassword>]|[-recoverykey <pathtoexternaldirectory>]|[-startupkey <pathtoexternalkeydirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <pathtoexternalkeydirectory>]|[-tpmandstartupkey <pathtoexternalkeydirectory>]|[-password]|[-ADaccountorgroup <domain\account>]}
[-usedspaceonly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <filesystemtype>] [-forceencryptiontype <type>] [-removevolumeshadowcopies][-computername <name>]
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<drive>` | 表示後面接著冒號的磁碟機號。 |
| -msfve-recoverypassword | 新增數位密碼保護裝置。 您也可以使用 **-rp**做為此命令的縮寫版本。 |
| `<numericalpassword>` | 表示修復密碼。 |
| -recoverykey | 新增用於復原的外部金鑰保護裝置。 您也可以使用 **-rk**做為此命令的縮寫版本。 |
| `<pathtoexternaldirectory>` | 表示修復金鑰的目錄路徑。 |
| -startupkey | 新增用於啟動的外部金鑰保護裝置。 您也可以使用 **-sk**作為此命令的縮寫版本。 |
| `<pathtoexternalkeydirectory>` | 表示啟動金鑰的目錄路徑。 |
| -憑證 | 新增資料磁片磁碟機的公用金鑰保護裝置。 您也可以使用 **-cert**做為此命令的縮寫版本。 |
| -tpmanDPIn | 新增適用于作業系統磁片磁碟機的信賴平臺模組（TPM）和個人識別碼（PIN）保護裝置。 您也可以使用 **-tp**做為此命令的縮寫版本。 |
| -tpmandstartupkey | 新增作業系統磁片磁碟機的 TPM 和啟動金鑰保護裝置。 您也可以使用 **-嘖**做為此命令的縮寫版本。 |
| -tpmanDPInandstartupkey | 新增作業系統磁片磁碟機的 TPM、PIN 和啟動金鑰保護裝置。 您也可以使用 **-tpsk**做為此命令的縮寫版本。 |
| -password | 新增資料磁片磁碟機的密碼金鑰保護裝置。 您也可以使用 **-pw**做為此命令的縮寫版本。 |
| -ADaccountorgroup | 新增磁片區以 SID 為基礎的身分識別保護裝置。 如果使用者或電腦具有適當的認證，磁片區將會自動解除鎖定。 指定電腦帳戶時，請將附加 **$** 至電腦名稱稱，並指定 **– service** ，以指出解除鎖定應出現在 BitLocker 伺服器的內容中，而不是使用者。 您也可以使用 **-sid**作為此命令的縮寫版本。 |
| -usedspaceonly | 將加密模式設定為 [僅加密已使用的空間]。 包含已使用空間的磁片區區段將會加密，但可用空間將不會。 如果未指定此選項，磁片區上所有已使用的空間和可用空間將會加密。 您也可以使用**來**做為此命令的縮寫版本。 |
| -encryptionMethod | 設定加密演算法和金鑰大小。 您也可以使用 **-em**作為此命令的縮寫版本。 |
| -skiphardwaretest | 在不進行硬體測試的情況下開始加密。 您也可以使用 **-s**作為此命令的縮寫版本。 |
| -discoveryvolumetype | 指定要用於探索資料磁片磁碟機的檔案系統。 探索資料磁片磁碟機是一種隱藏的磁片磁碟機，已新增至 FAT 格式、受 BitLocker 保護的卸載式資料磁片磁碟機，其中包含 BitLocker To Go 讀取工具。 |
| -forceencryptiontype | 強制 BitLocker 使用軟體或硬體加密。 您可以指定**硬體**或**軟體**做為加密類型。 如果已選取**硬體**參數，但磁片磁碟機不支援硬體加密，則 manage-bde 會傳回錯誤。 如果群組原則設定禁止指定的加密類型，manage-bde 會傳回錯誤。 您也可以使用 **-fet**做為此命令的縮寫版本。 |
| -removevolumeshadowcopies | 強制刪除磁片區的磁片區陰影複製。 執行此命令之後，您將無法使用先前的系統還原點來還原此磁片區。 您也可以使用 **-rvsc**做為此命令的縮寫版本。 |
| `<filesystemtype>` | 指定可與探索資料磁片磁碟機搭配使用的檔案系統： FAT32、預設值或無。 |
| -computername | 指定使用 manage-bde 來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整的說明。 |

### <a name="examples"></a>範例

若要開啟磁片磁碟機 C 的 BitLocker，並將修復密碼新增至磁片磁碟機，請輸入：

```
manage-bde –on C: -recoverypassword
```

若要開啟 C 磁片磁碟機的 BitLocker，請將修復密碼新增至磁片磁碟機，並將修復金鑰儲存到磁片磁碟機 E，輸入：

```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```

若要開啟 C 磁片磁碟機的 BitLocker，使用外部金鑰保護裝置（例如 USB 金鑰）解除鎖定作業系統磁片磁碟機，請輸入：

```
manage-bde -on C: -startupkey E:\
```

> [!IMPORTANT]
> 如果您將 BitLocker 與沒有 TPM 的電腦搭配使用，則需要此方法。

若要開啟資料磁片磁碟機 E 的 BitLocker，並新增密碼金鑰保護裝置，請輸入：

```
manage-bde –on E: -pw
```

若要開啟作業系統磁片磁碟機 C 的 BitLocker，以及使用硬體型加密，請輸入：

```
manage-bde –on C: -fet hardware
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde off 命令](manage-bde-off.md)

- [manage-bde pause 命令](manage-bde-pause.md)

- [manage-bde resume 命令](manage-bde-resume.md)

- [manage-bde 命令](manage-bde.md)

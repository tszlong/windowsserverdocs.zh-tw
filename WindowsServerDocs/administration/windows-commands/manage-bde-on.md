---
title: 管理-manage-bde 開啟
description: Manage-bde on 命令的參考文章，此命令會加密磁片磁碟機並開啟 BitLocker。
ms.topic: reference
ms.assetid: f6a12814-df74-416c-a04a-62ea8512263e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2774c44542889210a06420949813bdb67564d70a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634286"
---
# <a name="manage-bde-on"></a>管理-manage-bde 開啟

加密磁片磁碟機並開啟 BitLocker。

## <a name="syntax"></a>語法

```
manage-bde –on <drive> {[-recoverypassword <numericalpassword>]|[-recoverykey <pathtoexternaldirectory>]|[-startupkey <pathtoexternalkeydirectory>]|[-certificate]|
[-tpmandpin]|[-tpmandpinandstartupkey <pathtoexternalkeydirectory>]|[-tpmandstartupkey <pathtoexternalkeydirectory>]|[-password]|[-ADaccountorgroup <domain\account>]}
[-usedspaceonly][-encryptionmethod {aes128_diffuser|aes256_diffuser|aes128|aes256}] [-skiphardwaretest] [-discoveryvolumetype <filesystemtype>] [-forceencryptiontype <type>] [-removevolumeshadowcopies][-computername <name>]
[{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -ms-fve-recoverypassword | 新增數位密碼保護裝置。 您也可以使用 **-rp** 作為此命令的縮寫版本。 |
| `<numericalpassword>` | 表示修復密碼。 |
| -recoverykey | 新增用於復原的外部金鑰保護裝置。 您也可以使用 **-rk** 作為此命令的縮寫版本。 |
| `<pathtoexternaldirectory>` | 代表修復金鑰的目錄路徑。 |
| -startupkey | 新增用於啟動的外部金鑰保護裝置。 您也可以使用 **-sk** 作為此命令的縮寫版本。 |
| `<pathtoexternalkeydirectory>` | 表示啟動金鑰的目錄路徑。 |
| -憑證 | 新增資料磁片磁碟機的公開金鑰保護裝置。 您也可以使用 **-cert** 作為此命令的縮寫版本。 |
| -tpmanDPIn | 為作業系統磁片磁碟機新增可信賴平臺模組 (TPM) 和個人識別碼 (PIN) 保護裝置。 您也可以使用 **-tp** 作為此命令的縮寫版本。 |
| -tpmandstartupkey | 為作業系統磁片磁碟機新增 TPM 和啟動金鑰保護裝置。 您也可以使用 **-嘖嘖** 作為此命令的縮寫版本。 |
| -tpmanDPInandstartupkey | 為作業系統磁片磁碟機新增 TPM、PIN 和啟動金鑰保護裝置。 您也可以使用 **-tpsk** 作為此命令的縮寫版本。 |
| -password | 新增資料磁片磁碟機的密碼金鑰保護裝置。 您也可以使用 **-pw** 作為此命令的縮寫版本。 |
| -ADaccountorgroup | 為磁片區新增以 SID 為基礎的身分識別保護裝置。 如果使用者或電腦具有適當的認證，磁片區將會自動解除鎖定。 指定電腦帳戶時，請將新增 **$** 至電腦名稱稱並指定 **– service** ，表示解除鎖定應該在 BitLocker 伺服器的內容中進行，而不是使用者。 您也可以使用 **-sid** 做為此命令的縮寫版本。 |
| -usedspaceonly | 將加密模式設定為 [僅限使用的空間加密]。 包含已使用空間的磁片區區段將會加密，但可用空間將不會加密。 如果未指定此選項，磁片區上的所有已使用空間和可用空間都會加密。 您也 **可以使用此命令的縮寫** 版本。 |
| -encryptionMethod | 設定加密演算法和金鑰大小。 您也可以使用 **-em** 作為此命令的縮寫版本。 |
| -skiphardwaretest | 開始加密而不進行硬體測試。 您也可以使用 **-s** 作為此命令的縮寫版本。 |
| -discoveryvolumetype | 指定要用於探索資料磁片磁碟機的檔案系統。 探索資料磁片磁碟機是已新增至 FAT 格式、受 BitLocker 保護的卸載式資料磁片磁碟機（其中包含 BitLocker To Go 讀取工具）的隱藏磁片磁碟機。 |
| -forceencryptiontype | 強制 BitLocker 使用軟體或硬體加密。 您可以指定 **硬體** 或 **軟體** 做為加密類型。 如果已選取 **硬體** 參數，但磁片磁碟機不支援硬體加密，則 manage-bde 會傳回錯誤。 如果群組原則設定禁止指定的加密類型，則 manage-bde 會傳回錯誤。 您也可以使用 **-fet** 作為此命令的縮寫版本。 |
| -removevolumeshadowcopies | 強制刪除磁片區的磁片區陰影複製。 執行此命令之後，您將無法使用先前的系統還原點還原此磁片區。 您也可以使用 **-rvsc** 作為此命令的縮寫版本。 |
| `<filesystemtype>` | 指定可搭配探索資料磁片磁碟機使用的檔案系統： FAT32、預設或無。 |
| -computername | 指定使用 manage-bde 來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要開啟磁片磁碟機 C 的 BitLocker，並將修復密碼新增至磁片磁碟機，請輸入：

```
manage-bde –on C: -recoverypassword
```

若要開啟磁片磁碟機 C 的 BitLocker，請將修復密碼新增至磁片磁碟機，並將修復金鑰儲存至 E 磁片磁碟機，請輸入：

```
manage-bde –on C: -recoverykey E:\ -recoverypassword
```

若要開啟磁片磁碟機 C 的 BitLocker，使用外部金鑰保護裝置 (例如 USB 金鑰) 解除鎖定作業系統磁片磁碟機，請輸入：

```
manage-bde -on C: -startupkey E:\
```

> [!IMPORTANT]
> 如果您在沒有 TPM 的電腦上使用 BitLocker，則需要這個方法。

若要開啟資料磁片磁碟機 E 的 BitLocker，並新增密碼金鑰保護裝置，請輸入：

```
manage-bde –on E: -pw
```

若要開啟作業系統磁片磁碟機 C 的 BitLocker，並使用以硬體為基礎的加密，請輸入：

```
manage-bde –on C: -fet hardware
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde off 命令](manage-bde-off.md)

- [manage-bde pause 命令](manage-bde-pause.md)

- [manage-bde resume 命令](manage-bde-resume.md)

- [manage-bde 命令](manage-bde.md)

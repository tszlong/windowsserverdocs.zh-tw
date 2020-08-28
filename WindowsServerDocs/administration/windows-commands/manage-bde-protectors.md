---
title: manage-bde 保護裝置
description: Manage-bde 保護裝置命令的參考文章，可管理用於 BitLocker 加密金鑰的保護方法。
ms.topic: reference
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: 0461edcb2e1177f1a72ec7e4a1c893c80cd70698
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027566"
---
# <a name="manage-bde-protectors"></a>manage-bde 保護裝置

> 適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

管理用於 BitLocker 加密金鑰的保護方法。

## <a name="syntax"></a>語法

```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ----------- | ----------- |
| -get | 顯示磁片磁碟機上啟用的所有金鑰保護方法，並將其類型和識別碼提供 (識別碼) 。 |
| -新增 | 使用額外 **的-add** 參數，新增指定的金鑰保護方法。 |
| -delete | 刪除 BitLocker 所使用的金鑰保護方法。 除非使用選擇性 **的-delete** 參數來指定要刪除的保護裝置，否則將會從磁片磁碟機移除所有金鑰保護裝置。 刪除磁片磁碟機上的最後一個保護裝置時，會停用磁片磁碟機的 BitLocker 保護，以確保不會不慎遺失資料的存取權。 |
| -disable | 停用保護，讓任何人都能在磁片磁碟機上提供不安全的加密金鑰來存取加密的資料。 未移除任何金鑰保護裝置。 除非使用選用 **-disable** 參數來指定重新開機計數，否則系統會在下一次啟動 Windows 時繼續保護。 |
| -enable | 從磁片磁碟機移除不安全的加密金鑰，以啟用保護。 將會強制執行磁片磁碟機上所有設定的金鑰保護裝置。 |
| -adbackup | 備份指定給 Active Directory Domain Services (AD DS) 之磁片磁碟機的所有修復資訊。 若只要將單一修復金鑰備份至 AD DS，請附加 **-id** 參數，並指定要備份的特定修復金鑰的識別碼。 |
| -aadbackup | 備份指定給 Azure Active Directory (Azure AD) 之磁片磁碟機的所有修復資訊。 若只要將單一修復金鑰備份至 Azure AD，請附加 **-id** 參數，並指定要備份的特定修復金鑰的識別碼。 |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

#### <a name="additional--add-parameters"></a>其他-新增參數

-Add 參數也可以使用這些有效的額外參數。

```
manage-bde -protectors -add [<drive>] [-forceupgrade] [-recoverypassword <numericalpassword>] [-recoverykey <pathtoexternalkeydirectory>]
[-startupkey <pathtoexternalkeydirectory>] [-certificate {-cf <pathtocertificatefile>|-ct <certificatethumbprint>}] [-tpm] [-tpmandpin]
[-tpmandstartupkey <pathtoexternalkeydirectory>] [-tpmandpinandstartupkey <pathtoexternalkeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <name>]
[{-?|/?}] [{-help|-h}]
```

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -ms-fve-recoverypassword | 新增數位密碼保護裝置。 您也可以使用 **-rp** 作為此命令的縮寫版本。 |
| `<numericalpassword>` | 表示修復密碼。 |
| -recoverykey | 新增用於復原的外部金鑰保護裝置。 您也可以使用 **-rk** 作為此命令的縮寫版本。 |
| `<pathtoexternalkeydirectory>` | 代表修復金鑰的目錄路徑。 |
| -startupkey | 新增用於啟動的外部金鑰保護裝置。 您也可以使用 **-sk** 作為此命令的縮寫版本。 |
| `<pathtoexternalkeydirectory>` | 表示啟動金鑰的目錄路徑。 |
| -憑證 | 新增資料磁片磁碟機的公開金鑰保護裝置。 您也可以使用 **-cert** 作為此命令的縮寫版本。 |
| -cf | 指定將用來提供公開金鑰憑證的憑證檔案。 |
| <pathtocertificatefile> | 代表憑證檔案的目錄路徑。 |
| -ct | 指定將使用憑證指紋來識別公開金鑰憑證 |
| `<certificatethumbprint>` | 指定您想要使用之憑證的指紋屬性值。 例如，憑證指紋值為 a9 09 50 2d d8 2a e4 14 33 e6 f8 38 86 b0 0d 42 77 a3 2a 7b 應指定為 a909502dd82ae41433e6f83886b00d4277a32a7b。 |
| -tpmanDPIn | 為作業系統磁片磁碟機新增可信賴平臺模組 (TPM) 和個人識別碼 (PIN) 保護裝置。 您也可以使用 **-tp** 作為此命令的縮寫版本。 |
| -tpmandstartupkey | 為作業系統磁片磁碟機新增 TPM 和啟動金鑰保護裝置。 您也可以使用 **-嘖嘖** 作為此命令的縮寫版本。 |
| -tpmanDPInandstartupkey | 為作業系統磁片磁碟機新增 TPM、PIN 和啟動金鑰保護裝置。 您也可以使用 **-tpsk** 作為此命令的縮寫版本。 |
| -password | 新增資料磁片磁碟機的密碼金鑰保護裝置。 您也可以使用 **-pw** 作為此命令的縮寫版本。 |
| -adaccountorgroup | 為磁片區新增 (SID) 型身分識別保護裝置的安全識別碼。 您也可以使用 **-sid** 做為此命令的縮寫版本。 **重要事項：** 根據預設，您無法使用 WMI 或 manage-bde 從遠端新增 ADaccountorgroup 保護裝置。 如果您的部署需要能夠從遠端新增此保護裝置，您必須啟用限制委派。 |
| -computername | 指定使用 manage-bde 來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

#### <a name="additional--delete-parameters"></a>其他-刪除參數

```
manage-bde -protectors -delete <drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}]
[-id <keyprotectorID>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -類型 | 識別要刪除的金鑰保護裝置。 您也可以使用 **-t** 作為此命令的縮寫版本。 |
| ms-fve-recoverypassword | 指定應刪除任何修復密碼金鑰保護裝置。 |
| externalkey | 指定應刪除任何與磁片磁碟機相關聯的外部金鑰保護裝置。 |
| 憑證 (certificate) | 指定應刪除與磁片磁碟機相關聯的任何憑證金鑰保護裝置。 |
| Tpm | 指定是否應該刪除與磁片磁碟機相關聯的任何僅 TPM 金鑰保護裝置。 |
| tpmandstartupkey | 指定應刪除任何與磁片磁碟機相關聯的 TPM 和啟動金鑰金鑰保護裝置。 |
| tpmanDPIn | 指定應刪除與磁片磁碟機相關聯的任何 TPM 和 PIN 型金鑰保護裝置。 |
| tpmanDPInandstartupkey | 指定是否應刪除任何與磁片磁碟機相關聯的 TPM、PIN 和啟動金鑰型金鑰保護裝置。 |
| 密碼 | 指定應刪除與磁片磁碟機相關聯的任何密碼金鑰保護裝置。 |
| 身分識別 | 指定應刪除與磁片磁碟機相關聯的任何身分識別金鑰保護裝置。 |
| -ID | 使用金鑰識別碼來識別要刪除的金鑰保護裝置。 這個參數是 **-type** 參數的替代選項。 |
| `<keyprotectorID>` | 識別要刪除的磁片磁碟機上的個別金鑰保護裝置。 您可以使用 **manage-bde-保護裝置（get** ）命令來顯示金鑰保護裝置識別碼。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

#### <a name="additional--disable-parameters"></a>其他-停用參數

```
manage-bde -protectors -disable <drive> [-rebootcount <integer 0 - 15>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| rebootcount | 指定作業系統磁片區的保護已暫止，並且將在重新開機 Windows 之後，于 **rebootcount** 參數中指定的次數之後繼續。 指定 **0** 會無限期地暫停保護。 如果未指定此參數，BitLocker 保護會在 Windows 重新開機後自動繼續。 您也可以使用 **-rc** 作為此命令的縮寫版本。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要將憑證金鑰保護裝置（由憑證檔案識別）新增至磁片磁碟機 E，請輸入：

```
manage-bde -protectors -add E: -certificate -cf c:\File Folder\Filename.cer
```

若要將 **adaccountorgroup** 金鑰保護裝置（以網域和使用者名稱識別）新增至磁片磁碟機 E，請輸入：

```
manage-bde -protectors -add E: -sid DOMAIN\user
```

若要在電腦重新開機3次之前停用保護，請輸入：

```
manage-bde -protectors -disable C: -rc 3
```

若要刪除磁片磁碟機 C 上所有以 TPM 和啟動的金鑰為基礎的金鑰保護裝置，請輸入：

```
manage-bde -protectors -delete C: -type tpmandstartupkey
```

若要將磁片磁碟機 C 的所有修復資訊備份到 AD DS，請輸入：

```
manage-bde -protectors -adbackup C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)

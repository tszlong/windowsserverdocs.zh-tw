---
title: manage-bde 保護裝置
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: e01049a5fb3dc419e219fe4ec8b11dcdc790f919
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724112"
---
# <a name="manage-bde-protectors"></a>manage-bde：保護裝置

> 適用於：Windows Server (半年度管道)、Windows Server 2016

管理用於 BitLocker 加密金鑰的保護方法。
## <a name="syntax"></a>語法
```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
#### <a name="parameters"></a>參數

|   參數   |                                                                                                                                                                                           描述                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     -get      |                                                                                                                                            顯示磁片磁碟機上已啟用的所有金鑰保護方法，並提供其類型和識別碼（ID）。                                                                                                                                             |
|     -新增      |                                                                                                                                   新增使用其他[新增參數](manage-bde-protectors.md#BKMK_addprotectors)所指定的金鑰保護方法。                                                                                                                                    |
|    -delete    | 刪除 BitLocker 所使用的金鑰保護方法。 除非使用選擇性[的刪除參數](manage-bde-protectors.md#BKMK_deleteprotectors)來指定要刪除的保護裝置，否則會從磁片磁碟機移除所有金鑰保護裝置。 刪除磁片磁碟機上的最後一個保護裝置時，會停用磁片磁碟機的 BitLocker 保護，以確保不會不慎遺失資料的存取權。 |
|   -disable    |                      停用保護，讓任何人都能在磁片磁碟機上提供不安全的加密金鑰來存取加密的資料。 不會移除任何金鑰保護裝置。 除非使用選擇性的[停用參數](manage-bde-protectors.md#BKMK_disableprot)來指定重新開機計數，否則會在下一次啟動 Windows 時繼續保護。                       |
|    -enable    |                                                                                                                             從磁片磁碟機移除不安全的加密金鑰，以啟用保護。 系統將會強制執行磁片磁碟機上所有已設定的金鑰保護裝置。                                                                                                                             |
|   -adbackup   |                                                                          備份指定給 Active Directory Domain Services （AD DS）之磁片磁碟機的所有修復資訊。 若只要備份單一修復金鑰以 AD DS，請附加 **-id**參數，並指定要備份的特定修復金鑰識別碼。                                                                           |
|  -aadbackup   |                                                                            備份指定給 Azure Active Directory （Azure Ad）之磁片磁碟機的所有修復資訊。 若只要備份單一修復金鑰以 Azure AD，請附加 **-id**參數，並指定要備份的特定修復金鑰識別碼。                                                                             |
|    <Drive>    |                                                                                                                                                                          表示後面接著冒號的磁碟機號。                                                                                                                                                                          |
| -computername |                                                                                                              指定 manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。                                                                                                              |
|    <Name>     |                                                                                                                 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。                                                                                                                  |
|   -? 或/？    |                                                                                                                                                                            在命令提示字元中顯示簡短說明。                                                                                                                                                                            |
|  -help 或-h  |                                                                                                                                                                          在命令提示字元中顯示完整的說明。                                                                                                                                                                           |

### <a name="-add-syntax-and-parameters"></a><a name=BKMK_addprotectors></a>-新增語法和參數
```
manage-bde  -protectors  -add [<Drive>] [-forceupgrade] [-recoverypassword <NumericalPassword>] [-recoverykey <pathToExternalKeydirectory>]
[-startupkey <pathToExternalKeydirectory>] [-certificate {-cf <pathToCertificateFile>|-ct <CertificateThumbprint>}] [-tpm] [-tpmandpin] 
[-tpmandstartupkey <pathToExternalKeydirectory>] [-tpmandpinandstartupkey <pathToExternalKeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

|          參數           |                                                                                                                                                                                   描述                                                                                                                                                                                   |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           <Drive>            |                                                                                                                                                                 表示後面接著冒號的磁碟機號。                                                                                                                                                                  |
|      -msfve-recoverypassword       |                                                                                                                                    新增數位密碼保護裝置。 您也可以使用 **-rp**做為此命令的縮寫版本。                                                                                                                                     |
|     <NumericalPassword>      |                                                                                                                                                                        表示修復密碼。                                                                                                                                                                        |
|         -recoverykey         |                                                                                                                                新增用於復原的外部金鑰保護裝置。 您也可以使用 **-rk**做為此命令的縮寫版本。                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                               表示修復金鑰的目錄路徑。                                                                                                                                                                |
|         -startupkey          |                                                                                                                                 新增用於啟動的外部金鑰保護裝置。 您也可以使用 **-sk**作為此命令的縮寫版本。                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                                表示啟動金鑰的目錄路徑。                                                                                                                                                                |
|         -憑證         |                                                                                                                               新增資料磁片磁碟機的公用金鑰保護裝置。 您也可以使用 **-cert**做為此命令的縮寫版本。                                                                                                                               |
|             -cf              |                                                                                                                                              指定將用來提供公開金鑰憑證的憑證檔案。                                                                                                                                              |
|   <pathToCertificateFile>    |                                                                                                                                                             表示憑證檔案的目錄路徑。                                                                                                                                                              |
|             -ct              |                                                                                                                                           指定將使用憑證指紋來識別公開金鑰憑證                                                                                                                                           |
|   <CertificateThumbprint>    |                                                       指定您想要使用之憑證的 [指紋] 屬性值。 例如，憑證指紋值為 a9 09 50 2d d8 2a e4 14 33 e6 f8 38 86 b0 0d 42 77 a3 2a 7b 應指定為 a909502dd82ae41433e6f83886b00d4277a32a7b。                                                        |
|          -tpmanDPIn          |                                                                                           新增適用于作業系統磁片磁碟機的信賴平臺模組（TPM）和個人識別碼（PIN）保護裝置。 您也可以使用 **-tp**做為此命令的縮寫版本。                                                                                           |
|      -tpmandstartupkey       |                                                                                                                    新增作業系統磁片磁碟機的 TPM 和啟動金鑰保護裝置。 您也可以使用 **-嘖**做為此命令的縮寫版本。                                                                                                                    |
|   -tpmanDPInandstartupkey    |                                                                                                                新增作業系統磁片磁碟機的 TPM、PIN 和啟動金鑰保護裝置。 您也可以使用 **-tpsk**做為此命令的縮寫版本。                                                                                                                 |
|          -password           |                                                                                                                              新增資料磁片磁碟機的密碼金鑰保護裝置。 您也可以使用 **-pw**做為此命令的縮寫版本。                                                                                                                              |
|      -adaccountorgroup       | 新增磁片區的安全識別碼（SID）型身分識別保護裝置。  您也可以使用 **-sid**作為此命令的縮寫版本。 **重要事項：** 根據預設，您無法使用 WMI 或 manage-bde 從遠端新增 ADAccountOrGroup 保護裝置。  如果您的部署需要能夠從遠端新增此保護裝置，您必須啟用限制委派。 |
|        -computername         |                                                                                                       指定使用 manage-bde 來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。                                                                                                       |
|            <Name>            |                                                                                                         代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。                                                                                                         |

### <a name="-delete-syntax-and-parameters"></a><a name=BKMK_deleteprotectors></a>-delete 語法和參數
```
manage-bde  -protectors  -delete <Drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}] 
[-id <KeyProtectorID>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|       參數        |                                                                              描述                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        <Drive>         |                                                             表示後面接著冒號的磁碟機號。                                                             |
|         -類型          |                               識別要刪除的金鑰保護裝置。 您也可以使用 **-t**做為此命令的縮寫版本。                               |
|    msfve-recoverypassword    |                                                 指定應刪除任何修復密碼金鑰保護裝置。                                                 |
|      externalkey       |                                        指定應刪除與磁片磁碟機相關聯的任何外部金鑰保護裝置。                                         |
|      憑證 (certificate)       |                                       指定應刪除與磁片磁碟機相關聯的任何憑證金鑰保護裝置。                                       |
|          tpm           |                                        指定應刪除與磁片磁碟機相關聯的任何僅 TPM 金鑰保護裝置。                                         |
|    tpmandstartupkey    |                                指定應刪除與磁片磁碟機相關聯的任何 TPM 和啟動金鑰保護裝置。                                |
|       tpmanDPIn        |                                    指定應刪除與磁片磁碟機相關聯的任何 TPM 和 PIN 型金鑰保護裝置。                                    |
| tpmanDPInandstartupkey |                             指定應刪除與磁片磁碟機相關聯的任何 TPM、PIN 和啟動金鑰保護裝置。                             |
|        password        |                                        指定應刪除與磁片磁碟機相關聯的任何密碼金鑰保護裝置。                                         |
|        身分識別        |                                        指定應刪除與磁片磁碟機相關聯的任何身分識別金鑰保護裝置。                                         |
|          -id           |                使用金鑰識別碼來識別要刪除的金鑰保護裝置。 這個參數是 **-type**參數的替代選項。                 |
|    <KeyProtectorID>    |        識別要刪除之磁片磁碟機上的個別金鑰保護裝置。 您可以使用**manage-bde-保護裝置-get**命令來顯示金鑰保護裝置識別碼。         |
|     -computername      | 指定 manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
|         <Name>         |    代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。     |
|        -? 或/？        |                                                               在命令提示字元中顯示簡短說明。                                                               |
|      -help 或-h       |                                                             在命令提示字元中顯示完整的說明。                                                              |

### <a name="-disable-syntax-and-parameters"></a><a name=BKMK_disableprot></a>-停用語法和參數
```
manage-bde  -protectors  -disable <Drive> [-RebootCount <integer 0 - 15>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|   參數   |                                                                                                                                                                                                                   描述                                                                                                                                                                                                                    |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Drive>    |                                                                                                                                                                                                  表示後面接著冒號的磁碟機號。                                                                                                                                                                                                  |
|  RebootCount  | 從 Windows 8 開始，會指定作業系統磁片區的保護已暫停，並會在 Windows 重新開機 RebootCount 參數中指定的次數後繼續。 指定0會無限期暫停保護。 如果未指定此參數，BitLocker 保護會在 Windows 重新開機時自動繼續。 您也可以使用 **-rc**做為此命令的縮寫版本。 |
| -computername |                                                                                                                                      指定 manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。                                                                                                                                      |
|    <Name>     |                                                                                                                                         代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。                                                                                                                                          |
|   -? 或/？    |                                                                                                                                                                                                    在命令提示字元中顯示簡短說明。                                                                                                                                                                                                    |
|  -help 或-h  |                                                                                                                                                                                                  在命令提示字元中顯示完整的說明。                                                                                                                                                                                                   |

## <a name="examples"></a>範例
說明如何使用 **-保護**裝置命令，將憑證檔案所識別的憑證金鑰保護裝置新增至磁片磁碟機 E。
```
manage-bde  -protectors  -add E: -certificate  -cf c:\File Folder\Filename.cer
```
說明如何使用 **-保護**裝置命令，將網域和使用者名稱所識別的**adaccountorgroup**金鑰保護裝置新增至磁片磁碟機 E。
```
manage-bde  -protectors  -add E: -sid DOMAIN\user
```
說明如何**使用保護裝置命令來**停用保護，直到電腦重新開機3次為止。
```
manage-bde  -protectors  -disable C: -rc 3
```
說明如何使用 **-保護**裝置命令來刪除磁片磁碟機 C 上所有 TPM 和啟動金鑰的金鑰保護裝置。
```
manage-bde  -protectors -delete C: -type tpmandstartupkey
```
說明如何使用 **-保護**裝置命令，將磁片磁碟機 C 的所有修復資訊備份到 AD DS。
```
manage-bde  -protectors  -adbackup C:
```
## <a name="additional-references"></a>其他參考
-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)

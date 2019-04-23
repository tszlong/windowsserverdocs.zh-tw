---
title: 管理 bde 保護裝置
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: c804fe6fa4aca8c55bd6a312536cc453962b3c1c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852839"
---
# <a name="manage-bde-protectors"></a>管理 bde： 保護裝置

>適用於：Windows Server （半年通道），Windows Server 2016

管理用於 BitLocker 加密金鑰的保護方法。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。
## <a name="syntax"></a>語法
```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|-get|會顯示磁碟機上啟用的所有金鑰保護方法，並提供其類型和識別碼 (ID)。|
|-add|新增其他所使用的指定金鑰保護方法[將參數加入](manage-bde-protectors.md#BKMK_addprotectors)。|
|-delete|刪除 BitLocker 所使用的金鑰保護方法。 將從磁碟機移除所有金鑰保護裝置，除非選擇性[-刪除參數](manage-bde-protectors.md#BKMK_deleteprotectors)用來指定要刪除哪些保護裝置。 刪除磁碟機上的最後一個保護裝置時，磁碟機的 BitLocker 保護已停用，以確保該資料的存取權不會遺失不小心。|
|-disable|停用保護，這可讓任何人都能夠存取加密的資料，藉由加密金鑰可用磁碟機上不安全。 沒有金鑰保護裝置會移除。 保護將會繼續下一次 Windows 機器開機，除非選擇性[-停用參數](manage-bde-protectors.md#BKMK_disableprot)用來指定重新開機次數。|
|-enable|藉由移除磁碟機中的不安全的加密金鑰來啟用保護。 所有已設定金鑰保護裝置的磁碟機上將會強制執行。|
|-adbackup|若要備份至 Active Directory 網域服務 (AD DS) 中指定的磁碟機的所有修復資訊。 若要將只有單一的修復金鑰備份到 AD DS，附加 **-識別碼**參數並指定要備份特定的復原金鑰的識別碼。|
|-aadbackup|若要備份至 Azure Active Directory (Azure Ad) 中指定的磁碟機的所有修復資訊。 若要備份至 Azure AD 的只有單一的修復金鑰，請附加 **-識別碼**參數並指定要備份特定的復原金鑰的識別碼。|
|<Drive>|表示磁碟機代號，後面接著冒號。|
|-computername|指定該管理 bde.exe 會用來修改不同的電腦上的 BitLocker 保護。 您也可以使用 **-cn**為此命令縮寫版。|
|<Name>|表示要修改 BitLocker 保護之電腦的名稱。 可接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或 /？|顯示簡短說明，在命令提示字元。|
|-help 或-h|顯示完成的命令提示字元說明。|
### <a name="BKMK_addprotectors"></a>-加入語法及參數
```
manage-bde  protectors  add [<Drive>] [-forceupgrade] [-recoverypassword <NumericalPassword>] [-recoverykey <pathToExternalKeydirectory>]
[-startupkey <pathToExternalKeydirectory>] [-certificate {-cf <pathToCertificateFile>|-ct <CertificateThumbprint>}] [-tpm] [-tpmandpin] 
[-tpmandstartupkey <pathToExternalKeydirectory>] [-tpmandpinandstartupkey <pathToExternalKeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```
|參數|描述|
|-------|--------|
|<Drive>|表示磁碟機代號，後面接著冒號。|
|-recoverypassword|將數字密碼保護裝置。 您也可以使用 **-rp**為此命令縮寫版。|
|<NumericalPassword>|表示將修復密碼。|
|-recoverykey|新增復原外部金鑰保護裝置。 您也可以使用 **-rk**為此命令縮寫版。|
|<pathToExternalKeydirectory>|表示修復金鑰的目錄路徑。|
|-startupkey|將 啟動外部金鑰保護裝置。 您也可以使用 **-sk**為此命令縮寫版。|
|<pathToExternalKeydirectory>|表示啟動金鑰的目錄路徑。|
|-certificate|新增資料磁碟機的公用金鑰保護裝置。 您也可以使用 **-cert**為此命令縮寫版。|
|-cf|指定憑證檔案，可用來提供的公開金鑰憑證。|
|<pathToCertificateFile>|表示憑證檔案的目錄路徑。|
|-ct|指定憑證指紋，會用來識別的公開金鑰憑證|
|<CertificateThumbprint>|指定您想要使用的憑證 [指紋] 屬性的值。 例如，憑證的指紋值"a9 09 50 2d d8 2a e4 14 33 e6 f8 38 86 b0 0d 42 77 a3 2a 7b"應指定為"a909502dd82ae41433e6f83886b00d4277a32a7b"。|
|-tpmandpin|新增受信任的平台模組 (TPM) 和個人識別碼數字 (PIN) 保護裝置的作業系統磁碟機。 您也可以使用 **-tp**為此命令縮寫版。|
|-tpmandstartupkey|新增作業系統磁碟機的 TPM 和啟動金鑰保護裝置。 您也可以使用 **-嘖嘖**為此命令縮寫版。|
|-tpmandpinandstartupkey|新增 TPM、 PIN 和作業系統磁碟機的啟動金鑰保護裝置。 您也可以使用 **-tpsk**為此命令縮寫版。|
|-password|新增資料磁碟機的密碼金鑰保護裝置。 您也可以使用 **-pw**為此命令縮寫版。|
|-adaccountorgroup|新增安全性識別碼 (SID) 為基礎的磁碟區的身分識別保護裝置。  您也可以使用 **-sid**為此命令縮寫版。 **重要：** 根據預設，您無法加入從遠端使用 WMI 或管理 bde ADAccountOrGroup 保護裝置。  如果您的部署必須能夠從遠端新增此保護裝置，您必須啟用限制的委派。|
|-computername|指定該管理 bde 用來修改不同的電腦上的 BitLocker 保護。 您也可以使用 **-cn**為此命令縮寫版。|
|<Name>|表示要修改 BitLocker 保護之電腦的名稱。 可接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
### <a name="BKMK_deleteprotectors"></a>-刪除語法和參數
```
manage-bde  protectors  delete <Drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}] 
[-id <KeyProtectorID>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
|參數|描述|
|-------|--------|
|<Drive>|表示磁碟機代號，後面接著冒號。|
|-type|識別要刪除的金鑰保護裝置。 您也可以使用 **-t**為此命令縮寫版。|
|recoverypassword|指定應該刪除任何修復密碼金鑰保護裝置。|
|externalkey|指定應該刪除與磁碟機相關聯的任何外部金鑰保護裝置。|
|憑證|指定應該刪除與磁碟機相關聯的金鑰保護裝置任何憑證。|
|tpm|指定應該刪除任何僅使用 TPM 金鑰保護裝置相關聯的磁碟機。|
|tpmandstartupkey|指定應該刪除任何 TPM 和啟動主要根據金鑰保護裝置相關聯的磁碟機。|
|tpmandpin|指定應該刪除任何 TPM 和 PIN 基礎磁碟機相關聯的金鑰保護裝置。|
|tpmandpinandstartupkey|指定應該刪除任何 TPM、 PIN 和啟動主要根據金鑰保護裝置相關聯的磁碟機。|
|密碼|指定應該刪除與磁碟機相關聯任何密碼金鑰保護裝置。|
|身分|指定應該刪除與磁碟機相關聯的金鑰保護裝置任何身分識別。|
|-id|識別金鑰的保護裝置，若要使用的金鑰識別項刪除。 這個參數時的替代選項 **-類型**參數。|
|<KeyProtectorID>|識別個別金鑰保護裝置的磁碟機上刪除。 金鑰保護裝置識別碼可以使用來顯示**管理 bde-保護裝置-取得**命令。|
|-computername|指定該管理 bde.exe 會用來修改不同的電腦上的 BitLocker 保護。 您也可以使用 **-cn**為此命令縮寫版。|
|<Name>|表示要修改 BitLocker 保護之電腦的名稱。 可接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或 /？|顯示簡短說明，在命令提示字元。|
|-help 或-h|顯示完成的命令提示字元說明。|
### <a name="BKMK_disableprot"></a>-停用語法和參數
```
manage-bde  protectors  disable <Drive> [-RebootCount <integer 0 - 15>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
|參數|描述|
|-------|--------|
|<Drive>|表示磁碟機代號，後面接著冒號。|
|RebootCount|從 Windows 8 中，指定作業系統磁碟區的保護已暫停，就能繼續使用 Windows 之後重新啟動 RebootCount 參數中指定的次數。 指定 0 會無限期暫停保護。 如果這個參數未指定 BitLocker 保護便會自動，繼續重新啟動 Windows 時。 您也可以使用 **-rc**為此命令縮寫版。|
|-computername|指定該管理 bde.exe 會用來修改不同的電腦上的 BitLocker 保護。 您也可以使用 **-cn**為此命令縮寫版。|
|<Name>|表示要修改 BitLocker 保護之電腦的名稱。 可接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。|
|-? 或 /？|顯示簡短說明，在命令提示字元。|
|-help 或-h|顯示完成的命令提示字元說明。|
## <a name="BKMK_Examples"></a>範例
下列範例說明如何利用 **-保護裝置**命令來新增憑證的金鑰保護裝置，識別要 e 磁碟機的憑證檔案
```
manage-bde  protectors  add E: -certificate  cf "c:\File Folder\Filename.cer"
```
下列範例說明如何利用 **-保護裝置**命令來新增**adaccountorgroup** e 磁碟機的網域和使用者名稱所識別的金鑰保護裝置
```
manage-bde  protectors  add E: -sid DOMAIN\user
```
下列範例說明如何使用 * * 保護裝置 * * 命令來停用保護，直到已重新啟動電腦 3 次。
```
manage-bde  protectors  disable C: -rc 3
```
下列範例說明如何利用 **-保護裝置**命令來刪除所有的 TPM 和啟動金鑰會根據 c 磁碟機中的金鑰保護裝置
```
manage-bde  protectors  delete C: -type tpmandstartupkey
```
下列範例說明如何利用 **-保護裝置**磁碟機 C 的所有修復資訊都備份到 AD DS 的命令。
```
manage-bde  protectors  adbackup C:
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)

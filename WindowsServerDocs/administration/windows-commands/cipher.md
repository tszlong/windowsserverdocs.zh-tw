---
title: cipher
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78ef795e-0f87-4acd-8d15-192c972c0f41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d801d6e6286e97319766c879f7289f6191cc7101
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434331"
---
# <a name="cipher"></a>cipher



顯示或變更 NTFS 磁碟區上的目錄和檔案的加密。 如果未指定參數，使用**cipher**顯示目前的目錄和它所包含的任何檔案的加密狀態。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
cipher [/e | /d | /c] [/s:<Directory>] [/b] [/h] [PathName [...]]
cipher /k
cipher /r:<FileName> [/smartcard]
cipher /u [/n]
cipher /w:<Directory>
cipher /x[:efsfile] [FileName]
cipher /y
cipher /adduser [/certhash:<Hash> | /certfile:<FileName>] [/s:Directory] [/b] [/h] [PathName [...]]
cipher /removeuser /certhash:<Hash> [/s:<Directory>] [/b] [/h] [<PathName> [...]]
cipher /rekey [PathName [...]]
```

## <a name="parameters"></a>參數

|          參數           |                                                                                                                                                   描述                                                                                                                                                    |
|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              /b               |                                                                                                    如果發生錯誤，就會中止。 根據預設， **cipher**會繼續執行，即使發生錯誤。                                                                                                    |
|              /c               |                                                                                                                                   加密的檔案會顯示資訊。                                                                                                                                    |
|              /d               |                                                                                                                                   解密指定的檔案或目錄。                                                                                                                                   |
|              /e               |                                                                                          加密指定的檔案或目錄。 目錄會標示，好讓之後新增的檔案將會加密。                                                                                           |
|              /h               |                                                                                                     會顯示檔案與隱藏或系統屬性。 根據預設，這些檔案未加密或解密。                                                                                                     |
|              /k               |                                                                            使用加密檔案系統 (EFS) 檔案，會建立新的憑證和金鑰以供使用。 如果 **/k**參數指定，則會忽略所有其他參數。                                                                            |
|  /r:\<FileName> [/smartcard]  |   產生的 EFS 修復代理程式金鑰和憑證，然後將它們寫入至.pfx 檔案 （包含憑證和私密金鑰） 和.cer 檔案 （包含只有憑證）。 如果 **/smartcard**指定時，它將修復金鑰和憑證寫入智慧卡，並會產生任何.pfx 檔。   |
|        /s:\<目錄 >        |                                                                                                               執行指定的作業上所有子目錄中的指定*Directory*。                                                                                                               |
|            /u [/n]            |  尋找本機的磁碟機上的所有已加密的檔案。 如果搭配 **/n**參數進行任何更新。 如果使用不含 **/n**， **/u**比較使用者的檔案加密金鑰或修復代理的索引鍵的目前項目，並加以更新，如果它們已變更。 此參數僅適用於 **/n**。  |
|        /w:\<目錄 >        | 移除整個磁碟區上可用的未使用的磁碟空間中的資料。 如果您使用 **/w**參數，則會忽略所有其他參數。 指定的目錄可以位於任何位置的本機磁碟區。 如果是掛接點，或指向另一個磁碟區，資料中的目錄上，也會移除磁碟區。 |
|  /x[:efsfile] [\<FileName>]   |                                 將備份 EFS 憑證及金鑰指定的檔案名稱。 如果搭配 **: efsfile**， **/x**備份用來加密檔案的使用者的憑證。 否則，目前使用者的 EFS 憑證及金鑰會備份。                                 |
|              /y               |                                                                                                                      在本機電腦上，會顯示您目前的 EFS 憑證縮圖。                                                                                                                      |
|  /adduser [/ certhash:\<雜湊 >  |                                                                                                                                              /certfile:<FileName>]                                                                                                                                               |
|            /rekey             |                                                                                                                 更新指定的加密的檔案，以使用目前設定的 EFS 金鑰。                                                                                                                 |
| /removeuser /certhash:\<雜湊 > |                                                                                       從指定的檔案中移除使用者。 *雜湊*供 **/certhash**必須是要移除之憑證的 SHA1 雜湊。                                                                                       |
|              /?               |                                                                                                                                       在命令提示字元顯示說明。                                                                                                                                       |

## <a name="remarks"></a>備註

-   如果未加密的父目錄，它遭到修改時，無法成為解密已加密的檔案。 因此，當您加密檔案時，您也應該加密的上層目錄。
-   系統管理員可以將.cer 檔案的內容新增至 EFS 修復原則，來建立使用者的修復代理程式，，然後匯入.pfx 檔案來復原個別檔案。
-   您可以使用多個目錄名稱和萬用字元。
-   您必須讓多個參數之間的空格。

## <a name="BKMK_examples"></a>範例

若要顯示目前目錄中的每個檔案和子目錄的加密狀態，請輸入：
```
cipher
```
加密的檔案和目錄會標示**E**。未加密的檔案和目錄會標示**U**。例如，下列的輸出會指出目前的目錄和其所有內容都目前未加密：
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
U Private
U hello.doc
U hello.txt
```
若要啟用在上述範例中所使用的私用目錄上的加密，請輸入：
```
cipher /e private
```
此時會顯示下列輸出：
```
Encrypting files in C:\Users\MainUser\Documents\
Private             [OK]
1 file(s) [or directorie(s)] within 1 directorie(s) were encrypted.
```
**Cipher**命令會顯示下列輸出：
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
E Private
U hello.doc
U hello.txt
```
請注意，標記為私用的目錄以加密。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
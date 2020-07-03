---
title: cipher
description: Encryption 命令的參考文章，它會顯示或變更 NTFS 磁片區上的目錄和檔案的加密。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 78ef795e-0f87-4acd-8d15-192c972c0f41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f23e2855c4542cc359d458f6825dd9420581657f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929975"
---
# <a name="cipher"></a>cipher

顯示或變更 NTFS 磁碟區上的目錄和檔案的加密。 如果在沒有參數的情況下使用， **cipher**會顯示目前目錄及其包含之任何檔案的加密狀態。

## <a name="syntax"></a>語法

```
cipher [/e | /d | /c] [/s:<directory>] [/b] [/h] [pathname [...]]
cipher /k
cipher /r:<filename> [/smartcard]
cipher /u [/n]
cipher /w:<directory>
cipher /x[:efsfile] [filename]
cipher /y
cipher /adduser [/certhash:<hash> | /certfile:<filename>] [/s:directory] [/b] [/h] [pathname [...]]
cipher /removeuser /certhash:<hash> [/s:<directory>] [/b] [/h] [<pathname> [...]]
cipher /rekey [pathname [...]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ---------- | ----------- |
| /b | 如果發生錯誤，則中止。 根據預設，即使遇到錯誤， **cipher**還是會繼續執行。 |
| /C | 顯示加密檔案的資訊。 |
| /d | 解密指定的檔案或目錄。 |
| /e | 加密指定的檔案或目錄。 系統會標示目錄，以便在之後新增的檔案會進行加密。 |
| /h | 顯示具有隱藏或系統屬性的檔案。 根據預設，這些檔案不會經過加密或解密。 |
| /k | 建立要與加密檔案系統（EFS）檔案搭配使用的新憑證和金鑰。 如果指定了 **/k**參數，則會忽略所有其他參數。 |
| /r： `<filename>` [/smartcard] | 會產生 EFS 復原代理程式金鑰和憑證，然後將它們寫入 .pfx 檔案（包含憑證和私密金鑰）和 .cer 檔案（僅包含憑證）。 如果指定 **/smartcard** ，則會將修復金鑰和憑證寫入智慧卡，且不會產生 .pfx 檔案。 |
| /s`<directory>` | 在指定*目錄*中的所有子目錄上執行指定的操作。 |
| /u [/n] |  尋找本機磁片磁碟機上的所有加密檔案。 如果搭配 **/n**參數使用，則不會進行任何更新。 如果使用時不含 **/n**， **/u**會將使用者的檔案加密金鑰或復原代理程式的金鑰與目前的金鑰進行比較，並在變更時加以更新。 這個參數只能搭配 **/n**使用。 |
| /w`<directory>` | 從整個磁片區上可用的未使用磁碟空間移除資料。 如果您使用 **/w**參數，則會忽略所有其他參數。 指定的目錄可以位於本機磁片區中的任何位置。 如果是掛接點或指向另一個磁片區中的目錄，則會移除該磁片區上的資料。 |
| /x [： efsfile] [ `<FileName>` ] | 將 EFS 憑證和金鑰備份至指定的檔案名。 如果搭配使用 **： efsfile**， **/x**會備份用來加密檔案的使用者憑證。 否則，會備份使用者目前的 EFS 憑證和金鑰。 |
| /y | 顯示本機電腦上目前的 EFS 憑證縮圖。 |
| /adduser [/certhash:`<hash>` | /certfile： `<filename>` ] |
| /rekey | 更新指定的加密檔案，以使用目前設定的 EFS 金鑰。 |
| /removeuser /certhash:`<hash>` | 從指定的檔案中移除使用者。 提供給 **/certhash**的*雜湊*必須是要移除之憑證的 SHA1 雜湊。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="remarks"></a>備註

- 如果上層目錄未加密，加密的檔案可能會在修改時被解密。 因此，當您加密檔案時，您也應該加密上層目錄。

- 系統管理員可以將 .cer 檔案的內容新增至 EFS 復原原則，為使用者建立修復代理，然後匯入 .pfx 檔案來復原個別檔案。

- 您可以使用多個目錄名稱和萬用字元。

- 您必須在多個參數之間加上空格。

## <a name="examples"></a>範例

若要顯示目前目錄中每個檔案和子目錄的加密狀態，請輸入：

```
cipher
```

加密的檔案和目錄會以**E**標示。未加密的檔案和目錄會標示為**U**。例如，下列輸出表示目前目錄及其所有內容都已加密：

```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
U Private
U hello.doc
U hello.txt
```

若要在上一個範例中使用的私用目錄上啟用加密，請輸入：

```
cipher /e private
```

下列輸出會顯示：

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

其中，**私**用目錄現在會標示為已加密。

### <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

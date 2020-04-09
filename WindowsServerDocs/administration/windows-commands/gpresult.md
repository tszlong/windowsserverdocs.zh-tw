---
title: gpresult
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 480599a4040ab1fdcc3842cdb0eaa8c35afa873c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842461"
---
# <a name="gpresult"></a>gpresult

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端使用者和電腦的原則結果組（RSoP）資訊。
若要透過防火牆針對遠端目的電腦使用 RSoP 報告，您必須具有可在埠上啟用輸入網路流量的防火牆規則。

## <a name="syntax"></a>語法

```
gpresult [/s <system> [/u <USERNAME> [/p [<PASSWOrd>]]]] [/user [<TARGETDOMAIN>\]<TARGETUSER>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <FILENAME> [/f] | /?}
```

### <a name="parameters"></a>參數

> [!NOTE]
> 除了使用 **/？** 時，您必須包含 **/r**、 **/v**、 **/z**、 **/x**或 **/h**這兩個輸出選項。

|                參數                 |                                                                                                     描述                                                                                                      |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              /s \<系統\>               |                                                  指定遠端電腦的名稱或 IP 位址。 請勿使用反斜線。 預設是本機電腦。                                                   |
|             /u \<使用者名稱\>              |                                使用指定使用者的認證來執行命令。 預設使用者是登入發出命令之電腦的使用者。                                 |
|            /p [\<密碼\>]             |            指定 **/u**參數中提供之使用者帳戶的密碼。 如果省略 **/p** ， **gpresult**會提示輸入密碼。 **/p**不能搭配 **/x**或 **/h**使用。            |
| /user [\<TARGETDOMAIN\>\\]\<TARGETUSER\> |                                                                            指定要顯示其 RSoP 資料的遠端使用者。                                                                             |
|      /scope {user &#124; computer}       |                                顯示使用者或電腦的 RSoP 資料。 如果省略 **/scope** ， **gpresult**會顯示使用者和電腦的 RSoP 資料。                                 |
|        [/x &#124; /h] <FILENAME>         | 以 XML （ **/x**）或 HTML （ **/h**）格式將報表儲存在位置，並以*FILENAME*參數所指定的檔案名儲存。 不能與 **/u**、 **/p**、 **/r**、 **/v**或 **/z**搭配使用。 |
|                    /f                    |                                                           強制**gpresult**覆寫 **/x**或 **/h**選項中所指定的檔案名。                                                           |
|                    /r                    |                                                                                             顯示 RSoP 摘要資料。                                                                                              |
|                    /v                    |                                                    顯示詳細資訊原則資訊。 這包括優先順序為1的詳細設定。                                                    |
|                    /z                    |                                     顯示群組原則的所有可用資訊。 這包括已套用優先順序為1和更高的詳細設定。                                      |
|                    /?                    |                                                                                         在命令提示字元顯示說明。                                                                                         |

## <a name="remarks"></a>備註
- 群組原則是主要的系統管理工具，用來定義和控制程式、網路資源以及作業系統如何操作組織中的使用者和電腦。 在 active directory 環境中，群組原則會根據其在網站、網域或組織單位中的成員資格，套用至使用者或電腦。
- 因為您可以將重迭的原則設定套用至任何電腦或使用者，所以群組原則功能會在使用者登入時產生一組產生的原則設定。 **gpresult**顯示使用者登入時，針對指定使用者在電腦上強制執行的一組原則設定。
- 由於 **/v**和 **/z**會產生許多資訊，因此將輸出重新導向至文字檔（例如， **gpresult/z > 的原則 .txt**）會很有用。
- **Gpresult**命令適用于 windows server 2012、windows Server 2008 R2、windows Server2008、windows 8、windows 7 和 windows Vista。
  ## <a name="examples"></a>範例
  下列範例會抓取電腦**srvmain**的遠端使用者**targetusername**的 rsop 資料，並只顯示有關使用者的 rsop 資料。 命令會以使用者**maindom\hiropln**的認證來執行，並輸入<strong>p@ssW23</strong>做為該使用者的密碼。

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
  ```
  
下列範例會將電腦**srvmain**的遠端使用者**targetusername**的所有可用群組原則資訊儲存至名為**Policy .txt**的檔案。 電腦未包含任何資料。 命令會以使用者**maindom\hiropln**的認證來執行，並輸入<strong>p@ssW23</strong>做為該使用者的密碼。

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
  ```
  
下列範例會顯示電腦**srvmain**和已登入使用者的 RSoP 資料。 同時包含使用者和電腦的相關資料。 命令會以使用者**maindom\hiropln**的認證來執行，並輸入<strong>p@ssW23</strong>做為該使用者的密碼。

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
  ```
  
## <a name="additional-references"></a>其他參考資料
- [群組原則技術中心](https://go.microsoft.com/fwlink/?LinkID=145531)

- - [命令列語法關鍵](command-line-syntax-key.md)

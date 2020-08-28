---
title: gpresult
description: Gpresult 命令的參考文章，會顯示遠端使用者和電腦的原則結果組 (RSoP) 資訊。
ms.topic: reference
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ef5de0c8e4e4c4f75d8ccd680e20b8cf00385f5
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025672"
---
# <a name="gpresult"></a>gpresult

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端使用者和電腦的原則結果組 (RSoP) 資訊。 若要將 RSoP 報告用於透過防火牆的遠端目的電腦，您必須擁有可在埠上啟用輸入網路流量的防火牆規則。

## <a name="syntax"></a>語法

```
gpresult [/s <system> [/u <username> [/p [<password>]]]] [/user [<targetdomain>\]<targetuser>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <filename> [/f] | /?}
```

> [!NOTE]
> 除非使用 **/？**，否則您必須包含 output 選項： **/r**、 **/v**、 **/z**、 **/x**或 **/h**。

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /s `<system>` | 指定遠端電腦的名稱或 IP 位址。 請勿使用反斜線。 預設是本機電腦。 |
| u `<username>` | 使用指定使用者的認證執行命令。 預設使用者是登入發出命令之電腦的使用者。 |
| /p `[<password>]` | 指定 **/u** 參數中所提供使用者帳戶的密碼。 如果省略 **/p** ，則 **會提示您輸入密碼** 。 **/P**參數不可與 **/x**或 **/h**一起使用。 |
| /user `[<targetdomain>\]<targetuser>]` | 指定要顯示其 RSoP 資料的遠端使用者。 |
| /scope `{user | computer}` | 顯示使用者或電腦的 RSoP 資料。 如果省略 **/scope** ， **gpresult** 會顯示使用者和電腦的 RSoP 資料。 |
| `[/x | /h] <filename>` | 將報表儲存為 XML (**/x**) 或 HTML (**/h**) 格式的位置，以及 *檔案名* 參數所指定的檔案名。 無法搭配 **/u**、 **/p**、 **/r**、 **/v**或 **/z**使用。 |
| /f | 強制 **gpresult** 覆寫在 **/x** 或 **/h** 選項中指定的檔案名。 |
| /r | 顯示 RSoP 摘要資料。 |
| /v | 顯示詳細資訊原則資訊。 這包括以1優先順序套用的詳細設定。 |
| /z | 顯示有關群組原則的所有可用資訊。 這包括以1和更高優先順序套用的詳細設定。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 群組原則是用來定義及控制程式、網路資源和作業系統如何針對組織中的使用者和電腦操作的主要系統管理工具。 在 active directory 環境中，群組原則會根據其在網站、網域或組織單位中的成員資格，套用至使用者或電腦。

- 由於您可以將重迭的原則設定套用至任何電腦或使用者，因此群組原則功能會在使用者登入時產生一組結果的原則設定。 **Gpresult**命令顯示當使用者登入時，針對指定的使用者在電腦上強制執行的一組原則設定。

- 因為 **/v** 和 **/z** 會產生大量資訊，所以將輸出重新導向至文字檔會很有用 (例如 `gpresult/z >policy.txt`) 。

### <a name="examples"></a>範例

若只要取得遠端使用者的 RSoP 資料，請使用電腦 srvmain 上的密碼*maindom\hiropln* ， *p@ssW23* 輸入： *srvmain*

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
```

若要將有關群組原則的所有可用資訊儲存到名為*policy.txt*的檔案，請*maindom\hiropln* *p@ssW23* 在 [電腦*srvmain*] 中輸入：

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
```

若要顯示已登入使用者的 RSoP 資料，請 *maindom\hiropln* [ *p@ssW23* 電腦 *srvmain*] 的密碼，輸入：

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

---
title: gpresult
description: Gpresult 命令的參考主題，會顯示遠端使用者和電腦的原則結果組（RSoP）資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e88a75a15168baaf2e49ca08ff20d3a8ffb5620c
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818858"
---
# <a name="gpresult"></a>gpresult

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端使用者和電腦的原則結果組（RSoP）資訊。 若要透過防火牆針對遠端目的電腦使用 RSoP 報告，您必須具有可在埠上啟用輸入網路流量的防火牆規則。

## <a name="syntax"></a>語法

```
gpresult [/s <system> [/u <username> [/p [<password>]]]] [/user [<targetdomain>\]<targetuser>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <filename> [/f] | /?}
```

> [!NOTE]
> 除了使用 **/？** 時，您必須包含輸出選項、 **/r**、 **/v**、 **/z**、 **/x**或 **/h**。

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /s`<system>` | 指定遠端電腦的名稱或 IP 位址。 請勿使用反斜線。 預設是本機電腦。 |
| u`<username>` | 使用指定使用者的認證來執行命令。 預設使用者是登入發出命令之電腦的使用者。 |
| /p`[<password>]` | 指定 **/u**參數中提供之使用者帳戶的密碼。 如果省略 **/p** ， **gpresult**會提示輸入密碼。 **/P**參數不能搭配 **/x**或 **/h**使用。 |
| /user`[<targetdomain>\]<targetuser>]` | 指定要顯示其 RSoP 資料的遠端使用者。 |
| /scope`{user | computer}` | 顯示使用者或電腦的 RSoP 資料。 如果省略 **/scope** ， **gpresult**會顯示使用者和電腦的 RSoP 資料。 |
| `[/x | /h] <filename>` | 以 XML （**/x**）或 HTML （**/h**）格式將報表儲存在位置，並以*filename*參數所指定的檔案名儲存。 不能與 **/u**、 **/p**、 **/r**、 **/v**或 **/z**搭配使用。 |
| /f | 強制**gpresult**覆寫 **/x**或 **/h**選項中所指定的檔案名。 |
| /r | 顯示 RSoP 摘要資料。 |
| /v | 顯示詳細資訊原則資訊。 這包括優先順序為1的詳細設定。 |
| /z | 顯示群組原則的所有可用資訊。 這包括已套用優先順序為1和更高的詳細設定。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 群組原則是主要的系統管理工具，用來定義和控制程式、網路資源以及作業系統如何操作組織中的使用者和電腦。 在 active directory 環境中，群組原則會根據其在網站、網域或組織單位中的成員資格，套用至使用者或電腦。

- 因為您可以將重迭的原則設定套用至任何電腦或使用者，所以群組原則功能會在使用者登入時產生一組產生的原則設定。 **Gpresult**命令會顯示使用者登入時，針對指定使用者在電腦上強制執行的一組原則設定。

- 由於 **/v**和 **/z**會產生許多資訊，因此將輸出重新導向至文字檔（例如，）會很有説明 `gpresult/z >policy.txt` 。

### <a name="examples"></a>範例

若只要針對遠端使用者取出 RSoP 資料，請使用 [電腦 srvmain] 上的 [密碼] *maindom\hiropln* *p@ssW23* ，輸入： *srvmain*

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
```

若要將群組原則的所有可用資訊儲存到名為 maindom\hiropln 的*檔案，請* *maindom\hiropln* *p@ssW23* 在 [電腦*srvmain*] 上輸入：

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
```

若要顯示已登入使用者的 RSoP 資料，請使用*maindom\hiropln*的密碼 *p@ssW23* ，針對電腦*srvmain*輸入：

```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

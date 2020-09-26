---
title: rundll32.exe printui.dll、PrintUIEntry
description: Rundll32.exe printui.dll PrintUIEntry 命令的參考文章，可自動化許多印表機設定工作。
ms.topic: reference
ms.assetid: 12fb48b6-5dd8-4cc0-8808-e6a681aceb84
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 05/25/2018
ms.openlocfilehash: 8ad0b9b3cfdca8d9ed82cc980d29df7c885bac46
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388449"
---
# <a name="rundll32-printuidllprintuientry"></a>rundll32.exe printui.dll、PrintUIEntry

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

自動化許多印表機設定工作。 printui.dll 是包含 [印表機設定] 對話方塊所使用之功能的可執行檔。 您也可以從腳本或命令列批次檔中呼叫這些函式，也可以從命令提示字元以互動方式執行這些函數。

## <a name="syntax"></a>語法

```
rundll32 printui.dll PrintUIEntry [baseparameter] [modificationparameter1] [modificationparameter2] [modificationparameterN]
```

您也可以使用下列替代語法，雖然本主題中的範例使用先前的語法：

```
rundll32 printui.dll,PrintUIEntry [baseparameter] [modificationparameter1] [modificationparameter2] [ModificationParameterN]
```

```
rundll32 printui PrintUIEntry [baseparameter] [modificationparameter1] [modificationparameter2] [modificationparameterN]
```

```
rundll32 printui,PrintUIEntry [baseparameter] [modificationparameter1] [modificationparameter2] [modificationparameterN]
```

### <a name="parameters"></a>參數

參數有兩種類型：基底參數和修改參數。 基底參數指定命令要執行的函式。 只有其中一個參數可以出現在指定的命令列中。 然後，您可以使用一或多個修改參數來修改基底參數（如果這些參數適用于基底參數） (並非所有的基底參數都支援所有的修改參數) 。

| 基底參數 | 說明 |
|--|--|
| /dl | 刪除本機印表機。 |
| /dn | 刪除網路印表機連接。 |
| /dd | 刪除印表機驅動程式。 |
| /e | 顯示指定印表機的列印喜好設定。 |
| /ga | 新增每部電腦印表機連線 (當電腦登入) 時，該連線可供該電腦上的任何使用者使用。 |
| /ge | 顯示電腦上每台電腦的印表機連接。 |
| /gd | 刪除每台電腦印表機連線 (下次使用者) 登入時，就會刪除該連接。 |
| /ia | 使用 .inf 檔案安裝印表機驅動程式。 |
| /id | 使用 [新增印表機驅動程式嚮導] 安裝印表機驅動程式。 |
| /if | 使用 .inf 檔案安裝印表機。 |
| /ii | 使用 [新增印表機嚮導] 搭配 .inf 檔案安裝印表機。 |
| /il | 使用 [新增印表機嚮導] 安裝印表機。 |
| /in | 連接到遠端網路印表機。 |
| /ip | 使用 [網路印表機安裝] 安裝程式安裝印表機 (可從列印管理) 的使用者介面使用。 |
| /k | 列印印表機上的測試頁面。 |
| /o | 顯示印表機的佇列。 |
| /p | 顯示印表機的屬性。 當您使用這個參數時，您也必須指定修改參數 **/n [name]** 的值。 |
| /s | 顯示列印伺服器的屬性。 如果您想要查看本機列印伺服器，就不需要使用修改參數。 但是，如果您想要查看遠端列印伺服器，就必須指定 **/c [name]** 修改參數。 |
| /Ss | 指定將儲存印表機的資訊類型。 如果未指定 **/Ss** 的值，預設行為就如同已指定所有值一樣。 使用這個基底參數，並將下列值放在命令列的結尾：<ul><li>**2**：儲存印表機 printER_INFO_2 結構中所包含的資訊。 此結構包含印表機的基本資訊，例如其名稱、伺服器名稱、埠名稱和共用名稱。</li><li>**7**：用於儲存 printER_INFO_7 結構中所包含的目錄服務資訊。</li><li>**c**：儲存印表機的色彩設定檔資訊。</li><li>**d**：儲存印表機特定的資料，例如印表機的硬體識別碼。</li><li>**s**：儲存印表機的安全描述項。</li><li>**g**：將資訊儲存在印表機的全域 DEVmode 結構中。</li><li>**m**：儲存印表機的最基本設定。 這相當於指定 **2** **d**和 **g**。</li><li>**u**：將資訊儲存在印表機的每個使用者 DEVmode 結構中。</li></ul> |
| /Sr | 指定要還原的印表機相關資訊，以及如何處理設定中的衝突。 使用，並將下列值放在命令列的結尾：<ul><li>**2**：還原印表機 printER_INFO_2 結構中包含的資訊。 此結構包含印表機的基本資訊，例如其名稱、伺服器名稱、埠名稱和共用名稱。</li><li>**7**：還原 printER_INFO_7 結構中所包含的目錄服務資訊。</li><li>**c**：還原印表機的色彩設定檔資訊。</li><li>**d**：還原印表機特定的資料，例如印表機的硬體識別碼。</li><li>**s**：還原印表機的安全描述項。</li><li>**g**：還原印表機的全域 DEVmode 結構中的資訊。</li><li>**m**：還原印表機的最基本設定。 這相當於指定 **2**、 **d**和 **g**。</li><li>**u** 還原 printe 中每個使用者的 DEVmode 結構的資訊。</li><li>**r**：如果儲存在檔案中的印表機名稱與要還原的印表機名稱不同，請使用目前的印表機名稱。 這不能以 **f**指定。 如果 **r** 和 **f** 都未指定，且名稱不相符，則還原設定會失敗。</li><li>**f**：如果儲存在檔案中的印表機名稱與要還原的印表機名稱不同，請使用檔案中的印表機名稱。 這不能與 **r**一起指定。 如果沒有指定 **f** 或 **r** ，且名稱不相符，則還原設定會失敗。</li><li>**p**：如果正在還原之檔案中的埠名稱不符合正在還原的印表機目前的埠名稱，則會使用印表機目前的埠名稱。</li><li>**h**：如果還原的印表機無法使用儲存的設定檔案中的資源分享名來共用，則嘗試使用目前的共用名稱或新產生的共用名稱來共用印表機，如果未指定 **h** 或 **h** ，而且還原的印表機無法與儲存的共用名稱共用，則還原會失敗。</li><li>**h**：如果還原的印表機無法與儲存的共用名稱共用，則請勿共用印表機。 如果未指定 **h** 和 **h** ，而且還原的印表機無法與儲存的共用名稱共用，則還原會失敗。</li><li>**i**：如果儲存的設定檔中的驅動程式與要還原之印表機的驅動程式不相符，則還原會失敗。 |
| /Xg | 抓取印表機的設定。 |
| /Xs | 設定印表機的設定。 |
| /y | 將安裝的印表機設定為預設印表機。 |
| /? | 顯示命令的產品內說明和其相關聯的參數。 |
| @ [file] | 指定命令列引數檔案，並將該檔案中的文字直接插入命令列中。 |

| 修改參數 | 說明 |
|--|--|
| /a [file] | 指定二進位檔案名稱。 |
| /b [name] | 指定基礎印表機名稱。 |
| /c [name] | 如果要執行的動作是在遠端電腦上，請指定電腦名稱稱。 |
| /f [file] | 根據您所執行的工作，物種通用命名慣例 (UNC) 路徑和名稱，以及 .inf 檔案名或輸出檔案名。 使用 **/f [file]** 指定相依的 .inf 檔案。 |
| /F [file] | 指定使用 **/f [file]** 所指定之 .inf 檔案的 UNC 路徑與 .inf 檔案的名稱。 |
| /h [架構] | 指定驅動程式架構。 使用下列其中一項： **x86**、 **x64**或 **Itanium**。 |
| /j [provider] | 指定列印提供者名稱。 |
| /l [path] | 指定您所使用的印表機驅動程式檔案所在的 UNC 路徑。 |
| /m [模型] | 指定驅動程式模型名稱。  (可以在 .inf 檔案中指定此值。 )  |
| /n [名稱] | 指定印表機名稱。 |
| /q | 執行命令，而不通知使用者。 |
| /r [埠] | 指定埠名稱。 |
| /U | 指定使用現有的印表機驅動程式（如果已安裝）。 |
| /t [#] | 指定要開始的以零起始的索引頁面。 |
| /v [版本] | 指定驅動程式版本。 如果您還沒有指定 **/k**的值，您必須指定下列其中一個值： **Type 2-Kernel 模式** 或 **類型 3-使用者模式**。 |
| /W | 如果在 **/f**指定的 .inf 檔案中找不到驅動程式，會提示使用者提供驅動程式。 |
| /Y | 指定不應自動產生印表機名稱。 |
| /z | 指定不自動共用正在安裝的印表機。 |
| /K | 將參數 **/h [架構]** 的意義變更為接受 **2** 取代 **x86**、 **3** 取代 **x64**，或 **4** 取代 **Itanium**。 此外，它也會將參數 **/v [version]** 的值變更為 [ **2]-核心模式**和**3**的值，以取代**類型3使用者模式**。 **2** |
| /Z | 共用正在安裝的印表機。 只搭配 **/if** 參數使用。 |
| /Mw [message] | 在認可命令列中指定的變更之前，對使用者顯示警告訊息。 |
| /Mq [message] | 在認可命令列中指定的變更之前，對使用者顯示確認訊息。 |
| /W [旗標] | 指定 [新增印表機嚮導]、[新增印表機驅動程式] 和 [網路印表機安裝精靈] 的任何參數或選項。<p>**r**：讓您能夠從最後一頁重新開機嚮導。 |
| /G [旗標] | 指定您想要使用的全域參數和選項。<p>**w**：抑制使用者的安裝驅動程式警告。 |

#### <a name="remarks"></a>備註

- **PrintUIEntry**關鍵字會區分大小寫，而且您必須使用本主題中範例所示的確切大小寫來輸入此命令的語法。

- 如需更多範例，請在命令提示字元中輸入： **rundll32.exe printui.dll，PrintUIEntry/？**

## <a name="examples"></a>範例

若要加入新的遠端印表機，請 printer1，針對執行此命令的使用者帳戶，Client1，這是可看見的電腦，輸入：

```
rundll32 printui.dll PrintUIEntry /in /n\\client1\printer1
```

若要使用 [新增印表機] 嚮導新增印表機，並使用 .inf 檔案 InfFile，請在磁片磁碟機 c： at Infpath 輸入：

```
rundll32 printui.dll PrintUIEntry /ii /f c:\Infpath\InfFile.inf
```

若要刪除現有的印表機，請 printer1 在電腦上的 Client1 上，輸入：

```
rundll32 printui.dll PrintUIEntry /dn /n\\client1\printer1
```

若要為電腦的所有使用者新增每台電腦的印表機連線，請 printer2，Client2，輸入 (當使用者登入) 時，將會套用連接：

```
rundll32 printui.dll PrintUIEntry /ga /n\\client2\printer2
```

若要刪除每一電腦的印表機連線，請 printer2，針對電腦的所有使用者，Client2，輸入 (當使用者登入) 時，將會刪除連線：

```
rundll32 printui.dll PrintUIEntry /gd /n\\client2\printer2
```

若要查看列印伺服器的屬性，請 printServer1，輸入：

```
rundll32 printui.dll PrintUIEntry /s /t1 /c\\printserver1
```

若要查看印表機的屬性，請 printer3，輸入：
```
rundll32 printui.dll PrintUIEntry /p /n\\printer3
```

## <a name="additional-references"></a>其他參考

- [rundll32](rundll32.md)

- [列印命令參考](print-command-reference.md)

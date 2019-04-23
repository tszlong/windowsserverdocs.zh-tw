---
title: rundll32 printui.dll,PrintUIEntry
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12fb48b6-5dd8-4cc0-8808-e6a681aceb84 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/25/2018
ms.openlocfilehash: c90641820bfa01c19ae7bf587c5467d3f9c5a01c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836839"
---
# <a name="rundll32-printuidllprintuientry"></a>rundll32 printui.dll,PrintUIEntry

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

會自動執行許多的印表機設定工作。 printui.dll 是可執行檔包含印表機設定對話方塊所使用的函式。 這些函式也可以呼叫從指令碼或命令列的批次檔，或可以執行以互動方式從命令提示字元。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。  
## <a name="syntax"></a>語法  
```  
rundll32 printui.dll PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
雖然本主題中的範例會使用先前的語法中，您也可以使用下列替代語法：  
```  
rundll32 printui.dll,PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
```  
rundll32 printui PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
```  
rundll32 printui,PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
## <a name="parameters"></a>參數  
有兩種類型的參數： 基底參數和修改參數。 基底的參數會指定所要執行此命令的函式。 只有其中一個這些參數可以出現在指定的命令列。 然後，您可以使用一或多個修改參數，如果不適用 （支援所有的基底參數不是所有的修改參數） 的基底參數來修改基底的參數。  
|基底的參數|描述|  
|----------|--------|  
|/dl|刪除本機印表機。|  
|/dn|刪除網路印表機連線。|  
|/dd|刪除印表機驅動程式。|  
|/e|顯示指定的印表機的列印喜好設定。|  
|/ga|將每個電腦的印表機連接 （連接是可供該電腦上的任何使用者登入時）。|  
|/ge|顯示每個電腦的電腦上的印表機連線。|  
|/gd|刪除每個電腦印表機連線 （連線刪除的下次使用者登入）。|  
|/ia|使用.inf 檔案，以安裝印表機驅動程式。|  
|/id|使用新增印表機驅動程式精靈 安裝印表機驅動程式。|  
|/if|使用.inf 檔案，以安裝印表機。|  
|/ii|使用.inf 檔案中的 [新增印表機精靈] 安裝印表機。|  
|/il|使用 [新增印表機精靈] 安裝印表機。|  
|/in|連接到遠端網路印表機。|  
|/ip|使用網路印表機安裝精靈 （可從 列印管理的使用者介面），以安裝印表機。|  
|/k|在印表機上列印測試頁。|  
|/o|會顯示印表機的佇列。|  
|/p|會顯示印表機的屬性。 當您使用這個參數時，您也必須指定修改參數的值 **/n [name]**。|  
|/s|顯示列印伺服器的內容。 如果您想要檢視本機列印伺服器，您不需要使用修改參數。 不過，如果您想要檢視遠端列印伺服器，您必須指定 **/c [name]** 修改參數。|  
|/Ss|指定將儲存何種印表機的資訊。 如果沒有的值 **/Ss**指定，預設行為會如同已指定 all 其中。 使用這個基底參數具有下列值放在命令列的結尾：<br /><br />-   **2**:使用儲存在印表機 s printER_INFO_2 結構中所包含的資訊。 此結構包含的印表機，例如其名稱、 伺服器名稱、 連接埠名稱和共用名稱的基本資訊。<br />-   **7**:使用儲存 printER_INFO_7 結構中包含的目錄服務資訊。<br />-   **c**:使用此選項，來儲存印表機的色彩設定檔資訊。<br />-   **d**:用來儲存印表機特定的資料，例如 s 印表機硬體識別碼。<br />-   **s**:使用此選項，來儲存印表機 's' 的安全性描述元。<br />-   **g**:使用此選項，來將資訊儲存在印表機 s 全域 devmode 抰炾。<br />-   **m**:使用此選項，來儲存印表機的最小設定。 這相當於指定**2** **d**，並**g**。<br />-   **u**:使用此選項，來將資訊儲存在每個使用者 devmode 抰炾的印表機。|  
|/Sr|指定還原印表機的相關資訊，以及如何處理設定中的衝突。 使用下列值放在命令列的結尾：<br /><br />-   **2**:使用還原印表機 s printER_INFO_2 結構中所包含的資訊。 此結構包含的印表機，例如其名稱、 伺服器名稱、 連接埠名稱和共用名稱的基本資訊。<br />-   **7**:使用還原 printER_INFO_7 結構中包含的目錄服務資訊。<br />-   **c**:使用還原印表機的色彩設定檔資訊。<br />-   **d**:使用還原印表機特定資料，例如 s 印表機硬體識別碼。<br />-   **s**:使用還原印表機 's' 的安全性描述元。<br />-   **g**:使用還原印表機 s 全域 devmode 抰炾中的資訊。<br />-   **m**:使用還原印表機的最小設定。 這相當於指定**2**， **d**，並**g**。<br />-   **u**使用還原的每位使用者 devmode 抰炾 printe s 中的資訊。<br />-   **r**： 如果印表機名稱儲存在檔案還原到印表機的名稱不同，則會使用目前的印表機名稱。 不能與指定**f**。 如果沒有**r**也不**f**指定名稱不相符，還原的設定會失敗。<br />-   **f**： 如果正在還原至印表機的名稱是儲存在檔案中的印表機名稱，然後使用印表機名稱檔案中。 不能與指定**r**。 如果沒有**f**也不**r**指定名稱不相符，還原的設定會失敗。<br />-   **p**： 如果要從還原之檔案中的連接埠名稱不相符的印表機還原到目前的連接埠名稱，要使用印表機 's' 目前的連接埠名稱。<br />-   **h**： 如果無法共用印表機還原到已儲存的設定檔中使用的資源共用名稱，然後嘗試將與目前的共用名稱或新產生的共用名稱共用印表機，如果既未**H**也**h**指定並無法共用印表機還原到與已儲存的共用名稱，然後還原會失敗。<br />-   **h**： 如果印表機還原到不能與已儲存的共用名稱共用，則不會共用印表機。 如果沒有**H**也不**h**指定並無法共用印表機還原到與已儲存的共用名稱，然後還原會失敗。<br />-   **我**： 如果已儲存的設定檔中的驅動程式不符合，正在還原印表機的驅動程式，則還原會失敗。|  
|/Xg|擷取印表機的設定。|  
|/Xs|設定印表機的設定。|  
|/y|設定安裝為預設印表機的印表機。|  
|/?|會顯示在產品的說明命令和其相關聯的參數。|  
|@[file]|指定命令列引數檔案，並直接將該檔案中的文字插入到命令列。|  
|修改參數|描述|  
|--------------|--------|  
|/a[file]|指定二進位檔名稱。|  
|/b[name]|指定基底的印表機名稱。|  
|/c[name]|如果要執行的動作是在遠端電腦上，請指定電腦名稱。|  
|/f[file]|指定通用命名慣例 (UNC) 路徑和名稱的.inf 檔案名稱或輸出檔名稱，根據您正在執行的工作。 使用 **/F [file]** 指定相依的.inf 檔案。|  
|/F[file]|指定的 UNC 路徑和名稱使用指定的.inf 檔案的.inf 檔案 **/f [file]** 而定。|  
|/h[architecture]|指定驅動程式架構。 使用下列其中之一： **x86**， **x64**，或**Itanium**。|  
|/j[provider]|指定列印的提供者名稱。|  
|/l[path]|指定您正在使用的印表機驅動程式檔案的所在位置的 UNC 路徑。|  
|/m[model]|指定驅動程式模型名稱。 （此值可以指定在.inf 檔案中）。|  
|/n[name]|指定的印表機名稱。|  
|/q|無通知的方式執行命令給使用者。|  
|/r[port]|指定的連接埠名稱。|  
|/u|指定要使用現有的印表機驅動程式，如果已安裝。|  
|/t[#]|指定的以零為起始的索引頁上啟動。|  
|/v[version]|指定驅動程式版本。 如果您還未指定的值 **/K**，您必須指定下列值之一：**輸入 2-核心模式**或是**輸入 3-使用者模式**。|  
|/w|所指定的.inf 檔案中找不到驅動程式時，提示使用者提供的驅動程式 **/f**。|  
|/Y|指定的印表機名稱應該不會自動產生。|  
|/z|指定不會自動共用安裝印表機。|  
|/K|變更參數的意義 **/h [架構]** 接受**2**取代**x86**， **3**取代**x64**，或**4**代替**Itanium**。 它也會變更參數的值 **/v [版本]** 接受**2**取代**輸入 2-核心模式**並**3** 取代**輸入 3-使用者模式**。|  
|/Z|共用的印表機，正在安裝。 只搭配 **/if**參數。|  
|/Mw[message]|向使用者顯示一則警告訊息，再認可指定命令列中的變更。|  
|/Mq[message]|向使用者顯示一則確認訊息，再認可指定命令列中的變更。|  
|/W[flags]|指定任何參數，或新增印表機精靈、 新增印表機驅動程式精靈 」，及網路印表機安裝精靈 的選項。<br /><br />**R**:可讓精靈，從最後一頁重新啟動。|  
|/G[flags]|指定通用的參數和您想要使用的選項。<br /><br />**w**:隱藏安裝驅動程式警告給使用者。|  
## <a name="remarks"></a>備註  
-   **PrintUIEntry**關鍵字是區分大小寫，和您必須輸入語法，此命令使用本主題中的範例所示的確切大小寫。  
-   請參閱[範例](#BKMK_Examples)在本文件的一些常見工作的語法。 如需其他範例，在命令提示字元中鍵入： **rundll32 printui.dll,PrintUIEntry /？**  
## <a name="BKMK_Examples"></a>範例  
若要新增新的遠端印表機，printer1，電腦中，Client1，也就是可見的使用者帳戶執行這個命令時，輸入：  
```  
rundll32 printui.dll PrintUIEntry /in /n\\client1\printer1  
```  
若要新增印表機，請使用新增印表機精靈，並使用.inf 檔，InfFile.inf，位於磁碟機 c： 在 Infpath，型別：  
```  
rundll32 printui.dll PrintUIEntry /ii /f c:\Infpath\InfFile.inf  
```  
若要刪除現有的印表機，printer1，在電腦上，Client1，輸入：  
```  
rundll32 printui.dll PrintUIEntry /dn /n\\client1\printer1  
```  
若要新增每個電腦印表機連線，printer2，所有使用者的電腦 Client2，型別 （當使用者登入時，將會套用連線）：  
```  
rundll32 printui.dll PrintUIEntry /ga /n\\client2\printer2  
```  
若要刪除每個電腦印表機連線，printer2，所有使用者的電腦 Client2，型別 （當使用者登入時，將刪除的連線）：  
```  
rundll32 printui.dll PrintUIEntry /gd /n\\client2\printer2  
```  
若要檢視的列印伺服器、 printServer1、 型別屬性：  
```  
rundll32 printui.dll PrintUIEntry /s /t1 /c\\printserver1  
```  
若要檢視印表機、 printer3、 型別屬性：  
```  
rundll32 printui.dll PrintUIEntry /p /n\\printer3  
```  
## <a name="additional-references"></a>其他參考資料  
  
-   [rundll32](rundll32.md)  
-   [列印命令參考資料](print-command-reference.md)  

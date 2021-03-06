---
title: 建立檔案到期工作
description: 本文說明針對即將到期檔案建立檔案管理工作的程序
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0ff4b46064ca780d63c6f06898c114cb180c3665
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971125"
---
# <a name="create-a-file-expiration-task"></a>建立檔案到期工作

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

下列程序將引導您針對即將到期的檔案完成建立檔案管理工作的程序。 檔案到期工作用於自動將所有符合特定條件的檔案移到指定的到期目錄，系統管理員可接著在其中備份和刪除這些檔案。

執行檔案到期工作時，會在到期目錄中建立新目錄，目錄是依執行工作的伺服器名稱分組。

新的目錄名稱會依據檔案管理工作名稱和執行時間來命名。找到已到期的檔案時，便會將該檔案移到新目錄，同時保留其原始目錄結構。

## <a name="to-create-a-file-expiration-task"></a>若要建立檔案到期工作

1. 按一下 [檔案管理工作]**** 節點。

2. 在 **\[檔案管理工作\]** 上按滑鼠右鍵，再按一下 **\[建立檔案管理工作\]** (或按一下 **\[動作\]** 窗格中的 **\[建立檔案管理工作\]**)。 如此會開啟 [建立檔案管理工作]**** 對話方塊。

3. 在 [一般]**** 索引標籤中，輸入下列資訊：

   -   **名稱**。 輸入新工作的名稱。

   -   **描述**。 輸入此工作的選擇性描述標籤。

   -   **範圍**。 使用 **\[新增\]** 按鈕以新增這項工作操作的目錄。 (選用) 您可以使用 **\[移除\]** 按鈕從清單中移除目錄。 管理工作檔案將會套用至此清單中的所有資料夾及其子資料夾。

4. 在 [動作]**** 索引標籤中，輸入下列資訊：

   - **類型**。 從下拉式清單方塊中選取 **\[檔案到期\]**。

   - [到期目錄]****。 選取檔案到期後要移到的目錄。

     > [!Warning]
     > 請勿選取工作領域內的目錄，如上一個步驟中所定義。 如此會造成導致系統不穩定以及資料遺失的反覆迴圈。

5. 選擇性地在 **\[通知\]** 索引標籤上按一下 **\[新增\]**，在工作對檔案執行動作的指定至少天數前，傳送電子郵件通知、記錄事件，或執行命令或指令碼。

   - 在 **\[執行工作以傳送通知之前的天數\]** 下拉式方塊中，輸入或選取一個值以指定在對檔案執行傳送通知動作之前的至少天數。

     > [!Note]
     > 只有在執行工作時才會傳送通知。 如果指定要傳送通知的至少天數與排定的工作不一致，將會在上一個已排程工作的那一天傳送通知。

   - 若要設定電子郵件通知，請按一下 **\[電子郵件訊息\]** 索引標籤，並輸入下列資訊：

     - 若要在已達閾值時通知系統管理員，請選取 **\[將電子郵件傳送給下列系統管理員\]** 核取方塊，並輸入要接收通知的系統管理帳戶名稱。 使用格式 <em>account@domain</em> ，並使用分號來分隔多個帳戶。

     - 若要傳送電子郵件給檔案即將到期的使用者，請選取 **\[傳送電子郵件給檔案即將到期的使用者\]** 核取方塊。

     - 若要設定訊息，請編輯提供的預設主旨列和郵件內文。 方括弧中的文字會插入關於引發通知之配額事件的變數資訊。 例如，來源檔案** \[ 擁有 \] **者變數會插入檔案即將到期的使用者名稱。 若要在文字中插入其他變數，請按一下 **\[插入變數\]**。

     - 若要附加即將到期檔案的清單，請按一下 **\[將要執行動作的檔案清單附加至電子郵件\]**，並輸入或選取 **\[清單中的檔案數目上限\]** 的值。

     - 若要設定其他標頭 (包括 [寄件者]、[副本]、[密件副本] 和 [回覆])，請按一下 **\[其他電子郵件標頭\]**。

   - 若要記錄事件，按一下 **\[事件記錄檔\]** 索引標籤並選取 **\[將警告傳送給事件記錄檔\]** 核取方塊，然後編輯預設記錄項目。

   - 若要執行命令或指令碼，按一下 **\[命令\]** 索引標籤，並選取 **\[執行這個命令或指令碼\]** 核取方塊。 然後輸入命令，或按一下 **\[瀏覽\]** 以搜尋儲存指令碼所在的位置。 您也可以輸入命令引數、選取命令或指令碼的工作目錄，或修改命令安全性設定。

6. 選擇性地使用 [報告]**** 索引標籤來產生一或多個記錄檔或存放裝置報告。

   - 若要產生記錄檔，請選取 **\[產生記錄\]** 核取方塊，然後選取一個或多個可用的記錄檔。

   - 若要產生報告，請選取 **\[產生報告\]** 核取方塊，然後選取一個或多個可用的報告格式。

   - 若要建立電子郵件產生的記錄檔或存放裝置報告，請選取 **\[將報告傳送給下列系統管理員\]** 核取方塊，然後使用 <em>account@domain</em> 格式輸入一個或多個系統管理電子郵件收件者。 使用分號隔開多個電子郵件地址。

     > [!Note]
     > 此報告會儲存在事件報告的預設位置中，您可以在 [檔案伺服器資源管理員選項]**** 對話方塊中修改預設位置。

7. 選擇性地使用 [條件]**** 索引標籤，只針對符合一組定義條件的檔案執行此工作。 可用的設定如下：

    -   **屬性條件**。 按一下 **\[新增\]** 建立以檔案分類為依據的新條件。 這樣會開啟 **\[屬性條件\]** 對話方塊，允許您選擇屬性、要對屬性執行的運算子，以及要跟屬性比較的值。 按一下 **\[確定\]** 後，即可接著建立其他條件，或是編輯或移除現有的條件。

    -   **\[自上次修改檔案以來的天數\]**。 按一下核取方塊，然後在微調方塊中輸入天數。 這樣將導致檔案管理工作只會套用至已超過指定天數未經修改的檔案。

    -   **\[自上次存取檔案以來的天數\]**。 按一下核取方塊，然後在微調方塊中輸入天數。 如果伺服器已設定為追蹤上次存取檔案之時的時間戳記，這樣將導致檔案管理工作只會套用至已超過指定天數未經存取的檔案。 如果伺服器未設定為追蹤存取的時間，這個條件就沒有效果。

    -   **\[自檔案建立以來的天數\]**。 按一下核取方塊，然後在微調方塊中輸入天數。 這將會導致工作只會套用到至少在指定天數前建立的檔案。

    -   **\[生效日期\]**。 設定此檔案管理工作要開始處理檔案的日期。 此選項很有用，可以將工作延遲直到您有機會通知使用者或預先做其他準備時。

8. 在 **\[排程\]** 索引標籤上，按一下 **\[建立排程\]**，然後按一下 **\[排程\]** 對話方塊中的 **\[新增\]**。 如此會顯示每天上午 9:00 設定的預設排程， 但您還是可以修改預設排程。 當您設定排程完畢時，按一下 **\[確定\]**，然後再按一次 **\[確定\]**。

## <a name="additional-references"></a>其他參考資料

-   [分類管理](classification-management.md)
-   [檔案管理工作](file-management-tasks.md)
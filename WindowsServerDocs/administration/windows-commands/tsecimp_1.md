---
title: tsecimp
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85fea84ed9dcb0f85bfa80e56f0c2c04d2c8e85b
ms.sourcegitcommit: 1bc3c229e9688ac741838005ec4b88e8f9533e8a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314307"
---
# <a name="tsecimp"></a>tsecimp



將可延伸標記語言 (XML) (XML) 檔案中的指派資訊匯入至 TAPI 伺服器安全性檔案 (Tsec .ini)。 您也可以使用此命令來顯示 TAPI 提供者清單, 以及與每個專案相關聯的線路裝置, 驗證 XML 檔案的結構而不匯入內容, 以及檢查網域成員資格。

## <a name="syntax"></a>語法

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/f \<Filename >|必要。 指定包含您要匯入之指派資訊的 XML 檔案名。|
|/v|驗證 XML 檔案的結構, 而不將資訊匯入 Tsec .ini 檔案。|
|u|檢查每個使用者是否為 XML 檔案中指定之網域的成員。 您使用此參數的電腦必須連線到網路。 如果您正在處理大量的使用者指派資訊, 此參數可能會大幅降低效能。|
|/d|顯示已安裝的電話語音提供者清單。 針對每個電話語音提供者, 會列出相關聯的線路裝置, 以及與每個線路裝置相關聯的位址和使用者。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   您要從中匯入指派資訊的 XML 檔案必須遵循下面所述的結構。  
    -   **UserList**元素

        **UserList**是 XML 檔案的最上層元素。
    -   **User**元素

        每個**user**元素都包含屬於網域成員之使用者的相關資訊。 每位使用者可能會被指派一或多個線路裝置。

        此外, 每個**使用者**元素可能會有一個名為**NoMerge**的屬性。 指定此屬性時, 會先移除使用者的所有目前行裝置指派, 再進行新的程式碼。 您可以使用這個屬性輕鬆地移除不想要的使用者指派。 根據預設, 不會設定這個屬性。

        **User**元素必須包含單一**DomainUserName**元素, 以指定使用者的網域和使用者名稱。 **User**元素可能也會包含一個**FriendlyName**元素, 以指定使用者的易記名稱。

        **User**元素可能包含一個**LineList**元素。 如果**LineList**元素不存在, 則會移除此使用者的所有線路裝置。
    -   **LineList**元素

        **LineList**元素包含可能指派給使用者之每一行或裝置的相關資訊。 每個**LineList**元素可以包含一個以上的**線條**元素。
    -   **Line**元素

        每個**線條**元素都會指定一行裝置。 您必須在**line**元素下加入**Address**元素或**PermanentID**元素, 以識別每個線路裝置。

        針對每個**線條**元素, 您可以設定 [**移除**] 屬性。 如果您設定此屬性, 則不會再將該線路裝置指派給該使用者。 如果未設定此屬性, 使用者會取得該線路裝置的存取權。 如果使用者無法使用線路裝置, 則不會提供任何錯誤。

## <a name="examples"></a>範例
- 下列範例 XML 程式碼區段說明如何正確使用上述定義的元素。  
  - 下列程式碼會移除指派給 User1 的所有線路裝置。  
    ```
    <UserList>
      <User NoMerge="1">
        <DomainUser>domain1\user1</DomainUser>
      </User>
    </UserList>
    ```  
  - 下列程式碼會先移除指派給 User1 的所有線路裝置, 再指派一個具有位址99999的行。 無論先前是否已指派任何線路裝置, User1 都不會指派任何其他線路裝置。  
    ```
    <UserList>
      <User NoMerge="1">
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - 下列程式碼會為 User1 新增一行裝置, 而不會刪除任何先前指派的線路裝置。  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - 下列程式碼會新增線路位址 99999, 並從 User1's access 移除行位址88888。  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
          <Line Remove="1">
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - 下列程式碼會新增永久裝置 1000, 並從 User1's 存取移除行88888。  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <PermanentID>1000</PermanentID>
          </Line>
          <Line Remove="1">
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```

-   指定 [ **/d**命令列] 選項之後, 會出現下列範例輸出, 以顯示目前的 TAPI 設定。 針對每個電話語音提供者, 會列出相關聯的線路裝置, 以及與每個線路裝置相關聯的位址和使用者。  
    ```
    NDIS Proxy TAPI Service Provider
            Line: "WAN Miniport (L2TP)"
                    Permanent ID: 12345678910

    NDIS Proxy TAPI Service Provider
            Line: "LPT1DOMAIN1\User1"
                    Permanent ID: 12345678910

    Microsoft H.323 Telephony Service Provider
            Line: "H323 Line"
                    Permanent ID: 123456
                    Addresses:
                            BLDG1-TAPI32

    ```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[命令 shell 總覽](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

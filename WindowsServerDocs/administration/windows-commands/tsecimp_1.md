---
title: tsecimp
description: '適用於 Windows 命令主題 * * *- '
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
ms.openlocfilehash: 38582706dfa5db2b5069415b81dafc533c8a89b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822099"
---
# <a name="tsecimp"></a>tsecimp



從可延伸標記語言 (XML) 檔案中會指派資訊匯入到 TAPI 伺服器安全性檔案 (Tsec.ini)。 您也可以使用此命令顯示 TAPI 提供者的清單，以及每個相關聯的線路裝置，驗證 XML 檔案的結構，但不匯入內容，並檢查網域成員資格。

## <a name="syntax"></a>語法

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/f \<Filename>|必要。 指定包含您要匯入指派資訊的 XML 檔案的名稱。|
|/v|驗證 XML 檔案的結構，而不需要將資訊匯入 Tsec.ini 檔案。|
|/u|檢查每位使用者是否為 XML 檔案中指定網域的成員。 使用這個參數的電腦必須連線到網路。 如果您正在處理大量的使用者指派資訊，此參數可能會大幅降低效能。|
|/d|會顯示一份已安裝的電話語音提供者。 每個電話語音提供者，會列出相關聯的線路裝置，以及地址和與每個線路裝置相關聯的使用者。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   您要匯入指派資訊的 XML 檔案必須遵循下述結構。  
    -   **UserList**項目

        **UserList**是 XML 檔案的最上層的項目。
    -   **使用者**項目

        每個**使用者**項目包含是網域成員的使用者資訊。 每位使用者可能會指派一或多個線路裝置。

        此外，每個**使用者**元素可能具有名為屬性**NoMerge**。 當指定這個屬性時，所有目前的行裝置工作分派，使用者會移除之前，先將新的。 您可以使用這個屬性，可輕鬆地移除不必要的使用者指派。 根據預設，未設定這個屬性。

        **使用者**項目必須包含單一**DomainUserName**元素，其指定的使用者網域和使用者名稱。 **使用者**項目可能也會包含一個**FriendlyName**元素，其指定的使用者易記名稱。

        **使用者**項目可能包含一個**LineList**項目。 如果**LineList**項目不存在，會移除此使用者的所有線路裝置。
    -   **LineList**項目

        **LineList**項目包含每個線條或可能會指派給使用者的裝置的相關資訊。 每個**LineList**項目可以包含多個**列**項目。
    -   **行**項目

        每個**列**項目會指定一個線路裝置。 您必須識別每個線路裝置，新增**地址**項目或有**PermanentID**項目底下**列**項目。

        每個**線條**項目，您可以設定**移除**屬性。 如果您設定這個屬性時，使用者不會再指派該線路裝置。 如果未設定這個屬性，則使用者會取得該線路裝置的存取。 如果線路裝置不是供使用者使用，會不提供任何錯誤。

## <a name="examples"></a>範例
-   下列範例 XML 程式碼區段說明上述定義的項目的正確用法。  
    -   下列程式碼中移除指派給 User1 的所有線路裝置。  
        ```
        <UserList>
          <User NoMerge="1">
            <DomainUser>domain1\user1</DomainUser>
          </User>
        </UserList>
        ```  
    -   下列程式碼中移除指派位址為 99999 的一行之前指派給 User1 的所有線路裝置。 User1 將有沒有指派其他線路裝置指派，而不論任何線路裝置是否先前指派。  
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
    -   下列程式碼將 user1 新增一個線路裝置，而不會刪除任何之前指派的線路裝置。  
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
    -   下列程式碼新增線路位置 99999 並移除 User1 的存取權的線路位址 88888。  
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
    -   下列程式碼會新增永久裝置 1000年並移除 User1 的存取權的線路位址 88888。  
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
-   下列範例輸出會出現後面 **/d**來顯示目前的 TAPI 設定指定命令列選項。 每個電話語音提供者，會列出相關聯的線路裝置，以及地址和與每個線路裝置相關聯的使用者。  
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

[命令殼層概觀](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)
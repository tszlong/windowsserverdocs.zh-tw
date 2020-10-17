---
title: tsecimp
description: 'Tsecimp 命令的參考文章，此命令會將可延伸標記語言 (XML)  (XML) 檔案中的指派資訊匯入至 TAPI 伺服器安全性檔案 ( # A0) 。'
ms.topic: reference
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0db7c7c191b4ca4cdd1d266892b68aa717c142da
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156666"
---
# <a name="tsecimp"></a>tsecimp

將可延伸標記語言 (XML)  (XML) 檔案中的指派資訊匯入至 TAPI 伺服器安全性檔案 ( # A0) 。 您也可以使用此命令來顯示 TAPI 提供者的清單，以及與每個裝置相關聯的線路裝置、驗證 XML 檔案的結構，而不匯入內容，以及檢查網域成員資格。

## <a name="syntax"></a>語法

```
tsecimp /f <filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /f `<filename>` | 必要。 指定需匯入其指派資訊的 XML 檔案名稱。 |
| /v | 驗證 XML 檔案結構，而不要將資訊匯入 Tsec.ini 檔案。 |
| /U | 檢查每位使用者是否是 XML 檔案中指定的網域成員。 使用此參數的電腦必須連線到網路。 如果您正在處理大量的使用者指派資訊，此參數可能會大幅降低效能。 |
| /d | 顯示已安裝的電話語音提供者清單。 對於每個電話語音提供者，會列出相關聯的線路裝置，也會列出與每個線路裝置相關聯的位址和使用者。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

您要從中匯入指派資訊的 XML 檔案必須遵循如下所述的結構：

```xml
<UserList>
  <User>
    <LineList>
      <Line>
```

- `<Userlist element>` -XML 檔案的最上層元素。

- `<User element>` -包含屬於網域成員之使用者的相關資訊。 每位使用者可以被指派一或多個線路裝置。 此外，每個 **使用者** 專案可能會有一個名為 **NoMerge**的屬性。 當指定此屬性時，會先移除使用者的所有目前線路裝置指派，然後才建立新的指派。 使用此屬性可以輕鬆地移除不想要的使用者指派。 依預設，不會設定此選項。 **User**元素必須包含單一**DomainUserName**元素，以指定使用者的網域和使用者名稱。 **使用者**專案也可能包含一個**FriendlyName**元素，為使用者指定易記的名稱。 **使用者**元素可能包含一個**LineList**元素。 如果 **LineList** 專案不存在，則會移除此使用者的所有線路裝置。

- `<LineList element>` -包含可能指派給使用者的每個線路或裝置的相關資訊。 每個 **LineList** 專案都可以包含一個以上的 **線條** 元素。

- `<Line element>` -指定線路裝置。 您必須在**line**元素底下新增**Address**元素或**PermanentID**元素，以識別每個線路裝置。 您可以針對每個 **線條** 元素設定 **移除** 屬性。 如果設定此屬性，就無法再指派該線路裝置給使用者。 如果未設定此屬性，使用者會取得該線路裝置的存取權。 如果使用者無法使用線路裝置，則不會提供錯誤。

##### <a name="sample-output-for-d-parameter"></a>/D 參數的範例輸出

此範例輸出會在執行 **/d** 參數之後顯示，以顯示目前的 TAPI 設定。 對於每個電話語音提供者，會列出相關聯的線路裝置，也會列出與每個線路裝置相關聯的位址和使用者。

```
NDIS Proxy TAPI Service Provider
  Line: WAN Miniport (L2TP)
    Permanent ID: 12345678910

NDIS Proxy TAPI Service Provider
  Line: LPT1DOMAIN1\User1
    Permanent ID: 12345678910

Microsoft H.323 Telephony Service Provider
  Line: H323 Line
    Permanent ID: 123456
    Addresses:
      BLDG1-TAPI32
```

## <a name="examples"></a>範例

若要移除指派給 *User1*的所有線路裝置，請輸入：

```xml
<UserList>
  <User NoMerge=1>
    <DomainUser>domain1\user1</DomainUser>
  </User>
</UserList>
```

若要移除指派給 *User1*的所有線路裝置，在使用位址 *99999*指派一行之前，請輸入：

```xml
<UserList>
  <User NoMerge=1>
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

在此範例中，不論先前是否已指派任何線路裝置， *User1* 都沒有指派任何其他線路裝置。

若要為 *User1*新增一行裝置，而不刪除任何先前指派的線路裝置，請輸入：

```xml
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

若要新增行位址*99999* ，並從*User1's*存取移除行位址*88888* ，請輸入：

```xml
<UserList>
  <User>
  <DomainUser>domain1\user1</DomainUser>
  <FriendlyName>User1</FriendlyName>
  <LineList>
    <Line>
      <Address>99999</Address>
    </Line>
    <Line Remove=1>
      <Address>88888</Address>
    </Line>
  </LineList>
  </User>
</UserList>
```

若要新增永久裝置*1000*並從*User1's*存取移除行*88888* ，請輸入：

```xml
<UserList>
  <User>
  <DomainUser>domain1\user1</DomainUser>
  <FriendlyName>User1</FriendlyName>
  <LineList>
    <Line>
    <PermanentID>1000</PermanentID>
    </Line>
    <Line Remove=1>
    <Address>88888</Address>
    </Line>
  </LineList>
  </User>
</UserList>
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [命令 shell 總覽](/previous-versions/windows/it-pro/windows-server-2003/cc737438(v=ws.10))

---
src: /API Documentation/Request API/Store/AmazonBuyGoodsRequest.md
---

# AmazonBuyGoodsRequest


Processes the receipt from an Amazon in app purchase.

The GameSparks platform will validate the amazonUserId and receiptId with Amazon using the Amazon Purchase Secret configured against the game.

The receiptId in the data will be recorded and the request will be rejected if the receiptId has previously been processed, this prevents replay attacks.

Once verfied, the players account will be credited with the Virtual Good, or Virtual Currency the purchase contains. The virtual good will be looked up by matching the productId in the receipt with the 'Amazon Product Id' configured against the virtual good.


<a href="https://api.gamesparks.net/#amazonbuygoodsrequest" target="_gsapi">View interactive version here</a>

## Request Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
amazonUserId | Yes | string | The userId obtained from the UserData within a PurchaseResponse
receiptId | Yes | string | The receiptId obtained from the Receipt within a PurchaseResponse
uniqueTransactionByPlayer | No | boolean | If set to true, the transactionId from this receipt will not be globally valdidated, this will mean replays between players are possible.

## Response Parameters


A response containing details of the bought items

Parameter | Type | Description
--------- | ---- | -----------
boughtItems | [Boughtitem[]](#boughtitem) | A JSON object containing details of the bought items
currency1Added | number | How much currency type 1 was added
currency2Added | number | How much currency type 2 was added
currency3Added | number | How much currency type 3 was added
currency4Added | number | How much currency type 4 was added
currency5Added | number | How much currency type 5 was added
currency6Added | number | How much currency type 6 was added
currencyConsumed | number | For a buy with currency request, how much currency was used
currencyType | number | For a buy with currency request, which currency type was used
invalidItems | string[] | A list of invalid items for this purchase (if any). This field is populated only for store buys
scriptData | ScriptData | A JSON Map of any data added either to the Request or the Response by your Cloud Code
transactionIds | string[] | The list of transactionIds, for this purchase, if they exist. This field is populated only for store buys

## Nested types

### Boughtitem

A nested object that represents a bought item.

Parameter | Type | Description
--------- | ---- | -----------
quantity | number | The quantity of the bought item
shortCode | string | The short code of the bought item

### ScriptData

A collection of arbitrary data that can be added to a message via a Cloud Code script.

Parameter | Type | Description
--------- | ---- | -----------
myKey | string | An arbitrary data key
myValue | JSON | An arbitrary data value.

## Error Codes

Key | Value | Description
--------- | ----------- | -----------
receiptId | REQUIRED | The receiptId is missing
amazonUserId | REQUIRED | The amazonUserId is missing
verificationError | 1 | No matching virtual good can be found
verificationError | 2 | The receiptId is not valid for the given userId and secret
verificationError | 3 | There was an error connecting to the Amazon service
verificationError | 4 | The Amazon purchase secret is not configured against the game
verificationError | 5 | The receiptId has previously been processed

## Code Samples

<h3>C#</h3>
```cs
	using GameSparks.Api;
	using GameSparks.Api.Requests;
	using GameSparks.Api.Responses;
	...
	new AmazonBuyGoodsRequest()
		.SetAmazonUserId(amazonUserId)
		.SetReceiptId(receiptId)
		.SetUniqueTransactionByPlayer(uniqueTransactionByPlayer)
		.Send((response) => {
		GSEnumerable<var> boughtItems = response.BoughtItems; 
		long? currency1Added = response.Currency1Added; 
		long? currency2Added = response.Currency2Added; 
		long? currency3Added = response.Currency3Added; 
		long? currency4Added = response.Currency4Added; 
		long? currency5Added = response.Currency5Added; 
		long? currency6Added = response.Currency6Added; 
		long? currencyConsumed = response.CurrencyConsumed; 
		int? currencyType = response.CurrencyType; 
		IList<string> invalidItems = response.InvalidItems; 
		GSData scriptData = response.ScriptData; 
		IList<string> transactionIds = response.TransactionIds; 
		});

```

### ActionScript 3
```actionscript
	import com.gamesparks.*;
	import com.gamesparks.api.requests.*;
	import com.gamesparks.api.responses.*;
	import com.gamesparks.api.types.*;
	...
	
	gs.getRequestBuilder()
	    .createAmazonBuyGoodsRequest()
		.setAmazonUserId(amazonUserId)
		.setReceiptId(receiptId)
		.setUniqueTransactionByPlayer(uniqueTransactionByPlayer)
		.send(function(response:com.gamesparks.api.responses.BuyVirtualGoodResponse):void {
		var boughtItems:Vector.<Boughtitem> = response.getBoughtItems(); 
		var currency1Added:Number = response.getCurrency1Added(); 
		var currency2Added:Number = response.getCurrency2Added(); 
		var currency3Added:Number = response.getCurrency3Added(); 
		var currency4Added:Number = response.getCurrency4Added(); 
		var currency5Added:Number = response.getCurrency5Added(); 
		var currency6Added:Number = response.getCurrency6Added(); 
		var currencyConsumed:Number = response.getCurrencyConsumed(); 
		var currencyType:Number = response.getCurrencyType(); 
		var invalidItems:Vector.<String> = response.getInvalidItems(); 
		var scriptData:ScriptData = response.getScriptData(); 
		var transactionIds:Vector.<String> = response.getTransactionIds(); 
		});

```

### Objective-C
```objectivec
	#import "GS.h"
	#import "GSAPI.h"
	...
	GSAmazonBuyGoodsRequest* request = [[GSAmazonBuyGoodsRequest alloc] init];
	[request setAmazonUserId:amazonUserId;
	[request setReceiptId:receiptId;
	[request setUniqueTransactionByPlayer:uniqueTransactionByPlayer;
	[request setCallback:^ (GSBuyVirtualGoodResponse* response) {
	NSArray* boughtItems = [response getBoughtItems]; 
	NSNumber* currency1Added = [response getCurrency1Added]; 
	NSNumber* currency2Added = [response getCurrency2Added]; 
	NSNumber* currency3Added = [response getCurrency3Added]; 
	NSNumber* currency4Added = [response getCurrency4Added]; 
	NSNumber* currency5Added = [response getCurrency5Added]; 
	NSNumber* currency6Added = [response getCurrency6Added]; 
	NSNumber* currencyConsumed = [response getCurrencyConsumed]; 
	NSNumber* currencyType = [response getCurrencyType]; 
	NSArray* invalidItems = [response getInvalidItems]; 
	NSDictionary* scriptData = [response getScriptData]; 
	NSArray* transactionIds = [response getTransactionIds]; 
	}];
	[gs send:request];

```

### C++
```cpp

	#include <GameSparks/generated/GSRequests.h>
	using namespace GameSparks::Core;
	using namespace GameSparks::Api::Responses;
	using namespace GameSparks::Api::Requests;
	...
	
	void AmazonBuyGoodsRequest_Response(GS& gsInstance, const BuyVirtualGoodResponse& response) {
	gsstl:vector<Types::Boughtitem*> boughtItems = response.getBoughtItems(); 
	Optional::t_LongOptional currency1Added = response.getCurrency1Added(); 
	Optional::t_LongOptional currency2Added = response.getCurrency2Added(); 
	Optional::t_LongOptional currency3Added = response.getCurrency3Added(); 
	Optional::t_LongOptional currency4Added = response.getCurrency4Added(); 
	Optional::t_LongOptional currency5Added = response.getCurrency5Added(); 
	Optional::t_LongOptional currency6Added = response.getCurrency6Added(); 
	Optional::t_LongOptional currencyConsumed = response.getCurrencyConsumed(); 
	Optional::t_LongOptional currencyType = response.getCurrencyType(); 
	gsstl:vector<gsstl::string> invalidItems = response.getInvalidItems(); 
	GSData scriptData = response.getScriptData(); 
	gsstl:vector<gsstl::string> transactionIds = response.getTransactionIds(); 
	}
	......
	
	AmazonBuyGoodsRequest request(gsInstance);
	request.SetAmazonUserId(amazonUserId)
	request.SetReceiptId(receiptId)
	request.SetUniqueTransactionByPlayer(uniqueTransactionByPlayer)
	request.Send(AmazonBuyGoodsRequest_Response);
```

### Java
```java
import com.gamesparks.sdk.api.autogen.GSRequestBuilder.AmazonBuyGoodsRequest;
import com.gamesparks.sdk.api.autogen.GSResponseBuilder.BuyVirtualGoodResponse;
import com.gamesparks.sdk.api.autogen.GSTypes.*;
import com.gamesparks.sdk.api.GSEventListener;

...
gs.getRequestBuilder().createAmazonBuyGoodsRequest()
	.setAmazonUserId(amazonUserId)
	.setReceiptId(receiptId)
	.setUniqueTransactionByPlayer(uniqueTransactionByPlayer)
	.send(new GSEventListener<BuyVirtualGoodResponse>() {
		@Override
		public void onEvent(BuyVirtualGoodResponse response) {
			List<Boughtitem> boughtItems = response.getBoughtItems(); 
			Long currency1Added = response.getCurrency1Added(); 
			Long currency2Added = response.getCurrency2Added(); 
			Long currency3Added = response.getCurrency3Added(); 
			Long currency4Added = response.getCurrency4Added(); 
			Long currency5Added = response.getCurrency5Added(); 
			Long currency6Added = response.getCurrency6Added(); 
			Long currencyConsumed = response.getCurrencyConsumed(); 
			Integer currencyType = response.getCurrencyType(); 
			List<String> invalidItems = response.getInvalidItems(); 
			GSData scriptData = response.getScriptData(); 
			List<String> transactionIds = response.getTransactionIds(); 
		}
	});

```

### Cloud Code
```javascript

	var request = new SparkRequests.AmazonBuyGoodsRequest();
	request.amazonUserId = ...;
	request.receiptId = ...;
	request.uniqueTransactionByPlayer = ...;
	var response = request.Send();
	
var boughtItems = response.boughtItems; 
var currency1Added = response.currency1Added; 
var currency2Added = response.currency2Added; 
var currency3Added = response.currency3Added; 
var currency4Added = response.currency4Added; 
var currency5Added = response.currency5Added; 
var currency6Added = response.currency6Added; 
var currencyConsumed = response.currencyConsumed; 
var currencyType = response.currencyType; 
var invalidItems = response.invalidItems; 
var scriptData = response.scriptData; 
var transactionIds = response.transactionIds; 
```



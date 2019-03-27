<!--
 # license: Copyright © 2011-2016 MOLPay Sdn Bhd. All Rights Reserved. 
 -->

<img src="https://user-images.githubusercontent.com/38641542/39353138-654385dc-4a39-11e8-9710-19e5f03ec62e.jpg">

# rms_mobile_xdk_flutter_beta

This is the beta but functional Razer Merchant Services Flutter payment module that is ready to be implemented into any Flutter project. An example application project is provided for MOLPayXDK framework integration reference.

## Recommended configurations

- Minimum Android SDK Version: 27 ++

- Minimum Android API level: 19 ++

- Minimum Android target version: Android 4.4

- Minimum Flutter version : 0.2.8 ++

- Xcode version: 9 ++

- Minimum target version: iOS 8

## Installation

1) Add `molpay_mobile_xdk_flutter_beta` in pubspec.yaml

```
dependencies:
  molpay_mobile_xdk_flutter_beta: "<put latest release version here>"

```

2) flutter packages get

3) Add the following import to dart code of your application

```dart
    import 'package:molpay_mobile_xdk_flutter_beta/molpay_mobile_xdk_flutter_beta.dart';
```



Add the following keys to _Info.plist_ file for iOS:

1) 'App Transport Security Settings > Allow Arbitrary Loads > YES' 

2) 'NSPhotoLibraryUsageDescription' > 'Payment images' 

3) 'NSPhotoLibraryAddUsageDescription' > 'Payment images' 


## Sample Result

```
=========================================
Sample transaction result in JSON string:
=========================================

{"status_code":"11","amount":"1.01","chksum":"34a9ec11a5b79f31a15176ffbcac76cd","pInstruction":0,"msgType":"C6","paydate":1459240430,"order_id":"3q3rux7dj","err_desc":"","channel":"Credit","app_code":"439187","txn_ID":"6936766"}

Parameter and meaning:

"status_code" - "00" for Success, "11" for Failed, "22" for *Pending. 
(*Pending status only applicable to cash channels only)
"amount" - The transaction amount
"paydate" - The transaction date
"order_id" - The transaction order id
"channel" - The transaction channel description
"txn_ID" - The transaction id generated by MOLPay

* Notes: You may ignore other parameters and values not stated above

=====================================
* Sample error result in JSON string:
=====================================

{"Error":"Communication Error"}

Parameter and meaning:

"Communication Error" - Error starting a payment process due to several possible reasons, please contact MOLPay support should the error persists.
1) Internet not available
2) API credentials (username, password, merchant id, verify key)
3) MOLPay server offline.
```

## Prepare the Payment detail object

```
var paymentDetails = {
    // Optional, REQUIRED when use online Sandbox environment and account credentials.
    'mp_dev_mode': false,

    // Mandatory String. Values obtained from MOLPay.
    'mp_username' : 'username',
    'mp_password' : 'password',
    'mp_merchant_ID' : 'merchantid',
    'mp_app_name' : 'appname',
    'mp_verification_key' : 'vkey123', 

    // Mandatory String. Payment values.
    'mp_amount' : '1.10',, // Minimum 1.01
    'mp_order_ID' : 'orderid123', 
    'mp_currency' : 'MYR',
    'mp_country' : 'MY',  
    
    // Optional String.
    'mp_channel' : '', // Use 'multi' for all available channels option. For individual channel seletion, please refer to https://github.com/MOLPay/molpay-mobile-xdk-examples/blob/master/channel_list.tsv.
    'mp_bill_description' : '',
    'mp_bill_name' : '',
    'mp_bill_email' : '',
    'mp_bill_mobile' : '',
    'mp_channel_editing' : false, // Option to allow channel selection.
    'mp_editing_enabled' : false, // Option to allow billing information editing.
    
    // Optional, but required payment values. User input will be required when values not passed.
    'mp_channel' : 'multi', // Use 'multi' for all available channels option. For individual channel seletion, please refer to https://github.com/MOLPay/molpay-mobile-xdk-examples/blob/master/channel_list.tsv.
    'mp_bill_description' : 'billdesc',
    'mp_bill_name' : 'billname',
    'mp_bill_email' : 'email@domain.com',
    'mp_bill_mobile' : '+1234567',

    // Optional, allow channel selection. 
    'mp_channel_editing' : false,

    // Optional, allow billing information editing.
    'mp_editing_enabled' : false,

    // Optional, for Escrow.
    'mp_is_escrow': '0', // Put "1" to enable escrow

    // Optional, for credit card BIN restrictions and campaigns.
    'mp_bin_lock' : ['414170', '414171'],   

    // Optional, for mp_bin_lock alert error.
    'mp_bin_lock_err_msg': 'Only UOB allowed',
    
    // WARNING! FOR TRANSACTION QUERY USE ONLY, DO NOT USE THIS ON PAYMENT PROCESS.
    // Optional, provide a valid cash channel transaction id here will display a payment instruction screen. Required if mp_request_type is 'Receipt'.
    'mp_transaction_id': '',
    // Optional, use 'Receipt' for Cash channels, and 'Status' for transaction status query.
    'mp_request_type': '',

    // Optional, use this to customize the UI theme for the payment info screen, the original XDK custom.css file can be obtained at https://github.com/MOLPay/molpay-mobile-xdk-examples/blob/master/custom.css.
    'mp_custom_css_url': '',

    // Optional, set the token id to nominate a preferred token as the default selection, set "new" to allow new card only.
    'mp_preferred_token': '',

    // Optional, credit card transaction type, set "AUTH" to authorize the transaction.
    'mp_tcctype': '',

    // Optional, required valid credit card channel, set true to process this transaction through the recurring api, please refer the MOLPay Recurring API pdf. 
    'mp_is_recurring': false,

    // Optional, show nominated channels.
    'mp_allowed_channels': ['credit', 'credit3'],

    // Optional, simulate offline payment, set boolean value to enable. 
    'mp_sandbox_mode': true,

    // Optional, required a valid mp_channel value, this will skip the payment info page and go direct to the payment screen.
    'mp_express_mode': true,

    // Optional, extended email format validation based on W3C standards.
    'mp_advanced_email_validation_enabled': true,

    // Optional, extended phone format validation based on Google i18n standards.
    'mp_advanced_phone_validation_enabled': true,

    // Optional, explicitly force disable user input.
    'mp_bill_name_edit_disabled': true,
    'mp_bill_email_edit_disabled': true,
    'mp_bill_mobile_edit_disabled': true,
    'mp_bill_description_edit_disabled': true,

    // Optional, EN, MS, VI, TH, FIL, MY, KM, ID, ZH.
    'mp_language': 'EN',

    // Optional, Cash channel payment request expiration duration in hour.
    'mp_cash_waittime': 48,
    
    // Optional, allow bypass of 3DS on some credit card channels.
    'mp_non_3DS': true,

    // Optional, disable card list option.
    'mp_card_list_disabled': true,

    // Optional for channels restriction, this option has less priority than mp_allowed_channels.
    'mp_disabled_channels': ['credit']  
};

```

## Start the payment module

```
//import molpay package
import 'package:molpay_mobile_xdk_flutter_beta/molpay_mobile_xdk_flutter_beta.dart';

//start molpay payment
String result = await molpay.startMolpay(paymentDetails);
print("Result" + result);

```
## Cash channel payment process (How does it work?)

    This is how the cash channels work on XDK:
    
    1) The user initiate a cash payment, upon completed, the XDK will pause at the “Payment instruction” screen, the results would return a pending status.
    
    2) The user can then click on “Close” to exit the MOLPay XDK aka the payment screen.
    
    3) When later in time, the user would arrive at say 7-Eleven to make the payment, the host app then can call the XDK again to display the “Payment Instruction” again, then it has to pass in all the payment details like it will for the standard payment process, only this time, the host app will have to also pass in an extra value in the payment details, it’s the “mp_transaction_id”, the value has to be the same transaction returned in the results from the XDK earlier during the completion of the transaction. If the transaction id provided is accurate, the XDK will instead show the “Payment Instruction” in place of the standard payment screen.
    
    4) After the user done the paying at the 7-Eleven counter, they can close and exit MOLPay XDK by clicking the “Close” button again.

## XDK built-in checksum validator caveats 

    All XDK come with a built-in checksum validator to validate all incoming checksums and return the validation result through the "mp_secured_verified" parameter. However, this mechanism will fail and always return false if merchants are implementing the private secret key (which the latter is highly recommended and prefereable.) If you would choose to implement the private secret key, you may ignore the "mp_secured_verified" and send the checksum back to your server for validation. 

## Private Secret Key checksum validation formula

    chksum = MD5(mp_merchant_ID + results.msgType + results.txn_ID + results.amount + results.status_code + merchant_private_secret_key)

## Support

Submit issue to this repository or email to our support@molpay.com

Merchant Technical Support / Customer Care : support@molpay.com <br>
Sales/Reseller Enquiry : sales@molpay.com <br>
Marketing Campaign : marketing@molpay.com <br>
Channel/Partner Enquiry : channel@molpay.com <br>
Media Contact : media@molpay.com <br>
R&D and Tech-related Suggestion : technical@molpay.com <br>
Abuse Reporting : abuse@molpay.com

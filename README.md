# Octus SDK Cordova Plugin
![plugin version](https://img.shields.io/badge/plugin_version-v1.0.4-blue)

Octus SDK uses advanced deep learning technologies for accurate and fast ID scanning and OCR.

# Table Of Content

- [Integration steps for Android](#integration-steps-for-android)
- [Integration steps for IOS](#integration-steps-for-ios)
- [Help](#help)

## Integration steps for Android

#### Step 1 : Adding plugin to the app

- Extract the plugin ZIP file. Plugin ZIP file is provided by FRSLABS.
- Update the maven credentials in `/path/to/octus/plugin/src/android/OCTUS.gradle` file. Maven credentials is provided by FRSLABS. `NOTE: Replace /path/to/octus/plugin with the path where you extracted the plugin zip file`
- Add the plugin to your app
    `cordova plugin add /path/to/octus/plugin`
    
#### Step 2 : Integrate Octus SDK

- Code to initialise octus sdk with required input parameters

```Javascript

  // Handle the success result here
  var successCallBack = function success(result) {
      console.info(result);
  }
  
  // Handle the failure result here
  var failureCallBack = function error(result) {
      console.info(result);
  }
  
  var scanSDKParams = {
   "licence_key" : "USE_YOUR_LICENCE_KEY",
   "language" : 1,//ENGLISH -1
   "show_instruction_flag" : true, // YES or NO
   "orientation_flat" : false, // YES or NO
   "data_points" : false, // YES or NO
   "set_alert_type" : 1, // YES or NO
   "set_scan_mode" : 1, // YES or NO
   "id_country" : "INDIA",
   "aadhar_masked" : true,// If aadhar masked required true otherwise false
   "id_type" : "E_MANDATE_CAT1", //Document type ADR,PAN,VID,NID,PPT,DRV etc
   "id_sub_type" : 1,//OCR-1,QR_CODE-2,MRZ-3,PDF417-4.
   "document_sides" : "FRONT_BACK"// Document sides for PAN-FRONT ,ADR-FRONT_BACK  If two sides FRONT_BACK one side FRONT
  }
  
  // This line starts the SDK
  OCTUS.invokeOctusSDK(scanSDKParams, successCallBack, failureCallBack);
```
  
- Sample Android success response


```Javascript

  {
     "GSTN":"",
     "aadhaarMaskStatus":"N,N",
     "aadhaarNumberValidation":"",
     "address1":"",
     "address2":"",
     "address3":"",
     "address4":"",
     "backIdScanStatus":"Fail",
     "bankAccountNumber":"",
     "bankIfsCode":"",
     "barcodeValue":"",
     "chequeAccountId":"",
     "chequeNumber":"",
     "city":"",
     "code":"OCR",
     "code1":"",
     "code2":"",
     "confidenceIndexB":"",
     "confidenceIndexF":"",
     "country":"",
     "dataPointAll":"false",
     "dateOfBirth":"",
     "documentCountry":"IN",
     "documentNumber1":"",
     "documentNumber2":"",
     "documentSide":"FRONT_BACK",
     "documentSubType":"OCR",
     "documentType":"E_MANDATE_CAT1",
     "emailHash":"",
     "errorCode":"",
     "expiryDate":"",
     "face":"",
     "frameLog":"",
     "frontIdScanStatus":"Fail",
     "gender":"",
     "issueDate":"",
     "issuingAuthority":"",
     "issuingCountry":"",
     "licenceType":"",
     "micrCode":"",
     "mobileHash":"",
     "mobileNumber":"",
     "mrzChecksumValidityStatus":"",
     "name1":"",
     "name2":"",
     "parent1":"",
     "parent2":"",
     "photo1":"/data/user/0/com.frslabs.octuscordova.sampleapp/files/octus/Image-7216.jpg",
     "photo2":"/data/user/0/com.frslabs.octuscordova.sampleapp/files/octus/Image-7978.jpg",
     "postCode":"",
     "signatureValidation":"",
     "specialCode1":"",
     "specialCode2":"",
     "spouse":"",
     "state":"",
     "yearOfBirth":""
  }

```

- Sample Android failure response

```Javascript
  {
      "error": "400"
  }

/*  801	Scan timed out
    802	Invalid ID parameters passed
    803	Camera permission denied
    804	Scan was interrupted
    805	Octus SDK License got expired
    806	Octus SDK License was invalid
    807	Invalid camera resolution
    811	QR not detected
    812	QR parsing failed
    108	Internet Unavailable
    401	Api Limit Exceeded
    429	Too many request   */
  ```
    
## Integration steps for IOS

#### Step 1 : Adding plugin and dependencies to the app

- Extract the plugin ZIP file. Plugin ZIP file is provided by FRSLABS.
- Add the plugin to your app `cordova plugin add /path/to/octus/plugin`. `NOTE: Replace /path/to/octus/plugin with the path where you extracted the plugin zip file`
- Add the swift support to cordova app :  `cordova plugin add cordova-plugin-add-swift-support --save`
- Add camera permissions to your app to capture picture
    `cordova plugin add cordova-plugin-ios-camera-permissions --variable CAMERA_USAGE_DESCRIPTION="To capture document"`
    
#### Step 2 : Save/Edit Netrc settings to install custom pod

You will need a valid netrc credentials to install octus from maven, which can be obtained by contacting `support@frslabs.com`. 

1. Create or edit .netrc file under current user's home directory
2. Write the below lines into that file, replace <YOUR_USERNAME> and <YOUR_PASSWORD> with your credentials which is shared through email and save the file.
```ruby
machine octus-ios.repo.frslabs.space
login <YOUR_USERNAME>
password <YOUR_PASSOWRD>
```
3. In terminal enter below command to install the pod.
    
    
- After adding the plugin you need to install the sdk through pod 
    1: first check with this command to see if all the details are added correctly
                `open podfile` 
    2: It should display the following details:

        `# DO NOT MODIFY -- auto-generated by Apache Cordova
        source 'https://gitlab.com/frslabs-public/ios/octus.git'
        source 'https://github.com/CocoaPods/Specs.git'
        platform :ios, '13.0'  //If changes are needed to platform ios version, you can change it here
        use_frameworks!
        target '<Your Target Name>' do
            pod 'Octus', '~> 1.6.0' //Add the version if it is missed in podfile
            pod 'TesseractOCRiOS'
        end`
    
    3: Use this command to install :  `pod install`
    4: Then build the project to add the plugin  :  `cordova build ios`
         
#### Step 3 : Integrate Octus SDK
 
- Passing the input parameters from index.js to native code

   ```Javascript
   
      var inputParamsDict = {};
      inputParamsDict['ios_licence_key'] = 'your licence key';
      inputParamsDict['ios_documentCountry'] = 'IN';
      inputParamsDict['ios_documentType'] = 'PASSPORT'; //'PAN CARD', 'AADHAAR CARD', 'PASSPORT', 'CHECK LEAF', 'DRIVING LICENCE', 'GST NUMBER', 'MASKED AADHAAR', 'SCAN IMAGE H', 'SCAN IMAGE V', 'NATIONAL ID CARD', 'VISA','VOTER ID CARD','E-MANDATE'.
      inputParamsDict['ios_documentSubType'] = 'MRTD'; //'OCR', 'MRTD', 'BARCODE', 'CROP'
      inputParamsDict['ios_documentSide'] = '1'; // '1' or '2'
      
      
      // Handle the success result here
      var successCallBack = function success(result){
          console.info(result);
      }
      
      // Handle the failure result here
      var failureCallBack = function error(result){
          console.info(result);
      }
  
      // This line starts the SDK
      OCTUS.invokeOctusSDK(inputParamsDict,successCallback,errorCallback);
      
    ```
    Octus Parameters(DESCRIPTION) : 
     
     1:     `inputParamsDict['ios_licence_key'] = 'your licence key';`
     
       Accepts the Octus licence key as a String
            
     2:     `inputParamsDict['ios_documentCountry'] = 'IN';`
              
        Sets the country associated with the Document.
        For the complete list of supported countries refer ISO_3166-1_alpha-2 format code

     3:     `inputParamsDict['ios_documentType'] = 'PASSPORT';`

        Sets the Document which has to be scanned. Possible values are:
        Value           
        'PAN CARD'
        'AADHAAR CARD'
        'VOTER ID CARD'
        'NATIONAL ID CARD'
        'PASSPORT'
        'VISA'
        'DRIVING LICENCE'
        'CHECK LEAF'
        'GST NUMBER'
        'MASKED AADHAAR'
        'SCAN IMAGE H'
        'SCAN IMAGE V'
        'E-MANDATE'
        
     4:      `inputParamsDict['ios_documentSubType'] = 'MRTD';`

         Sets the Document Sub Type. Majority of the documents support only OCR as a sub type.

         Documents where both OCR and BARCODE apply are : ADR
         Documents where only MRTD apply are : PPT
                                               VSA
         Possible values for Sub Type are:
             Value    
             OCR        Scans the document in OCR mode
             BARCODE    Scans the document in QR mode
             MRTD       Scans the document in MRZ mode
             CROP       Scans the document in Crop mode
             Note: OCR is only supported from iOS version 13 and above. 

    5:      `inputParamsDict['ios_documentSide'] = '1';`
    
        Set the document side '1' or '2' according to documentType AND ScanMode.

        Value             ScanMode        DocumentSide
        'PAN CARD'          OCR                1
        'AADHAAR CARD'      OCR, BARCODE      2, 1
        'VOTER ID CARD'     OCR                2
        'PASSPORT'          MRTD              1, 2
        'DRIVING LICENCE'   OCR                2
        'CHECK LEAF'        OCR                1
        'GST NUMBER'        OCR                1
        'MASKED AADHAAR'    CROP               2
        'SCAN IMAGE H'      CROP               2
        'SCAN IMAGE V'      CROP               2
        'E-MANDATE'.        OCR                1
         Note: In case of MRTD, DocumentSide = 2 is only applicable for Indian passports with address on the back page.

  ```
  
- Sample IOS Success reponse    
Example repsonse of passport
    ```Javascript
        {
          "name2" : "NAME 2",
          "frontImagePath" : "file:///var/mobile/Containers/Data/Application/D65000DF-70D4-438C-826A-1E77747C9885/Documents/OCTUS_FILE_FRONT.png",
          "documentType" : "P",
          "issuedBy" : "IND",
          "dob" : "dd/mm/yyyy",
          "facePath" : "file:///var/mobile/Containers/Data/Application/D65000DF-70D4-438C-826A-1E77747C9885/Documents/OCTUS_FILE_FACE.png",
          "croppedImagePath": "file:///var/mobile/Containers/Data/Application/D65000DF-70D4-438C-826A-1E77747C9885/Documents/OCTUS_FILE_CROP.png",
          "code" : "mrtd",
          "name1" : "NAME 1",
          "allCheckDigitsValid" : "true",
          "number" : "XXXXXXXX",
          "personalNumber" : "",
          "expiry" : "dd/mm/yyyy",
          "country" : "IND",
          "gender" : "GENDER"
        }
    ```

- Sample IOS failure response
    ```Javascript
        801    Scan timed out
        802    Invalid ID parameters passed
        803    Camera permission denied
        804    Scan interrupted
        805    License expired
        806    License invalid
        807    Invalid camera resolution
        811    QR not detected
        812    QR parsing failed
        108    Internet unavailable
        401    API limit exceeded
        429    Too many requests
    ```

## Help
For any queries/feedback , contact us at `support@frslabs.com` 



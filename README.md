## Octus - Cordova Plugin

Octus SDK uses advanced deep learning technologies for accurate and fast ID scanning and OCR.

#### 1. Adding plugin to the app

- Extract the plugin ZIP file `octus.zip`
- Update the maven credentials in `/path/to/octus/src/android/OCTUS.gradle` file
- Add the plugin to your app
    `cordova plugin add /path/to/octus`
#### For IOS 
- Add the swift support to cordova app :  `cordova plugin add cordova-plugin-add-swift-support --save`
- Add camera permissions to your app to capture picture
    `cordova plugin add cordova-plugin-ios-camera-permissions --variable CAMERA_USAGE_DESCRIPTION="To capture document"`
- After adding the plugin you need to install the sdk through pod 
    1: first check with this command to see if all the details are added correctly
                `open podfile` 
    2: It should display the following details:

        `# DO NOT MODIFY -- auto-generated by Apache Cordova
        source 'https://gitlab.com/frslabs-public/ios/octus.git'
        source 'https://github.com/CocoaPods/Specs.git'
        platform :ios, '13.0'  //If changes are needed to platform ios version, you can change it here
        use_frameworks!
        target 'octusApp' do
            project 'cordovaoctus.xcodeproj'
            pod 'Octus', '~> 1.0.0'
            pod 'TesseractOCRiOS'
        end`
    
    3: Use this command to install :  `pod install`
    4: Then build the project to add the plugin  :  `cordova build ios`


#### 2. Android Usage

- Initialize parameters

  ```Javascript
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
                 "id_type" : "ADR", //Document type ADR,PAN,VID,NID,PPT,DRV etc
                 "id_sub_type" : 1,//OCR-1,QR_CODE-2,MRZ-3,PDF417-4.
                 "document_sides" : "FRONT_BACK"// Document sides for PAN-FRONT ,ADR-FRONT_BACK  If two sides FRONT_BACK one side FRONT
                }
                
 #### IOS Usage
 
- Passing the input parameters from index.js to native code
    ```Javascript
       var inputParamsDict = {};
       inputParamsDict['LICENCE_KEY'] = 'your licence key';
       inputParamsDict['documentCountry'] = 'IN';
       inputParamsDict['documentType'] = 'PASSPORT'; //'PAN CARD', 'AADHAAR CARD', 'PASSPORT', 'CHECK LEAF', 'DRIVING LICENCE', 'GST NUMBER', 'MASKED AADHAAR', 'SCAN IMAGE H', 'SCAN IMAGE V', 'NATIONAL ID CARD', 'VISA','VOTER ID CARD'.
       inputParamsDict['documentSubType'] = 'MRTD'; //'OCR', 'MRTD', 'BARCODE', 'CROP'
       inputParamsDict['documentSide'] = '1'; // '1' or '2'
    ```
    Octus Parameters(DESCRIPTION) : 
     1:     `inputParamsDict['LICENCE_KEY'] = 'your licence key';`
     
            Accepts the Octus licence key as a String
            
     2:     `inputParamsDict['documentCountry'] = 'IN';`
              
            Sets the country associated with the Document.
            For the complete list of supported countries refer ISO_3166-1_alpha-2 format code

     3:     `inputParamsDict['documentType'] = 'PASSPORT';`

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
        
     4:      `inputParamsDict['documentSubType'] = 'MRTD';`

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

    5:      `inputParamsDict['documentSide'] = '1';`
    
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
             Note: In case of MRTD, DocumentSide = 2 is only applicable for Indian passports with address on the back page.

  ```
- Create callback methods

  ```Javascript
    var successCallBack = function success(result){
                          console.info(result);
                    }
  ```

    ```Javascript
   var failureCallBack = function error(result){
                         console.info(result);
           }
    ```

- Invoke the ANDROID SDK

  ```Javascript
   Cordova.exec(successCallBack, failureCallBack, "OCTUS", "invokeOctusSDK", [scanSDKParams]);
  ```
- Invoke the IOS SDK

    ```Javascript
        OCTUS.invokeOctusSDK(inputParamsDict,successCallback,errorCallback);
     ```
- Sample Android success response

  ```Javascript
    {
    	code = 'OCR',
    	  code1 = '',
    	  code2 = '',
    	  documentType = 'PAN',
    	  documentNumber1 = 'PAN NUMBER',
    	  documentNumber2 = '',
    	  name1 = 'ABC',
    	  name2 = 'DEF',
    	  parent1 = '',
    	  parent2 = '',
    	  spouse = '',
    	  gender = '',
    	  address1 = '',
    	  address2 = '',
    	  address3 = '',
    	  address4 = '',
    	  city = '',
    	  state = '',
    	  country = 'IN',
    	  postCode = '',
    	  mobileNumber = '',
    	  dateOfBirth = '08/06/1992',
    	  yearOfBirth = '',
    	  issueDate = '',
    	  expiryDate = '',
    	  issuingCountry = '',
    	  face = '/data/user/0/com.frslabs.cordova.first/files/scanId/Image-6909.jpg',
    	  photo1 = '/data/user/0/com.frslabs.cordova.first/files/scanId/Image-9606.jpg',
    	  photo2 = '',
    	  bankAccountNumber = '',
    	  bankIfsCode = '',
    	  GSTN = '',
    	  specialCode1 = '',
    	  specialCode2 = '',
    	  errorCode = '',
    	  licenceType = '',
    	  issuingAuthority = '',
    	  frontIdScanStatus = 'Success',
    	  backIdScanStatus = '',
    	  aadhaarNumberValidation = '',
    	  confidenceIndexF = '',
    	  confidenceIndexB = '',
    	  signatureValidation = '',
    	  mobileHash = '',
    	  emailHash = '',
    	  frameLog = ''
    }
  ```

- Sample Android failure response

  ```Javascript
    {
        "error": "400"
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
        429	Too many request */
    }
    
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





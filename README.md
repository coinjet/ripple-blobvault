[![Build Status](https://travis-ci.org/xdv/divvy-blobvault.svg?branch=master)](https://travis-ci.org/xdv/divvy-blobvault)

You can read the documentation at [apiary](http://docs.blobvaultv1.apiary.io/)


# Blobvault v1
Welcome to the Blobvault v1 API. This API provides access to
the Divvy Blobvault.

# Blobvault Links
+ [This: Raw API Blueprint](https://raw.githubusercontent.com/rook2pawn/ripple-blobvault/master/docs/v1-apiary-api-reference.md)
+ [Blobvault GitHub Repository](https://github.com/ripple/ripple-blobvault)

There are three types of signing, HMAC-RECOV, HMAC and ECDSA

# Group ECDSA-RECOV signing

This signature is specifically for account recovery. Applicable for requesting the encrypted blob and the encrypted_blobdecrypt_key

## GET /your/path{?signature,signature_date,signature_type,signature_username}

+ Parameters

    + signature_username (required, string,`<ripple username`>)
    + signature_date (required, string, `2014-05-19T17:21:34.000Z`) ... the date string
    + signature_type (required, string, `RIPPLE1-ECDSA-SHA512`)
    + signature (required, string, `AAAAHC2k5dWAnmC7zXr2N0ZEDbiF0kNQxHgf1xS-bixub5pmQFo-SMcDe0U-n6jkGaFrKfOn7NAlraTogHISkdzyUDY`) ... the computed signature based on the public key signed by the secret. 

+ Response 200


# Group ECDSA Signing

example:'https://id.staging.ripple.com/v1/user?signature=AAAAHC2k5dWAnmC7zXr2N0ZEDbiF0kNQxHgf1xS-bixub5pmQFo-SMcDe0U-n6jkGaFrKfOn7NAlraTogHISkdzyUDY&signature_date=2014-05-19T17:21:34.000Z&signature_blob_id=92da7f762e240b40c23c1f15c55cee6f488c8cc8adfea079d5e738b5b152ab2b&signature_account=rfaPezp7ShpR2c89J3pZehnXHmSKd85MCY&signature_type=RIPPLE1-ECDSA-SHA512'

In this example we just use GET but it applies to any HTTP Method (GET, POST) that is marked as using ECDSA

## GET /your/path{?signature_account,signature_type,signature,signature_date,signature_blob_id}

+ Parameters

    + signature_account   (required, string,`rwUNHL9AdSupre4tGb7NXZpRS1ift5sR7W`) ... the public key of the user
    + signature (required, string, `AAAAHC2k5dWAnmC7zXr2N0ZEDbiF0kNQxHgf1xS-bixub5pmQFo-SMcDe0U-n6jkGaFrKfOn7NAlraTogHISkdzyUDY`) ... the computed signature
    + signature_date (required, string, `2014-05-19T17:21:34.000Z`) ... the date string
    + signature_blob_id (required, string,`003bb8cfbe753657d312de52bb9863ace009e649712157d6a71fdb14a6ff249c`) ... the hexadecimal id
    + signature_type (required, string, `RIPPLE1-ECDSA-SHA512`) ... for ECDSA use this exact string
    
+ Response 200

# Group HMAC Signing

In this example we use GET but it can apply to any HTTP Method for any route marked using HMAC

## GET /your/path{?signature,signature_blob_id,signature_date}


+ Parameters

    + signature (required, string) ... the HMAC signature
    + signature_blob_id (required, string) ... the blob id
    + signature_date (required, string) ... the date string
    
+ Response 200


# Group Creating a user - uses ECDSA

## POST /v1/user

+ Request (application/json)

        {
            "address": "",
            "encrypted_blobdecrypt_key": "",
            "blob_id" : "",
            "username" : "",
            "auth_secret": "",
            "data" : "",
            "email" : "",
            "hostlink" : "",
            "encrypted_secret" : "",
            "origin" : "<origin field e.g. Ripple Trade>"
        }
+ Response 201 (application/json)

         {
         "result" : "success"
         }
         
+ Response 400 (application/json)

        { 
        "result" : "error",
        "message" : ""
        }

# Group Deleting a user - uses ECDSA

example: DELETE /v1/user/foo will delete user foo

## DELETE /v1/user/{username}

+ Parameters 

    + username (required, string) ... the username to delete

+ Response 200 (application/json)

        {
        result: "success"
        }

+ Response 400 (application/json)

        { 
        "result" : "error",
        }
        

# Group Looking up a user - by username

## GET /v1/user/{username}

+ Parameters

    + username (required, string) ... the username to lookup

    
+ Response 200 (application/json)

        {
        username : "<username>",
        version : 3,
        address : "<ripple address>",
        exists : true (or false),
        "version":3,
        "blobvault":"http://<blobvault url>",
        "pakdf":
            {
             "host":"auth1.ripple.com",
             "url":"https://auth1.ripple.com/api/sign",
            "exponent":"010001",
            "alpha":"7283d19e784f48a96062271a4fa6e2c3addf14e6edf78a4bb61364856d580f13552008d7b9e3b60ebd9555e9f6c7778ec69f976757d206134e54d61ba9d588a7e37a77cf48060522478352d76db000366ef669a1b1ca93c5e3e05bc344afa1e8ccb15d3343da94180dccf590c2c32408c3f3f176c8885e95d988f1565ee9b80c12f72503ab49917792f907bbb9037487b0afed967fefc9ab090164597fcd391c43fab33029b38e66ff4af96cbf6d90a01b891f856ddd3d94e9c9b307fe01e1353a8c30edd5a94a0ebba5fe7161569000ad3b0d3568872d52b6fbdfce987a687e4b346ea702e8986b03b6b1b85536c813e46052a31ed64ec490d3ba38029544aa",
            "modulus":"c7f1bc1dfb1be82d244aef01228c1409c198894eca9e21430f1669b4aa3864c9f37f3d51b2b4ba1ab9e80f59d267fda1521e88b05117993175e004543c6e3611242f24432ce8efa3b81f0ff660b4f91c5d52f2511a6f38181a7bf9abeef72db056508bbb4eeb5f65f161dd2d5b439655d2ae7081fcc62fdcb281520911d96700c85cdaf12e7d1f15b55ade867240722425198d4ce39019550c4c8a921fc231d3e94297688c2d77cd68ee8fdeda38b7f9a274701fef23b4eaa6c1a9c15b2d77f37634930386fc20ec291be95aed9956801e1c76601b09c413ad915ff03bfdc0b6b233686ae59e8caf11750b509ab4e57ee09202239baee3d6e392d1640185e1cd"
            }
        }
        

# Group Looking up a user - by address

## GET /v1/user/{address}

+ Parameters

    + address (required, string) ... the address of user to lookup

+ Response 200 (application/json)

        {
        username : "<username>",
        version : 3,
        address : "<ripple address>",
        exists : true,
        "version":3,
        "blobvault":"http://<blobvault url>",
        "pakdf":
            {
             "host":"auth1.ripple.com",
             "url":"https://auth1.ripple.com/api/sign",
            "exponent":"010001",
            "alpha":"7283d19e784f48a96062271a4fa6e2c3addf14e6edf78a4bb61364856d580f13552008d7b9e3b60ebd9555e9f6c7778ec69f976757d206134e54d61ba9d588a7e37a77cf48060522478352d76db000366ef669a1b1ca93c5e3e05bc344afa1e8ccb15d3343da94180dccf590c2c32408c3f3f176c8885e95d988f1565ee9b80c12f72503ab49917792f907bbb9037487b0afed967fefc9ab090164597fcd391c43fab33029b38e66ff4af96cbf6d90a01b891f856ddd3d94e9c9b307fe01e1353a8c30edd5a94a0ebba5fe7161569000ad3b0d3568872d52b6fbdfce987a687e4b346ea702e8986b03b6b1b85536c813e46052a31ed64ec490d3ba38029544aa",
            "modulus":"c7f1bc1dfb1be82d244aef01228c1409c198894eca9e21430f1669b4aa3864c9f37f3d51b2b4ba1ab9e80f59d267fda1521e88b05117993175e004543c6e3611242f24432ce8efa3b81f0ff660b4f91c5d52f2511a6f38181a7bf9abeef72db056508bbb4eeb5f65f161dd2d5b439655d2ae7081fcc62fdcb281520911d96700c85cdaf12e7d1f15b55ade867240722425198d4ce39019550c4c8a921fc231d3e94297688c2d77cd68ee8fdeda38b7f9a274701fef23b4eaa6c1a9c15b2d77f37634930386fc20ec291be95aed9956801e1c76601b09c413ad915ff03bfdc0b6b233686ae59e8caf11750b509ab4e57ee09202239baee3d6e392d1640185e1cd"
            }
        }



# Group Looking up a user via querstring

Example: /v1/authinfo?username=foo

## GET /v1/authinfo{?username}

+ Response 200 (application/json)

        {
        username : "<username>",
        version : 3,
        address : "<ripple address>",
        exists : true,
        "version":3,
        "blobvault":"http://<blobvault url>",
        "pakdf":
            {
             "host":"auth1.ripple.com",
             "url":"https://auth1.ripple.com/api/sign",
            "exponent":"010001",
            "alpha":"7283d19e784f48a96062271a4fa6e2c3addf14e6edf78a4bb61364856d580f13552008d7b9e3b60ebd9555e9f6c7778ec69f976757d206134e54d61ba9d588a7e37a77cf48060522478352d76db000366ef669a1b1ca93c5e3e05bc344afa1e8ccb15d3343da94180dccf590c2c32408c3f3f176c8885e95d988f1565ee9b80c12f72503ab49917792f907bbb9037487b0afed967fefc9ab090164597fcd391c43fab33029b38e66ff4af96cbf6d90a01b891f856ddd3d94e9c9b307fe01e1353a8c30edd5a94a0ebba5fe7161569000ad3b0d3568872d52b6fbdfce987a687e4b346ea702e8986b03b6b1b85536c813e46052a31ed64ec490d3ba38029544aa",
            "modulus":"c7f1bc1dfb1be82d244aef01228c1409c198894eca9e21430f1669b4aa3864c9f37f3d51b2b4ba1ab9e80f59d267fda1521e88b05117993175e004543c6e3611242f24432ce8efa3b81f0ff660b4f91c5d52f2511a6f38181a7bf9abeef72db056508bbb4eeb5f65f161dd2d5b439655d2ae7081fcc62fdcb281520911d96700c85cdaf12e7d1f15b55ade867240722425198d4ce39019550c4c8a921fc231d3e94297688c2d77cd68ee8fdeda38b7f9a274701fef23b4eaa6c1a9c15b2d77f37634930386fc20ec291be95aed9956801e1c76601b09c413ad915ff03bfdc0b6b233686ae59e8caf11750b509ab4e57ee09202239baee3d6e392d1640185e1cd"
            }
        }
        

# Group 2FA - Setting Two Factor Authentication 2FA Settings - uses ECDSA

## POST /v1/blob/{blob_id}/2FA

+ Parameters 

    + blob_id (required,string) ... the blob id

+ Request (application/json)

        {
            "remember_me" : true,
            "enabled" : true,
            "phone" : "<phone number>",
            "country_code" : "<country_code>",
            "via" : "sms"
        }

+ Response 200 (application/json)

        { 
            "result" : "success",
            "message" : "<message>"
        }
        
# Group 2FA - Getting Two Factor Authentication 2FA Settings - uses ECDSA

## GET /v1/blob/{blob_id}/2FA{?device_id}

Ex. /v1/blob/<blobid>/2FA?device_id=57261be82601452ada0f2795bc709734

+ Parameters

    + blob_id (required,string) ... the blob id
    + device_id   (optional, string,`57261be82601452ada0f2795bc709734`) ... 256 bit random number in hex
    
+ Response 200 (application/json)

        { 
            "result" : "success",
            "device_id" : "<device_id>", (if device id is supplied)
            "is_auth" : true, (if device id is supplied)
            "remember_me" : true,  
            "enabled" : true,
            "country_code": "<country_code",
            "masked_phone" : "<masked phone number>",
            "via" : "sms"
        }

# Group 2FA - Request 2FA Token

This is not a signed request but requires the blob id. No device ID required.

## GET /v1/blob/{blob_id}/2fa/requestToken

+ Response 200 (application/json)

        {
            "result" : "success",
            "message" : "<message>"
        }

# Group 2FA - Verify Device Token

This is not a signed request but requires the blob id. Client supplies device ID.

## POST /v1/blob/{blob_id}/2fa/verifyToken

+ Request (application/json)
        
        {
            "device_id" : "<device_id>",
            "token" : "<token>"
        }

+ Response 200 (application/json)

        {
            "result" : "success",
            "message" : "<message>"
        }


        
# Group Recovering a user blob - uses ECDSA-RECOV

## GET /v1/user/recov/{username}

+ Parameters

    + username (required, string) ... the username to lookup

    
+ Response 200 (application/json)

        {
            `result` : `success`,
            `blob` : `<blob data base64>`,
            `encrypted_secret` : `<encrypted secret>`,
            `revision` : `<revision number>`,
            `patches` : `<list of patches>`,
            `blob_id` : `<blob_id>`,
            `encrypted_blobdecrypt_key` : `<encrypted_blobdecrypt_key>`,
            `missing_user_fields` : `<missing user fields>`
        }
        
+ Response 400 (application/json)
    
        { 
            result : "error" 
        }

# Group Setting a user blob and fields - uses ECDSA

This is meant to be used for either resetting the blob after client blob recovery, or
this can be used password change, where all the listed fields in the request must be changed.

Since this endpoint changes the blob_id, please note to use the old blob_id for the ECDSA signature.

## POST /v1/user/{username}/updatekeys

+ Parameters 

    +  username (required, string) ... represents the old username we want to change

+ Request (application/json)
 
        {
            `blob_id`: `<new blob id>`,
            `data` : `<data base64>`,
            `revision`: `<last known revision>`,
            `encrypted_secret` : `<encrypted_secret>`,
            `encrypted_blobdecrypt_key` : `<encrypted_blobdecrypt_key>`
        }


+ Response 200 (application/json)

        {   
            result : 'success'
        }


+ Response 400 (application/json)
    
        { 
            result : "error" 
        }


        
# Group Renaming a user - uses ECDSA

Since this endpoint changes the blob_id, please note to use the old blob_id for the ECDSA signature.


## POST /v1/user/{username}/rename

+ Parameters 

    +  username (required, string) ... represents the old username we want to change

+ Request (application/json)

        {
            `blob_id`: `<new blob id>`,
            `username`: `<new username>`,
            `data` : `<data base64>`,
            `revision`: `<last known revision>`,
            `encrypted_secret` : `<encrypted_secret>`
        }


+ Response 200 (application/json)

        {   
            result : 'success'
        }


+ Response 400 (application/json)
    
        { 
            result : "error" 
        }

# Group Changing email address - uses ECDSA

## POST /v1/user/email

+ Request (application/json)

        {
            email : "<email address>",
            blob_id : "<blob id>",
            username : "<username>",
            hostlink : "<hostlink>"
        }

+ Response 200 (application/json)

        {
            result : "success"
        }

+ Response 400 (application/json)
    
        { 
            result : "error" 
        }

# Group Resending Email - uses ECDSA

## POST /v1/user/email/resend

+ Request (application/json)

        {
            email : "<email address>",
            username : "<username>",
            hostlink : "<hostlink>"
        }

+ Response 200 (application/json)

        {
            result : "success"
        }

+ Response 400 (application/json)
    
        { 
            result : "error" 
        }

# Group Verifying Email Token

## GET /v1/user/{username}/verify/{token}

+ Parameters

    + username ... string
    + token ... string
    
+ Response 200 (application/json)

        { 
            "result" : "success"
        }
        
# Group Requesting Phone Token

## POST /v1/user/{username}/phone

+ Parameters

    + username ... string
    
+ Request (application/json)

        {
            via:'sms',
            phone_number:"<phone number>",
            country_code:"<country code>"
        }

+ Response 200 (application/json)

        { 
            "result" : "success"
        }

+ Response 400 (application/json)

        { 
            "result" : "error"
        }
            
# Group Validating Phone Token

## POST /v1/user/{username}/phone/validate

+ Parameters

    + username ... string
    
+ Request (application/json)

        {
            phone_number:"<phone number>",
            country_code:"<country code>",
            token : "<token>"
        }

+ Response 200 (application/json)

        { 
            "result" : "success"
        }

+ Response 400 (application/json)

        { 
            "result" : "error"
        }
            

# Group Adding Profile Detail - uses HMAC

Choose any number of the following to add

## POST /v1/user/{username}/profile

+ Request (application/json)

        {
            "phone" : "<phone number>",
            "country" : "<country>",
            "region" : "<region>",
            "city" : "<city>"
        }
        
+ Response 200 (application/json)

        { 
            "result" : "success"
        }

        
# Group Retrieve a blob

## GET /v1/blob/{blob_id}{?device_id}

+ Parameters

    + blob_id (required,string) ... the blob id
    + device_id (optional, string) ... client produced id for two-factor auth (2fa)

+ Response 200 (application/json)

        {   
            result: 'success',
            encrypted_secret : '<base 64 encrypted secret>',
            blob : '<base 64 blob data>',
            revision : '<revision number>',
            email: '<email address>',
            quota: '<disk usage>',
            patches: '<array of patches>',
            twofactor: {
                enabled: <boolean>,
                is_auth: <boolean>,
                remember_me: <boolean>,
                masked_phone: "***-***-5555",
                via:"sms"
            } (only if device_id has attempted auth)
        }

+ Response 400 (application/json)
    
        { 
            result : "error" 
        }



# Group Add a blob patch - uses HMAC

## POST /v1/blob/patch

+ Request (application/json)

        {
          blob_id : "<blob id>"
          patch: "<blob patch>"
        }

+ Response 201 (application/json)

        {
          result: "success",
          revision: "<revision number>"
        }
        
+ Response 400 (application/json)
    
        { 
            result : "error" 
        }
        

# Group Get a blob Patch

## GET /v1/blob/{blob_id}/patch/{patch_id}


+ Parameters

    + blob_id ... string
    + patch_id ... string
    
+ Response 200 (application/json)

        { 
            result : "success",
            patch : "<patch>"
        }
        
+ Response 400 (application/json)
    
        { 
            result : "error" 
        }


# Group Consolidate a set of blob patches - uses HMAC

## POST /v1/blob/consolidate

+ Request (application/json)

        { data : "<data>",
          revision : "<revision number>",
          blob_id : "<blob_id>"
        }


+ Response 200 (application/json)

        { 
            "result" : "success"
        }
        
+ Response 400 (application/json)
    
        { 
            result : "error" 
        }


# Group Service Health

## GET /health

+ Response 200 (application/json)

        {
            status : 'ok'
        }
    
+ Response 500 (application/json)

        {
            status : 'not ok'
            
        }

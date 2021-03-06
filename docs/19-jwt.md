# jwt.go

Create base64-encoded header and claim set.

## Building and running the program
    
The source file, token.go, is in this directory:

    $GOPATH/github.com/steveperry-53/examples-go/19-jwt
    
Navigate to anywhere, for example $HOME, and enter this command;

    go install github.com/steveperry-53/examples-go/19-jwt
    
The executable file, 19-jwt, is in this directory:

    $GOPATH/bin
    
To run the executable, enter this command:

    $GOPATH/bin/19-jwt

## Discussion

The output of this program is

<base64-encoded header>.<base-64 encoded claim set>

Sign the output.
Sign the UTF-8 representation of the input using SHA256withRSA
(also known as RSASSA-PKCS1-V1_5-SIGN with the SHA-256 hash function)

My best guess for using openssl:
openssl dgst -sha256 -sign prikey.pem -out sign.sha256 <file>

Base64 encode the signature.
openssl base64 -in sign.sha256 -out sig-64

Then create this JWT:
<base64-encoded header>.<base-64 encoded claim set>.<base64-encoded signature>

Pass the JWT to the auth server.

POST /token HTTP/1.1
Host: oauth2.googleapis.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer&assertion=<jwt>

OR

curl -d 'grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer&assertion=<jwt>
' https://oauth2.googleapis.com/token

Problem:
{
  "error": "invalid_grant",
  "error_description": "Invalid JWT Signature."
}

I get this error even though I verified the signature locally.




# httpsign

Duplicated from [gin-contrib / httpsign](https://github.com/gin-contrib/httpsign)

Signing HTTP Messages Middleware base on [HTTP Signatures](https://tools.ietf.org/html/draft-cavage-http-signatures).

## Example

``` go

package main

import (
	"github.com/stremovskyy/httpsign"
	"github.com/stremovskyy/httpsign/crypto"
	"github.com/gin-gonic/gin"
)

func main() {
	// Define algorithm
	hmacsha256 := &crypto.HmacSha256{}
	hmacsha512 := &crypto.HmacSha512{}
	// Init define secret params
	readKeyID := httpsign.KeyID("read")
	writeKeyID := httpsign.KeyID("write")
	secrets := httpsign.Secrets{
		readKeyID: &httpsign.Secret{
			Key:       "HMACSHA256-SecretKey",
			Algorithm: hmacsha256, // You could using other algo with interface Crypto
		},
		writeKeyID: &httpsign.Secret{
			Key:       "HMACSHA512-SecretKey",
			Algorithm: hmacsha512,
		},
	}

	// Init server
	r := gin.Default()

	//Create middleware with default rule. Could modify by parse Option func
	auth := httpsign.NewAuthenticator(secrets)

	r.Use(auth.Authenticated())
	r.GET("/a", a)
	r.POST("/b", b)
	r.POST("/c", c)

	r.Run(":8080")
}

```

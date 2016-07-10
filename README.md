# go-fcm : FCM Library for Go

[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg?style=flat-square)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=MYW4MY786JXFN&lc=GB&item_name=go%2dfcm%20development&item_number=go%2dfcm&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donate_SM%2egif%3aNonHosted)
[![AUR](https://img.shields.io/aur/license/yaourt.svg?style=flat-square)](https://github.com/NaySoftware/go-fcm/blob/master/LICENSE)

Firebase Cloud Messaging ( FCM ) Library using golang ( Go )

This library uses HTTP/JSON Firebase Cloud Messaging connection server protocol


###### Features

* Send messages to a topic
* Send messages to a device list
* Message can be a notification or data payload
* Supports condition attribute (fcm only)
* Instace Id Features
	- Get info about app Instance
	- Subscribe app Instance to a topic
	- Batch Subscribe/Unsubscribe to/from a topic
	- Create registration tokens for APNs tokens



## Usage

```
go get github.com/NaySoftware/go-fcm

```

### Notes

###### Server Key

serverKey is the server key by Firebase Cloud Messaging

Server Key can be found in:

1. Firebase project settings
2. Cloud Messaging
3. then copy the server key

###### Retry mechanism

Retry should be implemented based on the requirements.
Sending a request will result with a "FcmResponseStatus" struct, which holds
a detailed information based on the Firebase Response, with RetryAfter
(response header) if available - with a failed request.
its recommended to use a backoff time to retry the request - (if RetryAfter
	header is not available).



# Examples

### Send to A topic

```golang

package main

import (
	"fmt"
    "github.com/NaySoftware/go-fcm"
)

const (
	 serverKey = "YOUR-KEY"
     topic = "/topics/someTopic"
)

func main() {

	data := map[string]string{
		"msg": "Hello World1",
		"sum": "Happy Day",
	}

	c := fcm.NewFcmClient(serverKey)
	c.NewFcmMsgTo(topic, data)


	status, err := c.Send()


	if err == nil {
    status.PrintResults()
	} else {
		fmt.Println(err)
	}

}


```


### Send to a list of Devices (tokens)

```golang

package main

import (
	"fmt"
    "github.com/NaySoftware/go-fcm"
)

const (
	 serverKey = "YOUR-KEY"
)

func main() {

	data := map[string]string{
		"msg": "Hello World1",
		"sum": "Happy Day",
	}

  ids := []string{
      "token1",
  }


  xds := []string{
      "token5",
      "token6",
      "token7",
  }

	c := fcm.NewFcmClient(serverKey)
    c.NewFcmRegIdsMsg(ids, data)
    c.AppendDevices(xds)

	status, err := c.Send()


	if err == nil {
    status.PrintResults()
	} else {
		fmt.Println(err)
	}

}



```

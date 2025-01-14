<a href="https://www.mailersend.com"><img src="https://www.mailersend.com/images/logo.svg" width="200px"/></a>

MailerSend Golang SDK

[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE.md)

# Table of Contents
* [Installation](#installation)
* [Usage](#usage)
* [Testing](#testing)
* [Support and Feedback](#support-and-feedback)
* [License](#license)

<a name="installation"></a>
# Installation
We recomend using this package with golang [modules](https://github.com/golang/go/wiki/Modules)

```
$ go get github.com/mailersend/mailersend-go
```

<a name="usage"></a>
# Usage

Sending a basic email.

``` go
package main

import (
    "context"
    "fmt"
    "time"

    "github.com/mailersend/mailersend-go"
)

var APIKey string = "Api Key Here"

func main() {
	// Create an instance of the mailersend client
	ms := mailersend.NewMailersend(APIKey)

	ctx := context.Background()
	ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
	defer cancel()

	subject := "Subject"
	text := "This is the text content"
	html := "<p>This is the HTML content</p>"

	from := mailersend.From{
		Name:  "Your Name",
		Email: "your@domain.com",
	}

	recipients := []mailersend.Recipient{
		{
			Name:  "Your Client",
			Email: "your@client.com",
		},
	}

	variables := []mailersend.Variables{
		{
			Email: "your@client.com",
			Substitutions: []mailersend.Substitution{
				{
					Var:   "foo",
					Value: "bar",
				},
			},
		},
	}

	tags := []string{"foo", "bar"}

	message := ms.NewMessage()

	message.SetFrom(from)
	message.SetRecipients(recipients)
	message.SetSubject(subject)
	message.SetHTML(html)
	message.SetText(text)
	//message.SetTemplateID("testtemplateid")
	message.SetSubstitutions(variables)

	message.SetTags(tags)

	res, _ := ms.Send(ctx, message)

	fmt.Printf(res.Header.Get("X-Message-Id"))

}

```

<a name="testing"></a>

# Testing

[pkg/testing](https://golang.org/pkg/testing/)

```
$ go test
```

<a name="endpoints"></a>
# Available endpoints

| Feature group | Endpoint    | Available |
| ------------- | ----------- | --------- |
| Email         | `POST send` | ✅         |

*If, at the moment, some endpoint is not available, please use other available tools to access it. [Refer to official API docs for more info](https://developers.mailersend.com/).*


<a name="support-and-feedback"></a>
# Support and Feedback

In case you find any bugs, submit an issue directly here in GitHub.

You are welcome to create SDK for any other programming language.

If you have any troubles using our API or SDK free to contact our support by email [info@mailersend.com](mailto:info@mailersend.com)

The official documentation is at [https://developers.mailersend.com](https://developers.mailersend.com)

<a name="license"></a>
# License

[The MIT License (MIT)](LICENSE)

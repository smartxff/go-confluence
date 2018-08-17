go-confluence
=============

Go library wrapping the [confluence REST API](https://docs.atlassian.com/confluence/REST/latest/).

Currently implemented
---------------------

- Authentication:
	- Basic Authentication
	- Token Key authentication
- Manipulating existing wiki content, i.e:
    - `CreateContent`
	- `DeleteContent`
	- `GetContent`
	- `UpdateContent`

This is everything I needed for my project. I might add some more functionality when I find the time. Feel free to send pull requests.
example
```
package main

import (
	wiki "github.com/smartxff/go-confluence"
	"fmt"
	"encoding/json"
)


func main() {
	auth := wiki.BasicAuth("username","passwd")
	wk,err := wiki.NewWiki("http://wiki.example.com",auth)
	if err !=nil{
		panic(err)
	}
	content := wiki.CreateContentRequest{
		Id:"parentId",
		Title:"title",
		Type:"page",
		Space: struct{ Key string `json:"key"` }{Key: "spaceKey"},
		Status: "current",
		Ancestors: []struct{ Id string `json:"id"` }{{Id: "parentId"}},
		Body: struct {
			View struct {
				Value          string `json:"value"`;
				Representation string `json:"representation"`
			}  `json:"view"`
		}{View: struct {
			Value          string `json:"value"`
			Representation string `json:"representation"`
		}   {Value: "ContentBody", Representation: "view"}},

	}


	resp,err := wk.CreateContent(content)
	if err !=nil{
		fmt.Println(err)
	}
	respContent := &wiki.Content{}
	if err := json.Unmarshal(resp,respContent);err !=nil{
		fmt.Println(err)
	}
	fmt.Println(respContent.Id)
}
```
# html/template
Dynamically load sub templates with {{template .MyVariable}}. Forked from Go's built in library "html/template" version 1.9.2.

[![Build Status](https://travis-ci.org/speedyhoon/html.svg?branch=master)](https://travis-ci.org/speedyhoon/html)
[![go report card](https://goreportcard.com/badge/github.com/speedyhoon/html)](https://goreportcard.com/report/github.com/speedyhoon/html)

## Example
```go
package main

import (
	"log"
	"os"
	"github.com/speedyhoon/html/template"
)

func main() {
	const tpl = `
<!doctype html>
<html>
	<head>
		<title>{{.Title}}</title>
	</head>
	<body>
		<p>Template name: {{.SubTemplate}}
		{{template "foo" .}}
		{{template .SubTemplate .}}
	</body>
</html>
{{define "foo"}}<div>Template data: {{.Data}}</div>{{end}}`

	data := struct {
		Title string
		Items []string
		SubTemplate string
		Data string
	}{
		Title: "My page",
		SubTemplate: "foo",
		Data: "Four, Five, Six.",
	}

	err := template.Must(template.New("page").Parse(tpl)).Execute(os.Stdout, data)
	if err != nil {
		log.Fatal(err)
	}
}
```

## Output
```html
<!doctype html>
<html>
        <head>
                <title>My page</title>
        </head>
        <body>
                <p>Template name: foo
                <div>Template data: Four, Five, Six.</div>
                <div>Template data: Four, Five, Six.</div>
        </body>
</html>
```
# gin-csrf

[![CircleCI](https://circleci.com/gh/vuongggggg/gin-csrf/tree/master.svg?style=svg)](https://circleci.com/gh/vuongggggg/gin-csrf/tree/master) [![GoDoc](https://godoc.org/github.com/vuongggggg/gin-csrf?status.svg)](https://godoc.org/github.com/vuongggggg/gin-csrf)

CSRF protection middleware for [Gin]. This middleware has to be used with [gin-contrib/sessions](https://github.com/gin-contrib/sessions).

Original credit to [tommy351](https://github.com/tommy351/gin-csrf).
Original credit to [utrack](https://github.com/utrack/gin-csrf).

## Installation

``` bash
$ go get github.com/vuongggggg/gin-csrf
```

## Overview
- Forked from **utrack/gin-csrf**
- What's newer than origin:

	- One token per request

## Usage

``` go
package main

import (
	"github.com/gin-contrib/sessions"
	"github.com/gin-contrib/sessions/cookie"
	"github.com/gin-gonic/gin"
	"github.com/utrack/gin-csrf"
)

func main() {
	r := gin.Default()
	store := cookie.NewStore([]byte("secret"))
	r.Use(sessions.Sessions("mysession", store))
	r.Use(csrf.Middleware(csrf.Options{
		Secret: "secret123",
		ErrorFunc: func(c *gin.Context) {
			c.String(400, "CSRF token mismatch")
			c.Abort()
		},
	}))

	r.GET("/protected", func(c *gin.Context) {
		c.String(200, csrf.GetToken(c))
	})

	r.POST("/protected", func(c *gin.Context) {
		c.String(200, "CSRF token is valid")
	})

	r.Run(":8080")
}

```

[Gin]: http://gin-gonic.github.io/gin/
[gin-sessions]: https://github.com/utrack/gin-sessions

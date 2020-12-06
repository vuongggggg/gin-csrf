# gin-csrf
[![Git Actions](https://github.com/vuongggggg/gin-csrf/workflows/Go/badge.svg)](https://github.com/vuongggggg/gin-csrf/workflows/Go/badge.svg)
[![Build Status](https://travis-ci.org/vuongggggg/gin-csrf.svg?branch=master)](https://travis-ci.org/vuongggggg/gin-csrf) [![CircleCI](https://circleci.com/gh/vuongggggg/gin-csrf/tree/master.svg?style=svg)](https://circleci.com/gh/vuongggggg/gin-csrf/tree/master) [![GoDoc](https://godoc.org/github.com/vuongggggg/gin-csrf?status.svg)](https://godoc.org/github.com/vuongggggg/gin-csrf)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fvuongggggg%2Fgin-csrf.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fvuongggggg%2Fgin-csrf?ref=badge_shield)

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
	"github.com/vuongggggg/gin-csrf"
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


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fvuongggggg%2Fgin-csrf.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fvuongggggg%2Fgin-csrf?ref=badge_large)
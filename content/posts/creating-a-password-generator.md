---
layout: post
title:  "[Golang] Creating a Password/Secret Generator"
date:   2024-03-02
categories: ["golang"]
draft: false
---

in order to practice me working on Golang, I decided to start writing some small scripts that would I would probably be using every once in a while. One of them is a Password/Secret Generator. The code will be shown below.

# How it works?

The [project](https://github.com/ebourgess/password-generator) is just a 73 lines of code function that’s run in the CLI with some flags being passed to that command. With the flags being:

- `length` and that would have the length of the string that we want to generate. by default it’s 12.
- `upper` indicating that the string should have Uppercase letters, this is enabled by default
- `lower` indicating that the string should have lowercase letters, this is enabled by default
- `digits` indicating that the string should have numbers, this isn’t enabled by default and should be passed as a flag `--digits` to the command when running
- `special` indicating that the string should have special characters, like `digits` this isn’t enabled by default and should be passed as a flag `--special` to the command when running.

In order to get the most out of the command, we simply need to:

- Build the `pass-generator.go` file into a binary by running `go build pass-generator.go`
- Move the `pass-generator` binary to `/usr/local/bin`
- And then run the following command

```Bash
pass-generator --length 32 --upper --lower --digits --special
```

Keep in mind that only the `--digits` and `--special` flats are required here. But by running the command above the output would be something like

```Plain
Q!`CFtnx^$6_9q}]Y:kjgpU0Akn]r2QB
```

# Code

```go
# pass-generator.go
package main

import (
 "flag"
 "fmt"
 "math/rand"
 "time"
)

var (
 length     = flag.Int("length", 12, "Password length")
 useLower   = flag.Bool("lower", true, "Use lowercase letters")
 useUpper   = flag.Bool("upper", true, "Use uppercase letters")
 useDigits  = flag.Bool("digits", false, "Use digits")
 useSpecial = flag.Bool("special", false, "Use special characters")
)

const (
 lowercase = "abcdefghijklmnopqrstuvwxyz"
 uppercase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
 digits    = "0123456789"
 special   = "!@#$%^&*()-_=+,.?/:;{}[]`~"
)

func generatePassword() string {
 var charset string

 if *useLower {
  charset += lowercase
 }

 if *useUpper {
  charset += uppercase
 }

 if *useDigits {
  charset += digits
 }

 if *useSpecial {
  charset += special
 }

 if charset == "" {
  panic("No character sets selected")
 }

 r := rand.New(rand.NewSource(time.Now().UnixNano()))
 password := make([]byte, *length)
 for i := range password {
  password[i] = charset[r.Intn(len(charset))]
 }

 return string(password)
}

func main() {
 flag.Parse()

 if *length <= 0 {
        fmt.Println("Error: Password length must be greater than 0.")
        return
    }

 if !(*useLower || *useUpper || *useDigits || *useSpecial) {
        fmt.Println("Error: At least one character set must be selected.")
  fmt.Println("Use -h for help.")
        return
    }

 password := generatePassword()
 fmt.Println(password)
}
```

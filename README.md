# promptui

Interactive prompt for command-line applications.

We built Promptui because we wanted to make it easy and fun to explore cloud
services with [manifold cli](https://github.com/manifoldco/manifold-cli).

[Code of Conduct](./CODE_OF_CONDUCT.md) |
[Contribution Guidelines](./.github/CONTRIBUTING.md)

[![GitHub release](https://img.shields.io/github/tag/manifoldco/promptui.svg?label=latest)](https://github.com/manifoldco/promptui/releases)
[![GoDoc](https://img.shields.io/badge/godoc-reference-blue.svg)](https://godoc.org/github.com/manifoldco/promptui)
[![Travis](https://img.shields.io/travis/manifoldco/promptui/master.svg)](https://travis-ci.org/manifoldco/promptui)
[![Go Report Card](https://goreportcard.com/badge/github.com/manifoldco/promptui)](https://goreportcard.com/report/github.com/manifoldco/promptui)
[![License](https://img.shields.io/badge/license-BSD-blue.svg)](./LICENSE.md)

## Overview

![promptui](https://media.giphy.com/media/xUNda0Ngb5qsogLsBi/giphy.gif)

Promptui is a library providing a simple interface to create command-line
prompts for go. It can be easily integrated into
[spf13/cobra](https://github.com/spf13/cobra),
[urfave/cli](https://github.com/urfave/cli) or any cli go application.

Promptui has two main input modes:

- `Prompt` provides a single line for user input. Prompt supports
  optional live validation, confirmation and masking the input.

- `Select` provides a list of options to choose from. Select supports
  pagination, search, detailed view and custom templates.

For a full list of options check [GoDoc](https://godoc.org/github.com/manifoldco/promptui).

## Basic Usage

### Prompt

```go
package main

import (
	"errors"
	"fmt"
	"strconv"

	"github.com/manifoldco/promptui"
)

func main() {
	validate := func(input string) error {
		_, err := strconv.ParseFloat(input, 64)
		if err != nil {
			return errors.New("Invalid number")
		}
		return nil
	}

	prompt := promptui.Prompt{
		Label:    "Number",
		Validate: validate,
	}

	result, err := prompt.Run()

	if err != nil {
		fmt.Printf("Prompt failed %v\n", err)
		return
	}

	fmt.Printf("You choose %q\n", result)
}
```

### Select

```go
package main

import (
	"fmt"

	"github.com/manifoldco/promptui"
)

func main() {
	prompt := promptui.Select{
		Label: "Select Day",
		Items: []string{"Monday", "Tuesday", "Wednesday", "Thursday", "Friday",
			"Saturday", "Sunday"},
	}

	_, result, err := prompt.Run()

	if err != nil {
		fmt.Printf("Prompt failed %v\n", err)
		return
	}

	fmt.Printf("You choose %q\n", result)
}
```

### More Examples

See full list of [examples](https://github.com/manifoldco/promptui/tree/master/_examples)

Config file can contain the folowing items:

FwFileName: path_to_fw.zip

DeviceRevision: YOUR_DEVICE_REVISION // example: "1.00"

DeviceModel: S020M or S020S

Port: YOUR_PORT // example:/dev/ttyUSB0

AwsKeyID: KEY_ID

AwsAccessKey: ACCES_KEY

Region: YOUR_REGION // example:eu-central-1

WiFiSSID: YOUR_SSID

WiFiPass: YOUR_PASS

// if LicenseKey is emty 'License' will be written to the device

// if not emty a new license will be requested

LicenseKey: YOUR_LICENSE_KEY 

// can be copied from the device if it already has a licence, 'LicenseKey' should be emty in that case

License: YOUR_LICENSE 

// if emty a new serial number will be created

// id not empty the provided  erial number will be used

SerialNumber: YOUR_SERIAL_NUMBER

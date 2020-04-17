GO Transliterator
==============

[![Build Status](https://travis-ci.com/alexsergivan/transliterator.svg?branch=master)](https://travis-ci.com/github/alexsergivan/transliterator)
[![Coverage Status](https://coveralls.io/repos/github/alexsergivan/transliterator/badge.svg)](https://coveralls.io/github/alexsergivan/transliterator)


GO Transliterator provides one-way string transliteration. It takes Unicode text and coverts to ASCII characters.

For now, only these languages have specific transliteration rules: DE, DA, EO, RU, BG, SV, HU, HR, SL, SR, NB, UK, MK, CA, BS. For other languages, general ASCII transliteration rules will be applied. Also, this package supports adding custom transliteration rules for your specific use-case. Please check the examples section below.


Installation
------------

```
go get -u github.com/alexsergivan/transliterator
```


Language specific transliteration example:
------

```go
package main

import (
	"fmt"
	"github.com/alexsergivan/transliterator"
)

func main() {
	trans := transliterator.NewTransliterator(nil)
	text := "München"
	// Langcode should be provided accrding to ISO 639-1.
	fmt.Println(trans.Transliterate(text, "de")) // Result: Muenchen
	fmt.Println(trans.Transliterate(text, "en")) // Result: Munchen

	anotherText := "你好"
	fmt.Println(trans.Transliterate(anotherText, "")) // Result: Ni Hao

	text := "Київ"
	fmt.Println(trans.Transliterate(text, "uk")) // Result: Kyiv
	fmt.Println(trans.Transliterate(text, "en")) // Result: Kiyiv
	fmt.Println(trans.Transliterate(text, "")) // Result: Kiyiv
}
```

Adding of custom Language translitartion rules:
------

```go
package main

import (
	"fmt"
	"github.com/alexsergivan/transliterator"
)

func main() {
	customLanguageOverrites := make(map[string]map[rune]string)

	customLanguageOverrites["myLangcode"] = map[rune]string{
		// Ї
		0x407: "CU",
		// и
		0x438: "y",
	}
	trans := transliterator.NewTransliterator(&customLanguageOverrites)
	text := "КиЇв"
	fmt.Println(trans.Transliterate(text, "myLangcode")) // Result: KyCUv

}
```

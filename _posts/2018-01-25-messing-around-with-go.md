---
layout: post
title:  "Messing around with Golang"
date:   2018-01-25 12:20:00 -0400
categories: Blog Post
---

# CodeWars with Golang

Over the past few days I have been getting more and more interested and into the go language. After reading through multiple docs and examples I decided to try my hand at a few (very easy) go coding problems. Here is the result.

Challenge 1 - Century
```
Given a year, return the century it is in.

The first century spans from the year 1 up to and including the year 100,
the second - from the year 101 up to and including the year 200, etc.

Let's see some examples:

centuryFromYear(1705) // returns 18
centuryFromYear(1900) // returns 19
centuryFromYear(1601) // returns 17
centuryFromYear(2000) // returns 20
```

Solution :

```
package kata

func century(year int) int {
	if year%100 == 0 {
		return year / 100
	} else {
		return year/100 + 1
	}

}
```


Challenge 2 - Return Negative
```
In this simple assignment you are given a number and have to make it negative. But maybe the number is already negative?

Example:

MakeNegative(1)    # return -1
MakeNegative(-5)   # return -5
MakeNegative(0)    # return 0
Notes:

The number can be negative already, in which case no change is required.
Zero (0) can't be negative, see examples above.
```

Solution :
```
package kata

func MakeNegative(x int) int {
	if x < 0 {
		return x
	} else if x > 0 {
		return x * -1
	} else {
		return 0
	}
}
```

Challenge 3 - String Repeat
```
Write a function called repeatStr which repeats the given string string exactly n times.

repeatStr(6, "I") // "IIIIII"
repeatStr(5, "Hello") // "HelloHelloHelloHelloHello"
```

Solution :
```
package kata

import (
      "strings"
)

func RepeatStr(repititions int, value string) string {
  y:= strings.Repeat(value, repititions)
  return y
}
```

Challenge 4 -
```
Return a new array consisting of elements which are multiple of their own index in input array (length > 1).

Some cases:

[22, -6, 32, 82, 9, 25] =>  [-6, 32, 25]

[68, -1, 1, -7, 10, 10] => [-1, 10]

[-56,-85,72,-26,-14,76,-27,72,35,-21,-67,87,0,21,59,27,-92,68] => [-85, 72, 0, 68]
```

Solution :
```
package kata

import (
	"fmt"
)

func multipleOfIndex(ints []int) []int {
	var empty []int
	for index, value := range ints {
		if index != 0 {
			if value%index == 0 {
				empty = append(empty, value)
			}
		}
	}
	// good luck
	fmt.Println(empty)
	return empty
}
```
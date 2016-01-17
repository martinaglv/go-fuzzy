# go-fuzzy
Go-fuzzy is a Golang fuzzy search implementation that for given keys returns objects with the closest values to an input.

## Installation
```bash
go get github.com/antoan-angelov/go-fuzzy
```

## Options
**keys**  
The list of properties to use fuzzy search on. It supports nested properties via dot notation.

```go
type Laptop struct {
  Manufacturer string
  HasFan bool
  Processor CPU
}

type CPU struct {
  Manufacturer string
  Series string
  Cores int
  Frequency int
}

processor := &CPU{Cores: 4, Frequency: 4}
laptop := &Laptop{HasFan: true, Processor: processor}

laptops := []Laptop { laptop }

fuzzy := &GoFuzzy{array: laptops, keys: []string{"Manufacturer", "Processor.Manufacturer"} }
```

---

**id**  
Name of the identifier property. If set, instead of returning the objects themselves, it will return the specified identifier of the objects.

---

**caseSensitive**  
Whether comparisons should be case sensitive.

---

**shouldSort**  
Whether to sort the result list by score.

---

**searchFn**  
The search function to use. The object must implement Searchable interface:
```go
type Searchable interface {
    SetPattern(pattern string, options []string)
    Search(text string)
}
```

---

**getFn**  
The method used to access an object's property. The default implementation handles dot notation nesting (i.e. a.b.c).

---

**sortFn**  
The function that is used for sorting the result list.


### Bitap specific options
**location**  
Determines approximately where in the text is the pattern expected to be found.

---

**threshold** 
At what point the match algorithm gives up. A threshold of 0.0 requires a perfect match (of both letters and location), a threshold of 1.0 would match anything.

---

**distance**  
Determines how close the match must be to the fuzzy location (specified by location). An exact letter match which is distance characters away from the fuzzy location would score as a complete mismatch. A distance of 0 requires the match be at the exact location specified, a threshold of 1000 would require a perfect match to be within 800 characters of the location to be found using a threshold of 0.8.

---

**maxPatternLength**  
The maximum length of the pattern. The longer the pattern, the more intensive the search operation will be. Whenever the pattern exceeds the maxPatternLength, an error will be thrown.

## Methods

### search(pattern)

@param {string} pattern The pattern string to fuzzy search on.
@return A list of all serch matches.

Searches for all the items whose keys (fuzzy) match the pattern.

### set(list)

@param list
@return The newly set list

Sets a new list for Fuse to match against.

## License
```
The MIT License (MIT)

Copyright (c) 2016 Antoan Angelov

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

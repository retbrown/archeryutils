# archeryutils

![GitHub](https://img.shields.io/github/license/retbrown/archeryutils)
![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/retbrown/archeryutils/testing.yaml)

> **This is a Go port of [jatkinson1000/archeryutils](https://github.com/jatkinson1000/archeryutils), ported and maintained by [@retbrown](https://github.com/retbrown). The original library is written in Python — all credit for the underlying algorithms, data, and design goes to the upstream authors. This fork is not affiliated with or endorsed by the upstream project.**

A collection of archery utilities in Go, ported from the Python library [archeryutils](https://github.com/jatkinson1000/archeryutils) by Jack Atkinson et al.
Designed to make the development of archery codes and apps easier.

Contains:
- Generic representations of targets and rounds
- World Archery, Archery GB, IFAA, and Archery Australia rounds (embedded JSON)
- Calculations for Archery GB handicaps and Archery Australia archer skill level
- Calculation of Archery GB classifications (outdoor, indoor, field, legacy indoor/field)

## Upstream

The algorithms, round data, and classification logic in this repository are a port of the Python library [jatkinson1000/archeryutils](https://github.com/jatkinson1000/archeryutils), synced against upstream v3.0.0. If you are working in Python, use the original library directly.

---

## Go

### Installation

```sh
go get github.com/retbrown/archeryutils
```

Requires Go 1.24+.

### Packages

| Package | Description |
|---|---|
| `targets` | `Target`, `ScoringSystem`, `FaceSpec`, distance/diameter quantities |
| `rounds` | `Pass`, `Round`, embedded round databases (AGB, WA, IFAA, AA) |
| `length` | Unit conversion helpers (metres, yards, cm, inches) |
| `handicaps` | AGB, AGBold, AA, AA2 handicap schemes; `HandicapTable` |
| `classifications` | AGB outdoor, indoor, field, old-indoor, old-field classifications |

### Quick start

```go
package main

import (
    "fmt"
    "github.com/retbrown/archeryutils/classifications"
    "github.com/retbrown/archeryutils/rounds"
)

func main() {
    r := rounds.AGBOutdoorMetric()["wa1440_90"]
    class, _ := classifications.CalculateOutdoorClassification(
        1200, r,
        classifications.Recurve, classifications.Male, classifications.Adult,
        true, true,
    )
    fmt.Println(class) // e.g. "B1"
}
```

```go
import (
    "fmt"
    "github.com/retbrown/archeryutils/handicaps"
    "github.com/retbrown/archeryutils/rounds"
)

s := handicaps.MustScheme("AGB")
r := rounds.WAOutdoor()["wa1440_90"]
h, _ := handicaps.HandicapFromScore(s, 1200, r, 0, true)
fmt.Printf("Handicap: %.0f\n", h)
```

### Testing

```sh
go test ./...
go test -race ./...
```

### Module layout

```
go.mod
targets/           target face definitions, scoring systems
rounds/            pass and round types; embedded JSON round data
length/            unit conversion
handicaps/         handicap schemes (AGB, AGBold, AA, AA2), tables
classifications/   AGB classification calculations + embedded data
```

---

## Licence

Licensed under the MIT Licence — see [LICENCE](LICENCE).

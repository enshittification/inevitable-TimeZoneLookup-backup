# SwiftTimeZoneLookup

[![Test](https://github.com/patrick-zippenfenig/SwiftTimeZoneLookup/actions/workflows/test.yml/badge.svg)](https://github.com/patrick-zippenfenig/SwiftTimeZoneLookup/actions/workflows/test.yml)

Resolve geographical coordinates to timezones and countries. This is a Swift wrapper for [ZoneDetect](https://github.com/BertoldVdb/ZoneDetect). 

A timezone database with up to 20 meter resolution is embedded as a bundle resource (~5 MB). If you want to manually specify the database location, use `try SwiftTimeZoneLookup(databasePath: "/path_to_timezone16_21_files/..")`

Embedded timezone database version [2024a (2024-03-14)](https://github.com/evansiroky/timezone-boundary-builder/releases/tag/2024a).

This library is thread safe.

## Usage
Add `SwiftTimeZoneLookup` as a dependency to your `Package.swift`

```swift
  dependencies: [
    .package(url: "https://github.com/patrick-zippenfenig/SwiftTimeZoneLookup.git", from: "1.0.0")
  ],
  targets: [
    .target(name: "MyApp", dependencies: [
      .product(name: "SwiftTimeZoneLookup", package: "SwiftTimeZoneLookup"),
    ])
  ]
```

In your code

```swift
import SwiftTimeZoneLookup


let database = try SwiftTimeZoneLookup()
guard let timezone = database.simple(latitude: 47.5, longitude: 8.6) else {
  fatalError("Timezone not found, coordinates invalid?")
}
print(timezone) // "Europe/Zurich"


guard let lookup = database.lookup(latitude: 47.5, longitude: 8.6) else {
  fatalError("Timezone not found, coordinates invalid?")
}
print(lookup) // SwiftTimeZoneLookupResult(timezone: "Europe/Zurich", countryName: Optional("Switzerland"), countryAlpha2: Optional("CH"))

```

## Build database
SwiftTimeZoneLookup comes with an integrated database. The database can be updated with the following commands:

```bash
git clone --recurse-submodules git@github.com:patrick-zippenfenig/SwiftTimeZoneLookup.git

brew install shapelib wget
cd Submodules/ZoneDetect/database/builder

# make sure to select to newest version in make.db script

LIBRARY_PATH=/opt/homebrew/Cellar/shapelib/1.5.0/lib CPATH=/opt/homebrew/Cellar/shapelib/1.5.0/include ./makedb.sh

cp out_v1/timezone* ../../../../Sources/SwiftTimeZoneLookup/Resources

```

## Credits
[ZoneDetect](https://github.com/BertoldVdb/ZoneDetect) as the upstream library.

Contains data from Natural Earth, placed in the Public Domain. Contains information from https://github.com/evansiroky/timezone-boundary-builder, which is made available here under the Open Database License (ODbL)
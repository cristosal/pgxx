# pgxx

A library that facilitates common sql queries with pgx

- Inserts that assign id field from database to struct
- Selects and queries which support generics
- Struct fields to database columns via `db` tags
- Support for pagination
- ReadOnly fields

## Installation

`go get -u github.com/cristosal/pgxx`

## Usage

Here is a simple example how to insert and update structs

```go
package main

import (
    "os"
    "fmt"
    "context"

    "github.com/cristosal/pgxx"
    "github.com/jackc/pgx/v5"
)

type Person struct {
    ID      pgxx.ID `db:"id"`
    Name    string  `db:"name"`
    Age     int     `db:"age"`
}

// TableName tells pgxx which table to use for the given struct
// if not implemented pgxx will use the snake-cased version of the struct name ie) person
func (p *Person) TableName() string {
    return "person"
}

func main() {
    conn, err := pgx.Connect(context.Background(), os.Getenv("CONNECTION_STRING"))
    if err != nil {
        fmt.Println("unable to connect to postgres")
        os.Exit(1)
    }

    p := Person{
        Name: "John Doe",
        Age: 29,
    }

    // inserts into the person table
    if err := pgxx.Insert(conn, &p): err != nil {
        fmt.Printf("error inserting person: %v\n", err)
        os.Exit(1)
    }

    fmt.Println("successfully added person")
}

```

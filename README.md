# Quickstart

```go
package main

import (
    "fmt"
    "log"

    "github.com/psotou/fintoc"
)

func main() {
    client, err := fintoc.NewClient("secret")
    if err != nil {
        log.Fatal(err)
    }

    link := client.Link.Get("linkToken")
    fmt.Prinln(link)
}
```

# Disclaimer

This ~yet another carbon copy~  Go library is heavily influenced by the [Python SDK](https://github.com/fintoc-com/fintoc-python) devoleped by the guys at [Fintoc](https://fintoc.com/).

The library is in a very early stage, which means you can only use certain endpoints, namely, the ones related to the **links**, **accounts** and **movements** resources. Though the whole API should be wrapped up soon.

# HOW-TO

## The `Link` interface

The `Link` interface comes with the methods `All`, `Get`, and `Delete`.

```go
client, err := fintoc.NewClient("secret")
if err != nil {
    log.Fatal(err)
}

link := client.Link.Get("linkToken") // to return one link object
links := client.Link.All()           // to return all of them
for _, l := range links {
    fmt.Println(l.Id)
}
```

The `Get` method comes with the `Update` and `Delete` methods, which act upon the object generated by said `Get`.

To **update** a link (that is, changing its active status to either true or false), we can do:

```go
link := client.Link.Update("linkToken", false) // to return one link object
```

or

```go
link := client.Link.Get("linkToken") // to return one link object
link.Update(false)
```

To **delete** a link, you can either run

```go
client.Link.Delete("linkId")        // will print the http status code of the request
```

or

```go
link := client.Link.Get("linkToken")
link.Delete()
```

## The `Account` interface

Similarly, the `Account` interface comes with the methods `All` and `Get`, which can be used as:

```go
account := link.Account.Get("accountId")
accounts := link.Account.All()
for _, acc := range accounts {
    fmt.Println(acc.Id)
}
```

## The `Movement` interface

The `Movement` interface also comes with the methods `All` and `Get`:

```go
movement := account.Movement.Get("movementId")
movements := account.Movement.All()
for _, mov := range movements {
    fmt.Println(mov.Id)
}
```

The `All` method of this interface allows for the use of query params. The endpoint for this resource supports three query params: `since`, `until` and `per_page` (more on the use of these [here](https://docs.fintoc.com/reference/listar-movimientos)). 

```go
params := fintoc.Params{Since: "2021-08-01", Until: "2021-08-31", PerPage: "100"}
movements := account.Movement.All(params)
for _, mov := range movements {
    fmt.Println(mov.Id)
}
```

# TO-DO

+ [ ] Add the rest of the resources
+ [ ] Add a workflow
+ [x] Add unit tests
+ [x] Add query params to movements endpoint
+ [x] Add methods PATCH and DELETE for the link object

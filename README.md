### go-commander
---
https://github.com/yitsushi/go-commander

```go
package main

import "github.com/yitsushi/go-commaner"

type YourCommand struct {
}

func(c *YourCommand) Execute(opt *commander.CommanderHelper) {
}

func NewYourCommand(appName string) *commander.ComandWrapper {
  return &commander.CommandWrapper {
    Handler: &YourCommnad{},
    Help: &commander.CommandDescriptor{
      Name: "your-command",
      ShortDescription: "This is my own command",
      LongDescription: `This is a very long description about this command.`,
      Arguments: "<filename> [optional-argument]",
      Examples: []string {
        "test.txt",
        "test.txt copy",
        "test.txt move"
      },
    },
  }
}

func main() {
  registry := commander.NewCommandRegistry()
  registry.Register(NewYourCommand)
  registry.Execute()
}

import subcommand "github.com/yitsushi/mypackage/command/something"

func (c *Something) Execute(opts *commander.CommandHelper) {
  registry := commander.NewCommandRegistry()
  registry.Depth = 1
  registry.Register(subcommand.NewSomethingMySubCommand)
  registry.Execute()
}


func MyValidator(c *commander.CommandHelper) {
  if c.Arg(0) == "" {
    panic("File?")
  }
  
  info, err := os.Stat(c.Arg(0))
  if err != nil {
    panic("File not found")
  }
  
  if !info.Mode().IsRegular() {
    panic("It's not a regular file")
  }
  
  if c.Arg(1) != "" {
    if c.Arg(1) != "copy" && c.Arg(1) != "move" {
      panic("Invalid operation")
    }
  }
}

func NewYourCommand(appName string) *commander.CommandWrapper {
  return &commander.CommandWrapper {
    Handler: &YourCommand{},
    Validator: MyValidattor
    Help: &commander.CommandDescriptor{
      Name: "your-command",
      ShortDescription: "This is my own command",
      LongDescription: `This is a very long description about this command.`,
      Arguments: "<filename> [optional-argument]",
      Examples: []string {
        "test.txt",
        "test.txt copy",
        "test.txt move"
      },
    },
  }
}

&commander.CommandWrapper{
  Handler: &MyCommand{},
  Arguments: []*commander.Argument{
    &commander.Argument{
      Name: "list",
      Type: "StringArray[]",
    },
  },
  Help: &commander.CommandDescriptor{
    Name: "my-command",
  },
}

if opts.HasValidTypedOpt("list") == nil {
  myList := opts.TypedOpt("list").([]string)
  if len(myList) > 0 {
    mockPrintf("My list: %v\n", myList)
  }
}

type MyCustomType struct {
  ID uint64
  Name string
}

commander.RegisterArgumentType("MyType", func(value string) (interface{}, error) {
  values := strings.Split(value, ":")
  
  if len(values) < 2 {
    return &MyCustomType{}, errors.New("Invalid format! MyType => 'ID:Name'")
  }
  
  id, err := strconv.ParseUint(values[0], 10, 64)
  if err != nil {
    return &MyCustomType{}, errors.New("Invaid format! MyType => 'ID:Name'")
  }
  
  return &MyCustomType {
    ID: id,
    Name: values[1],
  },
  nil
})

&commander.CommandWrapper{
  Handler: &MyCommand{},
  Arguments: []*commander.Argument{
    &commander.Argument{
      Name: "owner",
      Type: "My Type",
      FailOnError: true,
    },
  },
  Help: &commander.CommandDescriptor{
    Name: "my-command",
  },
}

if opts.HasValidTypedOpt("owner") == nil {
  owner := opts.TypedOpt("owner").(*MyCustomType)
  mockPrintf("OwnerID: %d, Name: %s\n", owner.ID, owner.Name)
}
```

```
go build mytool.go
./mytool
./mytool help your-command

go get github.com/yitsushi/go-commander
totp-cli help
```

```
```



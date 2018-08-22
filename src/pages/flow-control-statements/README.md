# Flow control statements
## (B) Loops and iteration
### For
**JS**
```Javascript
for(let i=0;i<10;i++){
    console.log(i);
}
```

**Go**
```Go
for i := 0; i < 10; i++ {
	fmt.Println(i)
}
```

### While
In Go, the `for`'s init and post statement are optional, effectively making it also a "while" statement:

**JS**
```Javascript
var i=0;
while(i<10){
    console.log(i);
    i++;
}
```

**Go**
```Go
i := 0
for i < 10 {
	fmt.Println(i)
	i++
}
```
### Iterating over an Array/Slice

**JS**
```Javascript
['Rick','Morty','Beth','Summer','Jerry'].forEach(function(value,index){
    console.log(value + ' at index ' + index);
});
```

**Go**
```Go
for i, v := range []string{"Rick", "Morty", "Beth", "Summer", "Jerry"} {
	fmt.Printf("%v at index %d", v, i)
}
```


## (B) If/Else
Go's `if` can contain an init statement, with variables declared scoped only to the `if` and `else` blocks.

**Go**
```Go
if value := getSomeValue(); value < limit {
	return value
} else {
	return value / 2
}
```

## (D) Switch
The switch statement was one of the motivation for writing this document.

Go defaults to break, and `fallthrough` needed for otherwise.

Javascript defaults to fallthrough, and `break` needed for otherwise.

**JS**
```Javascript
switch (favorite) {
    case "yellow":
        console.log("yellow");
        break;
    case "red":
        console.log("red");
    case "pruple":
        console.log("(and) purple");
    default:
        console.log("white");
}

```

**Go**
```Go
switch favorite {
case "yellow":
	fmt.Println("yellow")
case "red":
	fmt.Println("red")
	fallthrough
case "pruple":
	fmt.Println("(and) purple")
default:
	fmt.Println("white")
}
```

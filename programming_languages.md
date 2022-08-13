

# dotnet
 
## Online Compiler
- C# https://dotnetfiddle.net/
- Py https://www.programiz.com/python-programming/online-compiler/
- Ts https://www.typescriptlang.org/play
- Js https://jsfiddle.net/

## Hello
**C#**
```
using System;

public class Program
{
	public static void Main(string[] args)
	{
		Console.WriteLine("Hello World");
	}
}
```
**Ts**
```
function init() : void {
	console.log("Hello World");
};
init();
```
**js**
```
(function init() {
	console.log("Hello World");
}());
```
**Py**
```
import sys;

if __name__=="__main__":
	print("Hello World");
```
## Ternary operation
**C#**
```
bool isDone = false;
string result = isDone ? "Success" : "Fail";
Console.WriteLine(result);
```
**Ts**
```
let isDone: Boolean = false;
let result: string = isDone ? "Success" : "Fail";
console.log(result);
```
**Js**
```
var isDone = false;
var result = isDone ? "Success" : "Fail";
console.log(result);
```
**Py**
```
isDone = False;
result = "Success" if isDone else "Fail";
print(result);
```

## tmpl
**C#**
```
using System;
using System.Collections.Generic;
using System.Linq;
					
public class Program
{
	public static void Main()
	{
		List<int> a = new List<int>() { 1, 2, 3, 4 };
		List<int> b = new List<int>() { 3, 4, 5, 6 };		
		List<int> union = a.Union(b).ToList();
		List<int> except = a.Except(b).ToList();
		List<int> intersect = a.Intersect(b).ToList();
		
		Console.WriteLine(String.Join(',', union));
		Console.WriteLine(String.Join(',', except));
		Console.WriteLine(String.Join(',', intersect));
	}
}
```
**Ts**
```
cmd     #
```
**Js**
```
cmd     #
```
**Py**
```
import sys;

if __name__ == "__main__":
    a = [1, 2, 3, 4];
    b = [3, 4, 5, 6];
    unions = a + [value for value in b if value not in a];
    excepts = [value for value in a if value not in b];
    intersects = [value for value in a if value in b];
    
    print(unions);
    print(excepts);
    print(intersects);
```   

## tmpl
**C#**
```
cmd     #
```
**Ts**
```
cmd     #
```
**Js**
```
cmd     #
```
**Py**
```
cmd     #
```
## need to add

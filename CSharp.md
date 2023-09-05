
Console.writeLine("hello");

### string interpolation
Console.writeLine($"hello {name}");

string name = "abc"
name.length
name.TrimStart()
name.TrimEnd()
name.Trim()

### integer
int a = 10;
double a = 3;
decimal a;

## List
var- auto infer type
```
var names = new List<string>{"<name>", "scott"}
var intList = new List<int>;
names.Count
foreach(string name in names){name}
for(int i=0; i<n; i++>){names[i]}
names.Add("abc")
names.Remove("abc")
```

## Object Oriented Programming
### classes
```
namespace MyBank{
    public class BankAccount{
        public string Number {get;}
        public string Owner {get; set;}
        public decimal Balance {get;}

        public BankAccount(string name, decimal initBalance){
            this.Owner = name;
            this.Balance = initBalance;
        }

        public void MakeDeposit(decimal amount, DateTime date, string note){

        }
        public void MakeWithdrawal(decimal amount, DateTime date, string note){

        }
    }
}

## creating an Object
var account = new BankAccount("abc", 10);
account.Number ...
```
## NOTE
static variables remain same for different instances of objects
```

private static int a = 10;
constructor{
    a += 1;
}
# init new object : a = 11
# init new object : a = 12
```
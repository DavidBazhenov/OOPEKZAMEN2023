## &nbsp;&nbsp;&nbsp;&nbsp;C#. Классы String и StringBuilder.
### &nbsp;&nbsp;&nbsp;&nbsp;String
>&nbsp;&nbsp;&nbsp;&nbsp;В языке C# строковые значения представляет тип string, а вся функциональность работы с данным типом сосредоточена в классе System.String. Собственно string является псевдонимом для класса String. Объекты этого класса представляют текст как последовательность символов Unicode.  

#### &nbsp;&nbsp;&nbsp;&nbsp;Создавать строки можно, как используя переменную типа string и присваивая ей значение, так и применяя один из конструкторов класса String:

```
string s1 = "hello";
string s2 = new String('a', 6); // результатом будет строка "aaaaaa"
string s3 = new String(new char[] { 'w', 'o', 'r', 'l', 'd' });
string s4 = new String(new char[] { 'w', 'o', 'r', 'l', 'd' }, 1, 3); // orl
 
Console.WriteLine(s1);  // hello
Console.WriteLine(s2);  // aaaaaaa
Console.WriteLine(s3);  // world
Console.WriteLine(s4);  // orl
```
---

### &nbsp;&nbsp;&nbsp;&nbsp;StringBuilder

>&nbsp;&nbsp;&nbsp;&nbsp;**StringBuilder** — это изменяемый (редактируемый) строковый класс, определенный в пространстве имен System.Text. С его помощью вы можете напрямую изменять содержимое строкового объекта, что обеспечивает более высокую производительность чем традиционный способ класса System.String, где любое редактирование строки не меняет ее, а возвращает новый строковый объект в измененном формате.  

&nbsp;&nbsp;&nbsp;&nbsp;Рассмотрим концепцию класса на примере консольного приложения:  
```
var stringA = Console.ReadLine();
var stringB = Console.ReadLine();
stringA = stringA + stringB;
```
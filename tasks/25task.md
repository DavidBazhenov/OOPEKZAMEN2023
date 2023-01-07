## &nbsp;&nbsp;&nbsp;&nbsp;С#. Наследование, виртуальные функции
### &nbsp;&nbsp;&nbsp;&nbsp;Наследование  

>&nbsp;&nbsp;&nbsp;&nbsp;Наследование (inheritance) является одним из ключевых моментов ООП. Благодаря наследованию один класс может унаследовать функциональность другого класса.

#### &nbsp;&nbsp;&nbsp;&nbsp;Все классы по умолчанию могут наследоваться. Однако здесь есть ряд ограничений:

+ Не поддерживается множественное наследование, класс может наследоваться только от одного класса.

+ При создании производного класса надо учитывать тип доступа к базовому классу - тип доступа к производному классу должен быть таким же, как и у базового класса, или более строгим. То есть, если базовый класс у нас имеет тип доступа internal, то производный класс может иметь тип доступа internal или private, но не public.

+ Однако следует также учитывать, что если базовый и производный класс находятся в разных сборках (проектах), то в этом случае производый класс может наследовать только от класса, который имеет модификатор public.

+ Если класс объявлен с модификатором sealed, то от этого класса нельзя наследовать и создавать производные классы.

---

### &nbsp;&nbsp;&nbsp;&nbsp;Виртуальные функции

> &nbsp;&nbsp;&nbsp;&nbsp;**Виртуальная функция** — это функция-член, которую предполагается переопределить в производных классах. При ссылке на объект производного класса с помощью указателя или ссылки на базовый класс можно вызвать виртуальную функцию для этого объекта и выполнить версию функции производного класса.  

&nbsp;&nbsp;&nbsp;&nbsp;Предположим, что базовый класс содержит функцию, объявленную как виртуальную , а производный класс определяет ту же функцию. Функция производного класса вызывается для объектов производного класса, даже если она вызывается с помощью указателя или ссылки на базовый класс. В следующем примере показан базовый класс, который предоставляет реализацию функции PrintBalance и два производных класса.  

```
// deriv_VirtualFunctions.cpp
// compile with: /EHsc
#include <iostream>
using namespace std;

class Account {
public:
   Account( double d ) { _balance = d; }
   virtual ~Account() {}
   virtual double GetBalance() { return _balance; }
   virtual void PrintBalance() { cerr << "Error. Balance not available for base type." << endl; }
private:
    double _balance;
};

class CheckingAccount : public Account {
public:
   CheckingAccount(double d) : Account(d) {}
   void PrintBalance() { cout << "Checking account balance: " << GetBalance() << endl; }
};

class SavingsAccount : public Account {
public:
   SavingsAccount(double d) : Account(d) {}
   void PrintBalance() { cout << "Savings account balance: " << GetBalance(); }
};

int main() {
   // Create objects of type CheckingAccount and SavingsAccount.
   CheckingAccount checking( 100.00 );
   SavingsAccount  savings( 1000.00 );

   // Call PrintBalance using a pointer to Account.
   Account *pAccount = &checking;
   pAccount->PrintBalance();

   // Call PrintBalance using a pointer to Account.
   pAccount = &savings;
   pAccount->PrintBalance();
}
```

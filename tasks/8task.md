## &nbsp;&nbsp;&nbsp;&nbsp;С++. Виртуальные функции и их вызов. Абстрактные классы.
### &nbsp;&nbsp;&nbsp;&nbsp;Виртуальные функции и их вызов
>&nbsp;&nbsp;&nbsp;&nbsp;Виртуальная функция — это функция-член, которую предполагается переопределить в производных классах. При ссылке на объект производного класса с помощью указателя или ссылки на базовый класс можно вызвать виртуальную функцию для этого объекта и выполнить версию функции производного класса.  
#### Пример
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
При вызове функции с помощью указателей или ссылок применяются следующие правила.  

+ Вызов виртуальной функции разрешается в соответствии с базовым типом объекта, для которого она вызывается.  

+ Вызов невиртуальной функции разрешается в соответствии с типом указателя или ссылки.  

---

### &nbsp;&nbsp;&nbsp;&nbsp;Абстрактные классы

>&nbsp;&nbsp;&nbsp;&nbsp;**Абстрактные классы** - это классы, которые содержат или наследуют без переопределения хотя бы одну чистую виртуальную функцию. Абстрактный класс определяет интерфейс для переопределения производными классами.
#### Пример

```
class Figure
{
public:
    virtual double getSquare() = 0;
    virtual double getPerimeter() = 0;
    virtual void showFigureType() = 0;
};
```
&nbsp;&nbsp;&nbsp;&nbsp;При этом мы не можем создать объект абстрактного класса:
```
Figure figure;
```
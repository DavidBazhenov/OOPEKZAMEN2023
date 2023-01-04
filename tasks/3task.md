## &nbsp;&nbsp;&nbsp;&nbsp; С++. Разработка методов классов. Операции класса и их перегрузка.
### &nbsp;&nbsp;&nbsp;&nbsp; Разработка методов классов
>&nbsp;&nbsp;&nbsp;&nbsp;<font color="yellow">**Методы** - это функции, объявление которых размещено внутри определения класса или структуры. В список переменных, доступных для метода, неявно попадают все поля структуры или класса, в котором он объявлен.This text is red!</font>  

&nbsp;&nbsp;&nbsp;&nbsp;Говорят, что значения полей объекта класса определяют его состояние, а методы класса – поведение.   

&nbsp;&nbsp;&nbsp;&nbsp;В С++ имеется возможность управлять доступом к полям и методам класса, в частности, установление такого порядка, при котором доступ к полям класса имеют только его методы.(private, public)  

&nbsp;&nbsp;&nbsp;&nbsp;*Объединение данных и методов работы с ними в одну языковую конструкцию с возможностью управления доступом к данным называется инкапсуляцией.*  

&nbsp;&nbsp;&nbsp;&nbsp;C++ требует, чтобы каждый метод структуры или класса был упомянут в определении этой структуры или класса. Но допускается писать лишь объявление метода, определение размещать где-нибудь в другом месте:
```
// Определение структуры Vec2f содержит
// - объявление конструктора
// - объявление метода getLength
struct Vec2f
{
    float x = 0;
    float y = 0;

    Vec2f(float x, float y);
    float getLength() const;
};

// Определение конструктора (добавлен квалификатор "Vec2f::")
Vec2f::Vec2f(float x, float y)
    : x(x)
    , y(y)
{
}

// Определение метода getLength (добавлен квалификатор "Vec2f::")
float Vec2f::getLength() const
{
    const float lengthSquare = x * x + y * y;
    return std::sqrt(lengthSquare);
}
```

---

### &nbsp;&nbsp;&nbsp;&nbsp;Операторы
>&nbsp;&nbsp;&nbsp;&nbsp;<font color="yellow">Оператор - это символ, который сообщает компилятору выполнить определенные математические или логические манипуляции. C ++ богат встроенными операторами и предоставляет следующие типы операторов:</font>  
+ Арифметические операторы&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (+ - * /)
+ Реляционные операторы&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (< > <= и т.д.)
+ Логические операторы&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (|| && !)
+ Побитовые операторы&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (& | ^ ~  << >>)  
+ Операторы присваивания&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (= += и т.д.)  

---

### &nbsp;&nbsp;&nbsp;&nbsp;Перегрузка операторов
>&nbsp;&nbsp;&nbsp;&nbsp;<font color="yellow">Перегрузка операторов позволяет определить действия, которые будет выполнять оператор. Перегрузка подразумевает создание функции, название которой содержит слово operator и символ перегружаемого оператора. Функция оператора может быть определена как член класса, либо вне класса.</font>  

&nbsp;&nbsp;&nbsp;&nbsp;Перегрузить можно только те операторы, которые уже определены в C++. Создать новые операторы нельзя.  

&nbsp;&nbsp;&nbsp;&nbsp;Если функция оператора определена как отдельная функция и не является членом класса, то количество параметров такой функции совпадает с количеством операндов оператора. Например, у функции, которая представляет унарный оператор, будет один параметр, а у функции, которая представляет бинарный оператор, - два параметра. Если оператор принимает два операнда, то первый операнд передается первому параметру функции, а второй операнд - второму параметру. При этом как минимум один из параметров должен представлять тип класса   

&nbsp;&nbsp;&nbsp;&nbsp;Рассмотрим пример с классом Counter, который представляет секундомер и хранит количество секунд:  

```
#include <iostream>
 
class Counter
{
public:
    Counter(int sec)
    {
        seconds = sec;
    }
    void display() 
    {
        std::cout << seconds << " seconds" << std::endl;
    }
int seconds;
};
 
Counter operator + (Counter c1, Counter c2)
{
    return Counter(c1.seconds + c2.seconds);
}
 
int main()
{
    Counter c1(20);
    Counter c2(10);
    Counter c3 = c1 + c2;
    c3.display();   // 30 seconds
     
    return 0;
}
```
&nbsp;&nbsp;&nbsp;&nbsp;Также функции операторов могут быть определены как члены классов. Если функция оператора определена как член класса, то левый операнд доступен через указатель this и представляет текущий объект, а правый операнд передается в подобную функцию в качестве единственного параметра:  
```
#include <iostream>
 
class Counter
{
public:
    Counter(int sec)
    {
        seconds = sec;
    }
    void display() 
    {
        std::cout << seconds << " seconds" << std::endl;
    }
    Counter operator + (Counter c2)
    {
        return Counter(this->seconds + c2.seconds);
    }
    int operator + (int s)
    {
        return this->seconds + s;
    }
int seconds;
};
 
int main()
{
    Counter c1(20);
    Counter c2(10);
    Counter c3 = c1 + c2;
    c3.display();           // 30 seconds
    int seconds = c1 + 25;  // 45
     
    return 0;
}
```

---

#### Следует помнить:
- Что ряд операторов перегружаются парами. Например, если мы определяем оператор ==, то необходимо также определить и оператор !=. А при определении оператора < надо также определять функцию для оператора >  
- Особую сложность может представлять переопределение операций инкремента и декремента, поскольку нам надо определить и префиксную, и постфиксную форму для этих операторов 
    ```
      #include <iostream>
     
    class Counter
    {
    public:
        Counter(int sec)
        {
            seconds = sec;
        }
        void display() 
        {
            std::cout << seconds << " seconds" << std::endl;
        }
        // префиксные операторы
        Counter& operator++ ()
        {
            seconds += 5;
            return *this;
        }
        Counter& operator-- ()
        {
            seconds -= 5;
            return *this;
        }
        // постфиксные операторы
        Counter operator++ (int)
        {
            Counter prev = *this;
            ++*this;
            return prev;
        }
        Counter operator-- (int)
        {
            Counter prev = *this;
            --*this;
            return prev;
        }
        int seconds;
    };
    int main()
    {
        Counter c1(20);
        Counter c2 = c1++;
        c2.display();       // 20 seconds
        c1.display();       // 25 seconds
        --c1;
        c1.display();       // 20 seconds
        return 0;
    }
    ```

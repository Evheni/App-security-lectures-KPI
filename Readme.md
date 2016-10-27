#Лекции по комп. безопастности
<ul>
<li><a href="#Лекц1">Лекция 1</a></li>
<li><a href="#Лекц2">Лекция 2</a></li>
<li><a href="#Лекц3">Лекция 3</a></li>
<li><a href="#Лекц4">Лекция 4</a></li>
<li><a href="#Лекц5">Лекция 5</a></li>
<li><a href="#Лекц6">Лекция 6</a></li>
<li><a href="#Лекц7">Лекция 7</a></li>
<li><a href="#Лекц8">Лекция 8</a></li>
<li><a href="#Лекц9">Лекция 9</a></li>
</ul>
#Лекц1
##Короткі відомості
####1.Common language runtime (CLR)
####2.Сбірки дотнету
####3.Структура збірки
####4.Перегляд вмісту збірок
####5.Атрибути збірок

1)Компіляція
![Alt Text](1png.png "One png")
```
+ Оптимізація для процесора
+ Взаємодія компонентів (COM/DCOM)
- Моноплатформність
- Нестабільність
- Незахищеність ОС
- Крах DLL
```
2)Інтерпретація
```
+ Багатоплатформність
+ Стабільність
+ Захищеність
+ Спрощення розгортання
+ Оптимізація під процесор
+ Взаємодія компонентів
```
![Alt Text](2png.png "Two png")

#Лекц2
Збірка - одиниця в дотнет. Можем буду в .ехе(містять точку входа), .dll
![Alt Text](3png.png "3 png")
![Alt Text](4png.png "4 png")
![Alt Text](5png.png "5 png")
а) Найпростіший варіант створення збірок у вигляді одного файлу
![Alt Text](6png.png "6 png")

###ІІІ. Перегляд структури збірок
ildasm.exe - утиліта для "декомпіляції"
ilasm.exe - утиліта для компіляції IL-коду

###Перегляд збірки:
1) Ввести в команднії стрічці >ildasm.exe (Файл-Откріть)
2) Ввести в команднії стрічці >ildasm.exe <path>

для компіляції (створення збірки) в c# можна застосувати csc.exe
```
>csc /target:exe Hello.cs /out:hello.exe
             win.exe
             library (dll)
             module
```
для перегляду усіх команд можна використати:
```
>csc /?
```
Для створення модуля, можна використати таку команду:
```
>csc /target:module Module1.cs /out:Module1.netmodule
```
Для створення нових збірок, використовуючи готові модулі
```
>csc /target:exe /addModule:Module1.netmodule /out:hello.exe
```
Розділяють збірки на декілька файлів для створення программ з обмеженим функціоналом

###ІV. Використання збірок

Атрибути збірок можуть задаватись в файлі Assemblyinfo.cs, в якому можна задавати
цілий ряд додаткових параметрів збірок:
`Assembly Company - назва компанії`
`Assembly Copyright - інформація про авторів`
`Assembly Description - опис збірок`
`Assembly Version і т.д.`


###I. Структура програми
###II. Визначення збірки і посилань на збірки, що розділяються
###ІІІ. Визов методів
###ІV. Визначення просторів імен, класів і ...
###V. Визначення полів, конструкторів і властивостей класів

Як пишуться додатки, стуктура найпростішої програми:
Директиви (пишуться з крапки) - .method
За директивою можуть слідувати атрибути - .method static void
Потім ім'я методу - .method static void FirstPogram() {}
Далі вказуються деякі інструкції (тіло методу), але для компіляції головне
вказати ім'я збірки (посилання), яке може мати додаткові директиви
```
.assembly MyAssemble {

}
.method static void FirstProgram() {
    ret //вихід з файлу
}
```
Для .ехе файлів треба помітити точку входу за допомогою директиви .entrypoint
```
.assembly MyAssemble {

}
.method static void FirstProgram() {
    .entrypoint
    ret
}
```
Вказання посилань на зовнішні збірки, використовуються "дружні" імена
```
.assembly extern mscorlib{
    .ver 2:0:1:1
    .publickeytoken =  (11 B2 D4 F5 7D 99 FF)
}
.assembly MyAssemble {
    .ver 1:0:0:0
}
.method static void FirstProgram() {
    call [mscorlib]System.Console::Bepp()
    call [mscorlib]System.Console::ReadLine()
    .entrypoint
    ret
}
```
Визначення простору імен класів та функцій

Для створення простору імен використовується директива .namespace

a) .net 2.0 <
```
.namespace FirstSpace {
    .namespace  SecondSpace {
        .class BaseClass {

        }
    }
}
```
b) .net 2.0 >=
```
.class FirstSpace.SecondSpace.BaseClass {

}
.namespace FirstSpace {
    .namespace SecondSpace {
        .class BaseClass {}
        .class DerivedClass extends FirstSpace.SecondSpace.BaseClass {}
    }
}
```
#Лекц3

Для створення інтерфейсу директиви не існує. Треба створювати класс з атрибутом інтерфейсу:
```
.class interface IMyInterface {}
//--//extends//--//implements
.namespace FirstSpace {
    .class interface IMyInterface {/*impl*/}
    .class MyClass implements FirstSpace.IMyInterface {}
}
```
Переконались, що namespace "НЕ ІСНУЄ", але існують класи
```
.assembly MyAssembly {}
.namespace MyNamespace{
    .class Program {
        .method public static void main() {
            .entrypoint
            call string  [mscorlib]System.Console::ReadLine()
            pop
            call void  MyNamespace.Program::Voice()
            ret
        }
        .method static void Voice() {
            call void [mscorlib]System.Console::Beep()
            ret
        }
    }
}
```
```
.assembly MyAssembly {}
.method public static void main() {
    .entrypoint
    call string  [mscorlib]System.Console::ReadLine()
    pop
    call void  Voice()
    ret
}
.method static void Voice() {
    call void [mscorlib]System.Console::Beep()
    ret
}
```
:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
```
.assembly MyAssembly {}
.class BaseClass {
    .method void Voice {
        call void [mscorlib]System.Console::Beep()
        ret
    }
}
.class interface IPause() {
    .method void Pause() {}
}
.class DerivedClass extends BaseClass implements IPause {
    .method static void Pause {
        call string [mscorlib] System.Console::ReadLine()
        pop
        ret
    }
}
.method static void Main() {
    call void DerivedClass::Pause()
    call void DerivedClass::Voice()
}
```
![Alt Text](7png.png "7 png")

Статичний конструктор
```
.assembly MyAssembly {}
.class Program {
    .method static void .cctor() {
        call void [mscorlib]System.Console::Clear()
        ret
    }
    .method static void Main() {
        call string [mscorlib]System.Console::ReadLine()
        pop
        ret
        .entrypoint
    }
}
```
Конструктор екземпляру
```
class Instance {
    .method instance void .ctor() {
        .maxstack 1
        ldarg.0
        call instance void [mscorlib]System.Object::ctro()
        ldstr "Constructor was run!"
        call void [mscorlib]System.Console::WriteLine(string)
        ret
    }
    .field public string name
}

.method static void Main(){
    .entrypoint
    .locals init (class Instance obj)
    newobj instance void Instance::.ctor()
    ldstr "pupkin"
    stfld string Instance::name
    ldfld string Instance::name
    call void [mscrorlib]System.Console::WriteLine(string)
    ret
}
```
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
```
class Instance {
    .method instance void .ctor() {
        .maxstack 1
        ldarg.0
        call instance void [mscorlib]System.Object::ctro()
        ldstr "Constructor was run!"
        call void [mscorlib]System.Console::WriteLine(string)
        ret
    }
    .field public string name
}

.method static void Print(){
    ldarg.0
    ldfld string Instance::name
    ldstr "name="
    call void [mscrorlib]System.Console::WriteLine(string)
    call void [mscrorlib]System.Console::WriteLine(string)
    ret
}
```
#Лекц4

Пр 9
```
.method static void Main() {
    .locals (class Instance obj)
    newobj instance void Instance::ctor()
    stloc obj
    ldloc obj
    ldstr "pmpkin"
    stfld string Instance::name
    ldloc obj
    ldfld string Instance::name
    call instance void Instance::Print()
    ret
    .entrypoint
}
```
для доступу до полів можна використовувати властивості

Оголошення властивостей
```
.field private <type> <field.name>
.property instance public <Field_name> {
    .get instance <type> <ClassName>::get_<Field_name>
    .set instance void <ClassName>::set_<Field_name>
}

.method instance <type> <ClassName>::get_<Field_name>() {

}

.method instance void <ClassName>::set_<Field_name>(<type>) {
/*impl*/
}

call <type> <ClassName>::get_<Field_name>()
call void <ClassName>::set_<Field_name>(<type>)
```
```
┌────────────────────┬────────────┬──────────────┬──────────────────────────────┐
│  .Net              │    C#      │   IL         │         Конс нотація IL      │
├────────────────────┼────────────┼──────────────┼──────────────────────────────┤
│  System.Single     │    float   │   float32    │          r4                  │
│  System.Double     │    double  │   float64    │          r8                  │
│  System.Char       │    char    │   char       │          char                │
│  System.Boolean    │    bool    │   bool       │          boolean             │
│  System.String     │    string  │   string     │           ─                  │
│  System.Void       │    void    │   void       │          void                │
└────────────────────┼────────────┼──────────────┼──────────────────────────────┘
```
```
.assembly MyAssembly() {
    .method static void Main() {
        .locals init(int8 a,
                     int16 b,
                     int32 c,
                     int64 d,
                     float32 -1.2,
                     float64 3.14,
                     char chr,
                     string str)
        ldc.i4.s 16
        stloc a
        ldloc a
        call void [mscorlib]System.Console::WriteLine(int32)
    }
}
```
#Лекц5
![Alt Text](8png.png "8 png")

ldc - завантаж. конст
ldc.i4 10

ldstr 0 - занесення текстового рядка
ldstr "Hello"

ldloc - завантаження локальних аргументів
ldarg - завантаження локальних аргументів методів
ldfld - завантаження локальних полів

ldc.i4.0 ... ldc.i4.8
ldloc.0 ... ldloc.8

stloc - зберегти локальні змінні
starg - зберегти локальні аргументи методів
stfld - зберегти локальні поля

pop - очищення вершини стеку

Приклад 1
```
.assembly MyAssembly {}
.method static void Main() {
    ldstr "One"
    ldstr "Two"
    ldstr "Three"
    call void [mscorlib]System.Console::WriteLine(string)
    call void [mscorlib]System.Console::WriteLine(string)
    call void [mscorlib]System.Console::WriteLine(string)
    ret
    .entrypoint
}
```
Приклад 2
```
.assembly MyAssembly {}
.method static void Main() {
    .entrypoint
    .maxstack 2
    ldstr "Enter number:"
    call void [mscorlib]System.Console::WriteLine(string)
    call string [mscorlib]System.Console::ReadLine()
    call int32 [mscorlib]System.Int32::Parse(string)
    ldstr "You enter:"
    call void [mscorlib]System.Console::WriteLine(string)
    call void [mscorlib]System.Console::WriteLine(string)
    ret
}
```
Способи задання локальних змінних

1) .locals init(type <name>, ...)
    ldloc <name?
2) .localc init([0]type<name1>,
                [1]type<name2>, ...)
    ldloc.0 ... ldloc.3 ... ldloc.9

###Операції:
-1) Арифметичні: add(+), sub(-), mul(*), div(/), rem(%)
-2) Порівняння: ceq(=), clt(<), cgt(>)
-3) Переходу на мітку: btrue <label>, bfalse <label>, br<label
    beq(=), blt(<), ble(<=), bge(>)
-4) Упаковка даних:
    box type
    unbox type
```
WriteLine(string, object, object, ...)
WriteLine("x={0}", )

.locals init (int32, int32)
ldc.i4.1
stloc.0
ldc.i4.2
stloc.1
ldloc.0
ldloc.1
ceq
btrue _label
_label:
```
#Лекц6
...
Пропущено пол часа пары
...
if, switch
for, while, foreach - в CIL не существуют!
>"Они придуманы массонами!" - заявил Юрий Адамович

Реалізація умовного оператора if

Пр.2
```
.assembly MyAssembly {}
.method static void Main() {
    .maxstack 2
    .entrypoint
    .locals init(int32 x)
    ldc.i4 15 //x
    stloc x
    ldloc x
    ldc.i4 2
    rem
    ldc.i4 0
    ceq
    bfalse _odd //или 0dd
    ldstr "{0} - Even"
    ldloc x
    box int32
    call void [mscorlib]System.Console::WriteLine(string, object)
    ret

    _odd:
    ldstr "{0} - Odd"
    ldloc x
    box int32
    call void [mscorlib]System.Console::WriteLine(string, object)
    ret
}
```
Реалізація умовного оператора switch

Пр.3
```
.assembly MyAssembly {}
.method static void Main() {
    .maxstack 1
    .entrypoint
    .locals init(int32 x)
    ldc.i4 1 //x
    switch(_0, _1, _2)
    _0:
        ldstr "Case 0"
        call void [mscorlib]System.Console::WriteLine(string, object)
        ret
    _1:
        ldstr "Case 1"
        call void [mscorlib]System.Console::WriteLine(string, object)
        ret
    _2:
        ldstr "Case 2"
        call void [mscorlib]System.Console::WriteLine(string, object)
        ret
    ret
}
```
Реалізація оператора циклу for
```
int sum = 0;
for(int i = 1; i <= 5; i++)
    sum += i;
```
Пр.4
```
.assembly MyAssembly {}
.method static void Main() {
    .maxstack 1
    .entrypoint
    .locals init(int32 i, int32 sum)
    _Inicializaton:
        ldc.i4 1
        stloc i
    _Condition:
        ldloc i
        ldc.i4 5
        blt _Loop
        br _Exit
    _Loop:
        ldloc sum
        ldloc i
        add
        stloc summ
    _Incrementation:
        ldloc i
        ldloc.i4 1
        add
        stloc i
        br _Condition
    _Exit:
        ret
}
```
Реалізація оператору циклу while
```
int sum = 0;
int i = 1;
while(i <= 5) {
    sum += i;
    i++;
}
```
Пр.5
```
.assembly MyAssembly {}
.method static void Main() {
    .maxstack 1
    .entrypoint
    .locals init(int32 i, int32 sum)
    _Inicializaton:
        ldc.i4 1
        stloc i
    _Condition:
        ldloc i
        ldc.i4 5
        ble _Loop
        br _Exit
    _Loop:
        ldloc sum
        ldloc i
        add
        stloc summ
        ldloc i
        ldloc.i4 1
        add
        stloc i
        br _Condition
    _Exit:
        ret
}
```
Массиви створюються за допомогою команди new arr[], назви змінної та ініціалізації

Реалізація оператору циклу foreach
```
int[] numbers = {5,10,15}
foreach(int i in numbers)
    Console.WriteLine(i);
```
Пр.6
```
.assembly MyAssembly {}
.method static void Main() {
    .maxstack 1
    .entrypoint
    .locals init(int32[] numbers, int32 i)
    ldc.i4 3
    newarr int32
    stloc numbers

    //Присв значень
    ldloc numbers
    ldc.i4 2
    ldc.i4 15
    stelem.i4

    ldloc numbers
    ldc.i4 1
    ldc.i4 10
    stelem.i4

    ldloc numbers
    ldc.i4 0
    ldc.i4 5
    stelem.i4

    _Condition:
    ldloc numbers
    ldloc i
    ldlen
    bel _Loop
    br _Exit

    _Loop:
    ldloc i
    ldc.i4 1
    add
    ldloc numbers
    ldloc i
    ldelem.i4
    call void [mscorlib]System.Console::WriteLine(int32)
    br _Condition

    _Exit:
    ret
}
```
#Лекц7
![Alt Text](9png.png "9 png")

Технології ЗПП:
```
- "Водяні знаки"
- Відбиток пальців
- Перевірка цілістності
- "Клієнт-сервер"
- DRM
```

Унеможливлення декомпіляції

1. Додання атрибутів
```
using System;
using System.Runtaime.CompilerServices;
[assembly: SuppressILdasmAttribute()]
class Program() {
    ...
}
```
2. редагування Assemblyinfo.cs
```
[assembly: SupressILdasm]
```

#Лекц8
Крипография

Литература:
<ul>
<li>1. Смарт Н. "Криптография" 2005 - 528с.</li>
<li>2. Вендо Мао "Современная криптография" 2002 - 728с.</li>
<li>3. XF "Основы Криптологии" 2006 - 571с</li>
<li>4. Саломаа А. "Криптография с открытым ключем" 1995 - 768с.</li>
<li>5. Каутинхо С. "Введение в теорию чисел, алгоритмы RSA" 2001 - 368c.</li>
</ul>

Модель симметричної криптосистеми
<ul>
<li>І. Криптографія і ії основні пункти</li>
<li>ІІ. Модель Шеннона криптографічної системи</li>
<li>ІІІ. Принцип Керкхоффса</li>
<li>IV. Фізична оцінка верхньої границі ключа</li>
</ul>

![Alt Text](10png.png "10 png")
![Alt Text](11png.png "11 png")
###Шифр Цезаря
![Alt Text](12png.png "12 png")

```
c:\> cezar.exe -e filenametxt -key
```
Вимога до форми лабораторної
![Alt Text](13png.png "13 png")

Симетричні та десиметричні методи шифрування
![Alt Text](14png.png "14 png")

#Лекц9

Шеннон у 1947 р. виклав таку криптографічну систему:
Від Аліси до Боба передається шифроване повідомлення, Боб отримує і розшифровує повідомлення
![Alt Text](15png.png "15 png")
Ключ - дані для шифрування/розшифрування
Шифр - сукупність алгоритмів шифрування і розшифрування
![Alt Text](16png.png "16 png")

>"Принцип Керкхоффса - єдиним секретом повинен бути ключ, алгоритм повинен бути відкритим"

Чувак умножил Температуру солнца на мощность солнца и на время существования солнца и на k=1,4*10^-23
E=P*t = 3,86*10^26 * 3*10^17 = 10^44
kT = 1,4*10^-27 * 10^6 = 1,4*10^-17
N = E/(kT) = 10^44 * 3*10^-17 = 10^60
получил n = 60 - таких размеров ключ невозможно физически перебрать


В классической криптографии есть только два способа зашифровать сообщение:
заменить символ и поменять местами символы
Перестановочные шифры:
<ul>
<li>Маршрутная перестановка</li>
<li>Матричный перестановка</li>
<li>... перестановка</li>
</ul>

Шифры замены:
<ul>
<li>Простая замена</li>
<li>Сложная замена</li>
<li>Блочная замена</li>
<li>Полиалфавитная замена</li>
</ul>

![Alt Text](17png.png "17 png")

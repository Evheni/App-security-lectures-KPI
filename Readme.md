Лекц1
Короткі відомості
1.Common language runtime (CLR)
2.Сбірки дотнету
3.Структура збірки
4.Перегляд вмісту збірок
5.Атрибути збірок

1)Компіляція
![Alt Text](1png.png "One png")

+ Оптимізація для процесора
+ Взаємодія компонентів (COM/DCOM)
- Моноплатформність
- Нестабільність
- Незахищеність ОС
- Крах DLL

2)Інтерпретація

+ Багатоплатформність
+ Стабільність
+ Захищеність
+ Спрощення розгортання
+ Оптимізація під процесор
+ Взаємодія компонентів

![Alt Text](2png.png "Two png")

Лекц2
Збірка - одиниця в дотнет. Можем буду в .ехе(містять точку входа), .dll
![Alt Text](3png.png "3 png")
![Alt Text](4png.png "4 png")
![Alt Text](5png.png "5 png")
а) Найпростіший варіант створення збірок у вигляді одного файлу
![Alt Text](6png.png "6 png")

ІІІ. Перегляд структури збірок
ildasm.exe - утиліта для "декомпіляції"
ilasm.exe - утиліта для компіляції IL-коду

Перегляд збірки:
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

ІV. Використання збірок

Атрибути збірок можуть задаватись в файлі Assemblyinfo.cs, в якому можна задавати
цілий ряд додаткових параметрів збірок:
Assembly Company - назва компанії
Assembly Copyright - інформація про авторів
Assembly Description - опис збірок
Assembly Version і т.д.


I. Структура програми
II. Визначення збірки і посилань на збірки, що розділяються
ІІІ. Визов методів
ІV. Визначення просторів імен, класів і ...
V. Визначення полів, конструкторів і властивостей класів

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
```
.namespace FirstSpace {
    .namespace SecondSpace {
        .class BaseClass {}
        .class DerivedClass extends FirstSpace.SecondSpace.BaseClass {}
    }
}
```
ЛЕКЦИЯ 3

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
Лекц 4

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
Лекц 5
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

Операції:
1) Арифметичні: add(+), sub(-), mul(*), div(/), rem(%)
2) Порівняння: ceq(=), clt(<), cgt(>)
3) Переходу на мітку: btrue <label>, bfalse <label>, br<label
    beq(=), blt(<), ble(<=), bge(>)
4) Упаковка даних:
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
Лекция 6
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
    // _Inicializaton:
    //     ldc.i4 1
    //     stloc i
    // _Condition:
    //     ldloc i
    //     ldc.i4 5
    //     ble _Loop
    //     br _Exit
    // _Loop:
    //     ldloc sum
    //     ldloc i
    //     add
    //     stloc summ
    //     ldloc i
    //     ldloc.i4 1
    //     add
    //     stloc i
    //     br _Condition
    // _Exit:
    //     ret
}
```

# Задача "Понимание JVM"
## Код для исследования
```java
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```
## Описание
JVM обратится к ClassLoader с требованием загрузить класс JvmComprehension.
Полный путь запроса:  Application ClassLoader -> Platform ClassLoader -> Bootstrap ClassLoader -> Platform ClassLoader -> Application ClassLoader -> Класс JvmComprehension найден и помещается в Metaspace.
В момент вызова метода main в стэке создается фрейм.

1. Внутри фрейма main создается переменная i типа int со значением 1
2. В куче создается объект типа Object и во фрейме main создается переменная o, которой присваивается ссылка на этот объект.
3. В куче создается объект типа Integer со значением 2 и во фрейме main создается переменная ii, которой присваивается ссылка на этот объект.
4. В стэке создается фрейм printAll, в котором создаются переменные o, i, ii, которым передаются ссылки из кучи.
5. В куче создается объект типа Integer со значением 700 и во фрейме printAll создается переменная uselessVar, которой присваивается ссылка на этот объект.
6. В стэке создается фрейм println, которому передаются ссылки на переменные o, i и ii из кучи.
7. В стэке создается фрейм println, которому передается ссылка на созданный в куче объект типа String со значением "finished".

В ходе работы программы отрабатывает Garbage Collector.

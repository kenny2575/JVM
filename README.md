# Задача "Понимание JVM"
## Код для исследования
```java
    // ClassLoader подгружает классы JvmComprehension (Application) и String, Object, Integer, System (Bootstrap).
    // Запускается связывание и инициализация подгруженных классов
    // В куче создаётся статический объект out типа PrintStream, ему присваивается null
    public class JvmComprehension {
    // В области Metaspace выделяется память для подгруженных классов
    public static void main(String[] args) {
    // В стеке JvmComprehension создаётся фрейм метода main
    int i = 1;                      // 1
    // Во фрейме main созраняется переменная i типа int и её значение 1
    Object o = new Object();        // 2
    // В куче создаётся новый объект типа Object, во фрейм main добавляется переменная о (ссылка на этот объект)
    Integer ii = 2;                 // 3
    // В куче создаётся новый объект типа Integer, содержащий примитив int со значением 1, во фрейм main добавляется переменная ii (ссылка на этот объект)
    printAll(o, i, ii);             // 4
    // В стеке JvmComprehension создаётся фрейм метода printAll
    System.out.println("finished"); // 7
    // В куче создаётся объект типа String, хранящий переданную строку "finished" (param)
    // В стеке PrintStream создаётся фрейм метода println
    // Во фрейме println создаётся переменная со ссылкой на объект, содержащий ссылку на param
    // Стек PrintStream освобождается для GC
    }

    private static void printAll(Object o, int i, Integer ii) {
    // Во фрейме printAll создаётся переменная о типа Object со ссылкой переменной о стека main на объект Object в куче
    // Во фрейме printAll создаётся переменная i типа int и её значение, прочитанное из переменной i стека main
    // Во фрейме printAll создаётся переменная ii типа Integer со ссылкой переменной ii стека main на объект Integer в куче
        Integer uselessVar = 700;                   // 5
        // В куче создаётся новый объект типа Integer, содержащий примитив int со значением 700,
        //  во фрейм printAll добавляется переменная uselessVar (ссылка на этот объект)
        System.out.println(o.toString() + i + ii);  // 6
        // В куче создаётся новый объект типа String
        // Во фрейме printAll создаётся переменная со ссылкой на этот объект
        // В стеке Object создаётся фрейм метода toString
        // В куче создаётся новый объект типа String (пусть s1) для хранения возращаемого значения от toString()
        // Переменной arg присваивается ссылка на этот объект
        // Стек Object и объект s1 освобождаются для сборщика мусора
        // В стеке String создастся фрейм concat
        // Во фрейме concat создаётся переменная (пусть this) со ссылкой на s1
        // В стеке String создастся фрейм valueOf
        // Во фрейме valueOf создаётся переменная i со ссылкой на объект i фрейма printAll стека JvmComprehension
        // В куче создаётся новый объект типа String (пусть val) для хранения возращаемого значения от valueOf()
        // Во фрейме concat создаётся переменная str со ссылкой на этот объект
        // Фрейм valueOf освобождается для сборщика мусора
        // В куче создаётся объект типа String (пусть sConcat) для хранения возвращаемого значения concat()
        // Фрейм concat и объекты val освобождаются для сборщика мусора
        // Переменная arg фрейма printAll ссылается на объект sConcat, освобождая прошлую ссылку для GС
        // В стеке String создастся НОВЫЙ фрейм concat
        // Во фрейме concat создаётся переменная (пусть this) со ссылкой на sConcat
        // В стеке String создастся НОВЫЙ фрейм valueOf
        // Во фрейме valueOf создаётся переменная obj со ссылкой на объект ii фрейма printAll стека JvmComprehension
        // В стеке Object создаётся фрейм метода toString
        // В куче создаётся новый объект типа String (пусть tos) для хранения возращаемого значения от toString()
        // Стек Object освобождается для GC
        // В куче создаётся новый объект типа String (пусть val) для хранения возращаемого значения от valueOf()
        // Во фрейме concat создаётся переменная str со ссылкой на этот объект
        // Стек String освобождается для сборщика мусора
        // В куче создаётся объект типа String (пусть sConcat) для хранения возвращаемого значения concat()
        // Фрейм concat и объект val освобождаются для сборщика мусора
        // Переменная arg фрейма printAll ссылается на объект sConcat, освобождая прошлую ссылку для GC
        // В стеке PrintStream создаётся фрейм метода println
        // Во фрейме println создаётся переменная x со ссылкой на объект arg фрейма printAll стека JvmComprehension
        // Магия печатает x в консоль
        // Стек PrintStream освобождается для GC
    }
    // Фрейм printAll освобождается для GC
}
// Стек JvmComprehension освобождается для GC
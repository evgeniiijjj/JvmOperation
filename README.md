# Порядок работы JVM при выполнении следующего кода:

    public class JvmComprehension {

        public static void main(String[] args) {
            int i = 1;                      // 9
            Object o = new Object();        // 10
            Integer ii = 2;                 // 11
            printAll(o, i, ii);             // 12
            System.out.println("finished"); // 23
        }

        private static void printAll(Object o, int i, Integer ii) {
            Integer uselessVar = 700;                   // 14
            System.out.println(o.toString() + i + ii);  // 16
        }
    }
1. Запускается JVM, выделяется память для ее работы;
2. Вызывается Application ClassLoader с командой загрузить главный класс JvmComprehension;
3. Application ClassLoader переадресовывает эту команду Platform ClassLoader;
4. Platform ClassLoader адресует данную команду Bootstrap ClassLoader;
5. Bootstrap ClassLoader пытается найти данный класс в списке базовых ява классов, если не находит, уведомляет Platform ClassLoader;
6. Platform ClassLoader пытается найти данный класс в списке базовых, платформоориентированных классов, если не находит, уведомляет Application ClassLoader;
7. Application ClassLoader в случае, если определен пользовательский ClassLoader адресует команду ему, иначе загружает данный класс сам в область памяти Metaspace;
8. Вызывается метод main, в результате в области памяти Stack выделяется память - фрейм;
9. В фрейме выделяется память под локальную переменную - примитив int i, инициализируется значением 1;
10. Повтор шагов 2 - 7 для класса Object, создается объект o класса Object, который занимает память в Heap, New Generation, в фрейме выделяется память для хранения ссылки на объект;
11. Повтор шагов 2 - 7 для класса Integer, создается объект ii класса Integer, который занимает память в Heap, New Generation, в фрейме выделяется память для хранения ссылки на объект;
12. Вызывается метод printAll, в области памяти Stack выделяется фрейм для данного метода;
13. В фрейме выделяется память для хранения локальных переменных: примитива int i и ссылок на объекты Object o и Integer ii;
14. Создается объект uselessVar класса Integer, который сохраняется в Heap, New Generation;
16. Вызывается метод println, в области памяти Stack выделяется фрейм для данного метода;
17. Вызывается метод toString, в области памяти Stack выделяется фрейм для данного метода;
18. Метод toString завершает работу, память в Stack, занятая данным фреймом, высвобождается;
19. Метод println завершает работу, память в Stack, занятая данным фреймом, высвобождается;
20. Метод printAll завершает работу, память в Stack, занятая данным фреймом, высвобождается;
21. Объект uselessVar становиться недостижимым и будет очишен Garbage Сollector при следующей уборке мусора;
22. Вызывается метод println в области памяти Stack выделяется фрейм для данного метода;
23. Повтор шагов 2 - 7 для класса String, создается объект "finished" класса String, который занимает память в Heap, String Pool, в фрейме выделяется память для хранения ссылки на объект;
24. Метод println завершает работу, память в Stack, занятая данным фреймом высвобождается;
25. Объект "finished" становится недостижимым;
26. Метод main завершает работу, память в Stack, занятая данным фреймом высвобождается;
27. Объекты Object o, Integer ii становятся недостижимыми;
28. JVM завершает работу, память высвобождается.

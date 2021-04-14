# Потоки в Java
Поток в Java представлен в виде экземпляра класса java.lang.Thread. Стоит сразу понимать, что экземпляры класса Thread в Java сами по себе не являются потоками. Это лишь своего рода API для низкоуровневых потоков, которыми управляет JVM и операционная система. Когда при помощи java launcher'а мы запускаем JVM, она создает главный поток с именем main и ещё несколько служебных потоков.

**Создание потока**

Cоздать своего наследника. Например:
```
public class HelloWorld{
    public static class MyThread extends Thread {
        @Override
        public void run() {
            System.out.println("Hello, World!");
        }
    }

    public static void main(String []args){
        Thread thread = new MyThread();
        thread.start();
    }
}
```

Как видим, запуск задачи выполняется в методе run, а запуск потока в методе start. Не стоит их путать, т.к. если мы запустим метод run напрямую, никакой новый поток не будет запущен. Именно метод start просит JVM создать новый поток.

Вариант с потомком от Thread плох уже тем, что мы в иерархию классов включаем Thread. Второй минус — мы начинаем нарушать принцип "Единственной ответственности" SOLID, т.к. наш класс становится одновременно ответственным и за управление потоком и за некоторую задачу, которая должна выполняться в этом потоке.

Как же правильно? Ответ находится в том самом методе run, который мы переопределяем:
```
public void run() {
	if (target != null) {
		target.run();
	}
}
```
Здесь target — это некоторый java.lang.Runnable, который мы можем передать для Thread при создании экземпляра класса. Поэтому, мы можем сделать так:
```
public class HelloWorld{
    public static void main(String []args){
        Runnable task = new Runnable() {
            public void run() {
                System.out.println("Hello, World!");
            }
        };
        Thread thread = new Thread(task);
        thread.start();
    }
}
```
или
```
public static void main(String []args){
	Runnable task = () -> {
		System.out.println("Hello, World!");
	};
	Thread thread = new Thread(task);
	thread.start();
}
```
## java.util.concurrent.Callable
Оказывается, у java.lang.Runnable есть брат и зовут его java.util.concurrent.Callable и появился он на свет в Java 1.5. В чём же различия? Если приглядеться к JavaDoc этого интерфейса, мы видим, что в отличие от Runnable, новый интерфейс объявляет метод call, который возвращает результат. Кроме того, по умолчанию он throws Exception. То есть избавляет нас от необходимости на проверяемые исключения писать try-catch блоки. Уже неплохо, правда?

Теперь у нас есть вместо Runnable новый task:
```
Callable task = () -> {
	return "Hello, World!";
};
```
Но что с ним делать? Зачем нам вообще задача, выполняемая в потоке, которая возвращает результат? Очевидно, что в дальнейшем мы рассчитываем получить результат действий, которыев в будущем будут выполнены. Будущее по-английский — Future. И интерфейс есть с точно таким же именем: java.util.concurrent.Future
## java.util.concurrent.Future

Интерфейс java.util.concurrent.Future описывает API для работы с задачами, результат которых мы планируем получить в будущем: методы получения результата, методы проверки статуса.

Для Future нас интересует его реализация java.util.concurrent.FutureTask. То есть это Task, который будет выполнен во Future. Чем эта реализация ещё интересна, так это тем, что она реализует и Runnable. Можно считать это своего рода адаптером старой модели работы с задачами в потоках и новой модели (новой в том смысле, что она появилась в java 1.5).

Вот пример:
```
import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

public class HelloWorld {

    public static void main(String []args) throws Exception {
        Callable task = () -> {
            return "Hello, World!";
        };
        FutureTask<String> future = new FutureTask<>(task);
        new Thread(future).start();
        System.out.println(future.get());
    }
}
```
## Пример программы 
```
package com.company;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class Main {

    //функция, которую нужно выполнить в отдельном потоке
    public static String executePost(String targetURL, String urlParameters) { return "any";}

    static class SomeThing implements Callable<String>
    {
        String targetURL;
        String urlParameters;

        SomeThing(String url, String params) {
            this.targetURL = url;
            this.urlParameters = params;
        }

        @Override
        public String call() throws Exception {
            System.out.println("Привет из побочного потока!");
            return executePost(targetURL, urlParameters);
        }
    }

    private static SomeThing mThing;

    public static void main(String[] args) {

        String forSearch = "Radioactive";
        mThing = new SomeThing("https://itunes.apple.com/search?term=" + forSearch, "");

        FutureTask<String> task = new FutureTask<>(mThing);
        Thread myThready = new Thread(task);
        myThready.start();

        try {
            String quest = task.get();
            System.out.println(quest);
        } catch (InterruptedException | ExecutionException e) {
            //something
        }
    }
}

```
# Источники
- https://javarush.ru/groups/posts/2065-threadom-java-ne-isportishjh--chastjh-iv---callable-future-i-druzjhja
- https://ru.stackoverflow.com/questions/847345/Выполнение-функции-в-отдельном-потоке-java

# потоки в Java
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

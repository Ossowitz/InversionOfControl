# SPRING FRAMEWORK

```java
/**
 * @// TODO: 14.04.2023  
 */
```

# Inversion of Control (IoC)

## Проблема #1:

### Сильная зависимость:

MusicPlayer сильно зависит от ClassicalMusic. <br/>
Класс MusicPlayer "заточен" на работу только с ClassicalMusic.

```java
class ClassicalMusic {
    // Code to access classical music 
}

class MusicPlayer {
    private ClassicalMusic classicalMusic;

    public void playMusic() {
        // Code to play classical music 
    }
}
```

## Решение:

Использовать интерфейс (или абстрактный класс),
который бы обобщал различные музыкальные жанры.

```java
interface Music {
    // The code that is needed to access any genre of music
}

class ClassicalMusic implements Music {
    // Code to access classical music
}

class RockMusic implements Music {
    // code to access rock music
}

class MusicPlayer {
    private Music music;

    public void playMusic() {
        music = new ClassicalMusic();
        // or
        music = new RockMusic();

        // Code to play music
    }
}
```

## Проблема #2:

### Слабая зависимость:

Объекты создаются вручную. Мы хотим вынести эти детали в конфигурационный файл,
а не лезть каждый раз в код (и перекомпилировать его) для того, чтобы поменять объект.

```java
interface Music {
    // The code that is needed to access any genre of music
}

class ClassicalMusic implements Music {
    // Code to access classical music
}

class RockMusic implements Music {
    // code to access rock music
}

class MusicPlayer {
    private Music music;

    public void playMusic() {
        music = new ClassicalMusic();
        // or
        music = new RockMusic();

        // Code to play music
    }
}
```

## Решение:

Использовать Spring Framework, который сам создаст необходимые объекты (бины)
согласно конфигурационному файлу.

## Bean

• Это просто Java-объект, который создаётся и управляется Spring Container. <br/>
• Когда Java-объекты создаются с помощью Spring'а, они называются бинами (beans). <br/>
• Бины создаются из Java-классов (так же, как и обычные объекты).

```xml

<bean>id="testBean"
    class="us.ossowitz.springcourse.TestBean">
    <constructor-arg value="Ilya"/>
</bean>
```

• id - идентификатор бина <br/>
• class - полное имя класса

## Проблема #3:

MusicPlayer сам создаёт свои зависимости. Это архитектурно неправильно - противоречит принципу IoC.

```java
interface Music {
    // The code that is needed to access any genre of music
}

class ClassicalMusic implements Music {
    // Code to access classical music
}

class RockMusic implements Music {
    // code to access rock music
}

class MusicPlayer {
    private Music music;

    public void playMusic() {
        music = new ClassicalMusic();
        // or
        music = new RockMusic();

        // Code to play music
    }
}
```

## Решение:

Использовать принцип IoC.

# Inversion of Control (IoC)

```java
interface Music {
    // The code that is needed to access any genre of music
}

class ClassicalMusic implements Music {
    // Code to access classical music
}

class RockMusic implements Music {
    // code to access rock music
}

class MusicPlayer {
    private Music music;

    public void playMusic() {
        music = new ClassicalMusic();
        // or
        music = new RockMusic();

        // Code to play music
    }
}
```

• MusicPlayer зависит от классов, реализующих интерфейс Music. <br/>
• MusicPlayer сам создаёт объект ClassicalMusic. <br/>
• Вместо этого мы хотим передавать объект ClassicalMusic внутрь
MusicPlayer - это и называется инверсией управления (IoC).

### Инверсия управления - это такой архитектурный подход, когда сущность не сама создаёт свои зависимости, а когда этой сущности зависимости поставляются извне.

## Последняя проблема

```java
class MusicPlayer {
    private Music music;

    // Зависимость внедряется извне (IoC)
    public MusicPlayer(Music music) {
        this.music = music;
    }

    public void playMusic() {
        // Больше не создаём объекты!

        // ... Код для воспроизведения музыки
    }
}
```

Объект, который мы хотим внедрить в MusicPlayer необходимо где-то создать.

```java
class UseMusicPlayer {
    public static void main(String[] args) {
        MusicPlayer musicPlayer = new MusicPlayer(new ClassicalMusic());
    }
}
```

**Эту проблему можно решить с помощью внедрения зависимостей (Dependency Injection).** <br/>
Этой задачей тоже занимается Spring Framework.

## Spring можно конфигурировать с помощью:

• XML-файла конфигураций (старый способ, но многие существующие приложения
до сих пор его используют). <br/>
• Java-аннотаций и немного XML (современный способ). <br/>
• Вся конфигурация на Java-коде (современный).

## Способы внедрения зависимостей

• Через конструктор. <br/>
• Через setter. <br/>
• Есть множество конфигураций того, как внедрять (scope, factory method и т.д.). <br/>
• Можно внедрять через XML, аннотации или Java-код. <br/>
• Процесс внедрения можно автоматизировать (Autowiring).

### Внедрение (Injection) с помощью конструктора

```xml

<bean id="musicBean"
      class="us.ossowitz.springcourse.ClassicalMusic">
</bean>

<bean id="musicPlayer"
      class="us.ossowitz.springcourse.MusicPlayer">
<constructor-arg ref="musicBean"/>
</bean>
```

### Использование

```java
MusicPlayer musicPlayer=context.getBean("musicPlayer",MusicPlayer.class);
        musicPlayer.playMusic();
```

### Внедрение зависимостей через setter

```java
public void setMusic(Music music){
        this.music=music;
        }
```

```xml

<bean id="musicPlayer"
      class="us.ossowitz.springcourse.MusicPlayer">
    <property name="music" ref="musicBean"/>
</bean>
```

### Внедрение простых значений

```java
private String name;
private int volume;

public void setName(String name){
        this.name=name;
        }

public void setVolume(int volume){
        this.volume=volume;
        }
```

```xml

<property name="name" value="Some name"/>
<property name="volume" value="50"/>
```

### Внедрение простых значений из внешнего файла

• Не хотим каждый раз лезть в applicationContext.xml <br/>
• Хотим все простые значения указать в одном файле.

1. Создаём файл, расширения .properties <br/>
   Содержимое: <br/>

```text
musicPlayer.name=Some name
musicPlayer.volume=70
```

2. В applicationContext.xml подтягиваем путь до файла musicPlayer.properties: <br/>

```xml

<context:property-placeholder location="classpath:musicPlayer.properties"/>
```

3. Внедряем зависимости: <br/>

```xml

<property name="name" value="${musicPlayer.name}"/>
<property name="volume" value="${musicPlayer.volume}"/>
```

## Scope

**Scope задаёт то, как Spring будет создавать ваши бины. <br/>
Определяет жизненный цикл бина и возможное количество создаваемых бинов.**

### Singleton

**Scope, который используется по умолчанию**

```xml
<bean id="musicBean"
      class="us.ossowitz.springcourse.MusicPlayer">
</bean>
```

• По умолчанию создаётся объект (он создаётся до вызова метода getBean()). <br/>
• При всех вызовах getBean() возвращается ссылка на один и тот же единственный объект. <br/>
• Подходит для stateless-объектов (объекты, состояние
которых нам менять не приходится. Потому что если мы будем изменять состояние
у Singleton бина, столкнёмся с проблемой).

### Prototype

**Scope, который каждый раз создаёт новый объект при вызове getBean()**

• Такой бин создаётся только после обращения к Spring Container-у 
с помощью метода getBean(). <br/>
• Для каждого такого обращения создаётся новый бин в Spring Container. <br/>
• Чаще всего используется тогда, когда у нашего бина есть изменяемые состояния (stateful). <br/>

## Жизненный цикл бина (Bean lifecycle)

![img.png](img.png)

### init-method & destroy-method

#### init-method
• Метод, который запускается в ходе инициализации бина. <br/>
• Инициализация ресурсов, обращение к внешним файлам, запуск БД.

#### destroy-method
• Метод, который запускается в ходе уничтожения бина (при 
завершении работы приложения). <br/>
• Очищение ресурсов, закрытие потоков ввода-вывода,
закрытие доступа к БД.
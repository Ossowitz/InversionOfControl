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

# Решение:

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

# Проблема #2:

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

# Решение:

Использовать Spring Framework, который сам создаст необходимые объекты (бины)
согласно конфигурационному файлу.

# Bean

• Это просто Java-объект. <br/>
• Когда Java-объекты создаются с помощью Spring'а, они называются бинами (beans). <br/>
• Бины создаются из Java-классов (так же, как и обычные объекты).

```xml

<bean>id="testBean"
    class="us.ossowitz.springcourse.TestBean">
    <constructor-arg value="Ilya"/>
</bean>
```

# Проблема #3:

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

# Решение:

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

# Последняя проблема
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


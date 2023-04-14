# Проблема #1:

### Сильная зависимость:

MusicPlayer сильно зависит от ClassicalMusic.
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

Использовать интерфейс (или абстрактный класс), который бы обобщал различные музыкальные жанры.

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

Объекты создаются вручную. Мы хотим вынести эти детали в конфигурационный файл, а не лезть каждый раз в код (и перекомпилировать его) для того, чтобы поменять объект

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
Использовать Spring Framework, который сам создаст необходимые объекты (бины) согласно конфигурационному файлу.
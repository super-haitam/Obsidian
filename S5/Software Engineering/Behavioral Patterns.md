
>[!note] Definition
>They focus on how classes interact with each other and how they share responsability between them.
>- **Goal**: They characterize complex control flow and communication pathways that run *at runtime* between objects.

# Pattern #1: State
The **State Pattern** is a behavioral pattern that allows the object to change its behavior whenever its internal state changes. It appears as if the object has changed its class.

Instead of having massive `switch` or `if-else` cases depending on that internal state, we divide the class into multiple classes, each with a specific behavior, matching one of class's under a certain value of that internal state.
## Java Implementation Example
Here is how you can implement a clean State pattern for an Audio Player in Java:
### The State Interface
```java
// 1. The State Interface
public interface AudioState {
    void pressPlay(AudioPlayer player);
    void pressLock(AudioPlayer player);
}
```
### The Context Class
```java
// 2. The Context Class (The Audio Player)
public class AudioPlayer {
    private AudioState currentState;

    public AudioPlayer() {
        // Start in the Ready/Stopped state
        this.currentState = new StoppedState();
    }

    public void setState(AudioState state) {
        this.currentState = state;
    }

    // Delegate actions to the current state
    public void pressPlay() {
        currentState.pressPlay(this);
    }

    public void pressLock() {
        currentState.pressLock(this);
    }
}
```
### Concrete States
```java
// 3. Concrete State A: Stopped State
public class StoppedState implements AudioState {
    @Override
    public void pressPlay(AudioPlayer player) {
        System.out.println("Starting playback... Switching to Playing state.");
        player.setState(new PlayingState());
    }

    @Override
    public void pressLock(AudioPlayer player) {
        System.out.println("Locking player while stopped.");
        player.setState(new LockedState());
    }
}

// 4. Concrete State B: Playing State
public class PlayingState implements AudioState {
    @Override
    public void pressPlay(AudioPlayer player) {
        System.out.println("Pausing playback... Switching to Paused state.");
        player.setState(new PausedState());
    }

    @Override
    public void pressLock(AudioPlayer player) {
        System.out.println("Cannot lock device while playing! Stop first.");
    }
}

// 5. Concrete State C: Paused State
public class PausedState implements AudioState {
    @Override
    public void pressPlay(AudioPlayer player) {
        System.out.println("Resuming playback... Switching to Playing state.");
        player.setState(new PlayingState());
    }

    @Override
    public void pressLock(AudioPlayer player) {
        System.out.println("Locking player from paused state.");
        player.setState(new LockedState());
    }
}

// 6. Concrete State D: Locked State
public class LockedState implements AudioState {
    @Override
    public void pressPlay(AudioPlayer player) {
        System.out.println("Device is locked! Unlock first.");
    }

    @Override
    public void pressLock(AudioPlayer player) {
        System.out.println("Unlocking device...");
        player.setState(new StoppedState());
    }
}
```
# Pattern #2: Observer
The **Observer Pattern** is about linking a certain **Subject** class to a number of **Observer** objects, to which updates will happen to whenever **Subject** wishes it to be necessary. It is just like a radio station emitting waves to parabolas whenever a new show begins.
Whenever the Subject's state changes, it automatically broadcasts a notifiction to all registered observers *without needing to know who or what they are*.
A few typical **Subject** methods:
- `void register(Observer o)`: Add an observer to the list of linked observers.
- `void remove(Observer o)`: Remove observer from the list.
- `void notifiy()`: Call `update()` on all linked observers.

## The Classic Analogy: A Weather Station
Imagine a meteorological station that measures temperature, humidity, and barometric pressure.
- **The Subject**: The central Weather Station
- **The Observers**: Display units - for example *Current Conditions* display, a *Statistics* display, and a *Forecast* display.
- **The Mechanism**: Whenever the Weather Station gets sensory data, it pushed the change to all devices connected to it. A new device "Mobile App Notification" for example doesn't need to change the Weather Station code, no, it just plugs itself and that's it.

### Java Implementation Exemple
Here is a classic implementation of the Observer pattern in Java:
#### Imports
```java
import java.util.ArrayList;
import java.util.List;
```
#### Observer (Interface)
```java
// 1. The Observer Interface
public interface Observer {
    void update(float temperature, float humidity, float pressure);
}
```
#### Subject (Interface)
```java
// 2. The Subject Interface
public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}
```
#### Concrete Subject
```java
// 3. Concrete Subject: WeatherData
public class WeatherData implements Subject {
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData() {
        observers = new ArrayList<>();
    }

    @Override
    public void registerObserver(Observer o) {
        observers.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature, humidity, pressure);
        }
    }

    // Called whenever new weather measurements come in
    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        notifyObservers();
    }
}
```
#### Concrete Observers
```java
// 4. Concrete Observer A: Current Conditions Display
public class CurrentConditionsDisplay implements Observer {
    private float temperature;
    private float humidity;

    public CurrentConditionsDisplay(Subject weatherData) {
        weatherData.registerObserver(this); // Automatically subscribe upon creation
    }

    @Override
    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() {
        System.out.println("Current conditions: " + temperature + "°C and " + humidity + "% humidity");
    }
}

// 5. Concrete Observer B: Statistics Display
public class StatisticsDisplay implements Observer {
    private float maxTemp = 0.0f;
    private float minTemp = 200;
    private int numReadings = 0;
    private float tempSum = 0.0f;

    public StatisticsDisplay(Subject weatherData) {
        weatherData.registerObserver(this);
    }

    @Override
    public void update(float temperature, float humidity, float pressure) {
        tempSum += temperature;
        numReadings++;
        if (temperature > maxTemp) maxTemp = temperature;
        if (temperature < minTemp) minTemp = temperature;
        display();
    }

    public void display() {
        System.out.println("Avg/Max/Min temperature = " + (tempSum / numReadings) + "/" + maxTemp + "/" + minTemp);
    }
}
```
#### Usage in Client Code
```java
public class WeatherStationApp {
    public static void main(String[] args) {
        // Create the subject
        WeatherData weatherData = new WeatherData();

        // Create displays and register them automatically to the subject
        CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay(weatherData);
        StatisticsDisplay statisticsDisplay = new StatisticsDisplay(weatherData);

        System.out.println("--- First weather update ---");
        weatherData.setMeasurements(26.5f, 65.0f, 1013.1f);

        System.out.println("\n--- Second weather update ---");
        weatherData.setMeasurements(28.0f, 70.0f, 1012.0f);
    }
}
```

**Output**
```
--- First weather update ---
Current conditions: 26.5°C and 65.0% humidity
Avg/Max/Min temperature = 26.5/26.5/26.5

--- Second weather update ---
Current conditions: 28.0°C and 70.0% humidity
Avg/Max/Min temperature = 27.25/28.0/26.5
```
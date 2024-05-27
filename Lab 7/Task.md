# TASK

**Scenario Description:** Participants are provided with a series of common software design challenges. They will need to choose appropriate design patterns to solve these specific problems effectively.

## 1. **Design Challenges:**
* **Global Configuration Management:** Se utiliza el patron singleton ya que garantiza solo una instancia del objeto con la configuracion accesible para todo el sistema+

* **Dynamic Object Creation Based on User Input:** Se utiliza el atron factory ya que permite la creacion dinamica de diferentes tipos de objetos basados en la entrada del usuario.

* **State Change Notification Across System Components:** Se utiliza el patron observer ya que permite que los componentes suscritos reciban notificaciones de cambio de estado

* **Efficient Management of Asynchronous Operations:** Se elige el patron Promise ya que permite gestionar operaciones asincronas como llamadas a API's


## 2. Project Execution Simulation:
### Singleton
```
public class Configuration {
    private static Configuration instance;
    private Map<String, String> config;
    private Configuration() {
        config = new HashMap<>();
    }
    public static Configuration getInstance() {
        if (instance == null) {
            instance = new Configuration();
        }
        return instance;
    }
    public void setConfig(String key, String value) {
        config.put(key, value);
    }
    public String getConfig(String key) {
        return config.get(key);
    }
}
// Uso
public class Main {
    public static void main(String[] args) {
        Configuration config = Configuration.getInstance();
        config.setConfig("db_host", "localhost");
        System.out.println(config.getConfig("db_host"));
    }
}
```

### Factory
```
interface UIElement {
    void render();
}
// Implementa productos concretos
class Button implements UIElement {
    public void render() {
        System.out.println("Rendering a Button");
    }
}
class TextField implements UIElement {
    public void render() {
        System.out.println("Rendering a TextField");
    }
}
// Crea la fábrica que devuelve el producto correcto basado en la entrada del usuario
class UIElementFactory {
    public static UIElement createElement(String elementType) {
        switch (elementType) {
            case "button":
                return new Button();
            case "textfield":
                return new TextField();
            default:
                throw new IllegalArgumentException("Unknown element type");
        }
    }
}
```
### Observer
```

class Subject {
    private List<Observer> observers = new ArrayList<>();
    public void attach(Observer observer) {
        observers.add(observer);
    }
    public void detach(Observer observer) {
        observers.remove(observer);
    }
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(this);
        }
    }
}

class ConcreteSubject extends Subject {
    private String state;
    public String getState() {
        return state;
    }
    public void setState(String state) {
        this.state = state;
        notifyObservers();
    }
}

interface Observer {
    void update(Subject subject);
}
// ConcreteObserver
class ConcreteObserver implements Observer {
    public void update(Subject subject) {
        if (subject instanceof ConcreteSubject) {
            ConcreteSubject concreteSubject = (ConcreteSubject) subject;
            System.out.println("Observer: Reacted to the event. New state is " + concreteSubject.getState());
        }
    }
}
```

### future
```
public static void main(String[] args) {
        String[] urls = {"http://api1.com", "http://api2.com", "http://api3.com"};
        CompletableFuture<?>[] futures = new CompletableFuture<?>[urls.length];
        for (int i = 0; i < urls.length; i++) {
            futures[i] = fetchData(urls[i]).thenAccept(System.out::println);
        }
        CompletableFuture<Void> allOf = CompletableFuture.allOf(futures);
        try {
            allOf.get();  // Espera a que todas las tareas completen
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
}
```
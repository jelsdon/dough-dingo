![Image of a dingo head](https://github.com/jelsdon/dough-dingo/blob/main/images/dough-dingo-small.png?raw=true)

# dough-dingo
Dough-Dingo is a dough rising controller ...prototype.

The Dough-Dingo will convert any compartment into the dough rising
station of your dreams.

## ~~Features~~ Wish List
* Multiple thermal probes (Low/High position, in-dough, outside)
* Switchable heat source (thermal mat)
* Fan control
* Web Cam (Timelapse, horizontal bar overlay for growth estimate)
* Over / Under temperature alarm
* Humidity monitoring
* Light management
* Network interface
* Physical interface
* Datalogging and presentation

## High Level Design
![Image of component diagram](https://github.com/jelsdon/dough-dingo/blob/main/images/components.png?raw=true)
<details>

```
@startuml
!theme amiga

interface "Heat mat"


folder "Dough Dingo System" {
  [PID]
  [Datalogger]
  [Controller]
  [Webservice]
  frame "Communication" {
    [WiFi]
    frame "GPIO" {
    [1-Wire]
    [Digital I/O]
    [PWM]
    [I2C]
    }
  }
  [Webservice] <--> Controller
  [Controller] <--> Datalogger
  [Controller] <--> PID
  [Controller] <--> Communication
}


folder "Temperature Probes" {
  folder "DS18b20" {
    [1-Wire] --> "Bottom Probe"
    "Bottom Probe" --> "Dough Probe"
    "Dough Probe" --> "Top Probe"
  }
}

folder "Relays" {
    "Heater Relay" --> "Heat mat"
}

 
folder "Screens" {
  interface "ssd1306"
}


[I2C] --> ssd1306
[Digital I/O] --> "Heater Relay"
[Digital I/O] --> Light
[Digital I/O] <-- "Rotary encoder switch"
[PWM] --> Fan
Operator --> Webservice
Operator --> "Rotary encoder switch"
@enduml
```

</details>

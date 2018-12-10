# defibrillator

This is a simplified system representing an automated external defibrillator.
This project has been made to simulate its behaviour using [plug-obp](https://plug-obp.github.io/).

## Class description
### Defibrillator
This class is the "controller" of the system, it receives events from the User, the Calculator and the Capacitor and can send events to the Capacitor and the Led.

Its behaviour comes from the [Open-source Automated External Defibrillator (OAED)](https://github.com/CentroEPiaggio/Open-Automated-External-Defibrillator).

A plantuml description of the state machine is available [here](https://github.com/Pyponou/defibrillator/blob/master/defibrillator_SimpleMAE.puml).

### User
The User is at the moment represented as a class but should not exist and work with buttons. It can only send events to the defibrillator. 

### Capacitor

### Calculator

### Led

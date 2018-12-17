# defibrillator

This is a simplified system representing an automated external defibrillator.
This project has been made to simulate its behaviour using [plug-obp](https://plug-obp.github.io/).

## Class description
### Defibrillator
This class is the "controller" of the system, it receives events from the User, the Calculator and the Capacitor and can send events to the Capacitor and the Led.

Its behaviour comes from the [Open-source Automated External Defibrillator (OAED)](https://github.com/CentroEPiaggio/Open-Automated-External-Defibrillator).

The class has 5 states : 
- **Measurement mode** : is the starting state. In measurement mode, the device has not yet diagnosed any issue in the patient. Therefore, the operations are limited to continuously acquiring signals from the patient.
- **Charging mode** : is reached when OAED successfully diagnoses any issue for the first time. As a result, the charging circuit is enabled, while the patient is still monitored.
- **Discharge enabled** : is entered when the patient is both suffering from any issue and the capacitor is ready. In this mode the defibrillator is armed and ready to deliver the shock when the operator will press the defibrillate "button".
- **Internal discharge** : represents an emergency stop for OAED. Whenever something is not working properly and the capacitor is charged (even partially), OAED will dump the defibrillation energy to the internal discharge circuit.
- **Lead-Off** : is an idle state in which OAED just waits for a patient to be connected. Thereafter in this state, OAED will only perform impedance acquisitions. When it recognizes a patient, lead-off mode is switched to Measurement mode.

It can send events to the capacitor : 
- *startCharge* : indicate that the capacitor must start charging.
- *deliverShock* : indicate that the capacitor can release the energy to the patient.
- *abort* : indicate that the capacitor must discharge.

It can also send events to the Orange Led : 
- *switchOn* : to switch on the led.
- *switchOff* : to switch off the led.

![Alt text](images/defibrillator_SimpleMAE.png?raw=true "Defibrillator State Machine")

### User
The User is at the moment represented as a class but should not exist, it work with buttons instead.
The class has 3 states :
- **Idle** : The initial state, when the User has not connected the patient yet.
- **Helping** : When the patient is connected and the User is waiting for a signal form the Defibrillator.
- **LifeSaving** : When the Defibrillator indicates the patient needs help.

It can send events to the defibrillator :
- *defibrillate* : indicate that the user "pressed" the button to start the defibrillation (release the energy to the patient).
- *lead_On* : indicate that the patient is now connected.
- *lead_Off* : indicate the the patient is disconnected.

![Alt text](images/User.png?raw=true "User State Machine")

### Capacitor

The capacitor is a composant used to store the the energy that can be released to the patient.
Its behaviour has been simplified.
The class has 3 states :
- **Idle** : Initial state, the capacitor is discharged.
- **Charging** : When the capacitor is charging.
- **Charged** : When the capacitor is charged and ready to release the energy.

It can send events to the defibrillator :
- *capacitor_rdy* : to indicate it is ready the release the energy.

![Alt text](images/capacitor_MAE.png?raw=true "Capacitor State Machine")

### Calculator

The calculator is the class saying if the patient is suffering from any ventricular issue (VI) or not (NoVI).
A ventricular issue can either be Tachycardia or Fibrillation.

It can send events to the defibrillator : 
- *VI* : when a ventricular issue is detected.
- *NoVI* : when no ventricular issue has been detected.

There is no modeling diagram for this one as it has been simplified a lot and the algorithm behind it doesn't interest us for the simulation.
For more informations about the algorithm : click [here](https://github.com/CentroEPiaggio/Open-Automated-External-Defibrillator/blob/master/Thesis.pdf).

### Led

The led is a way to communicate with the User.
When the led is On, it means the User can deliver the shock.
Otherwise, it can't.

There is no diagram for this one as it is really simple.

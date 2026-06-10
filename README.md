# Laborator 2 - Modelare UML: Sistem Gestiune Comenzi

Acest repository conține diagramele UML pentru refactorizarea unui sistem de gestiune a comenzilor.

## Decizii de modelare
1. **Use Case:** Am separat clar responsabilitățile actorilor (Client, Admin, Sistem Plată) și am folosit relații `<<include>>` pentru acțiunile dependente (plasare comandă -> plată) și `<<extend>>` pentru cele opționale (reducere).
2. **Class Diagram (Before):** Am evidențiat „God Class-ul” (`OrderManager`) care încalcă principiul Single Responsibility, gestionând direct baza de date, email-urile și logica de business.
3. **Class Diagram (After):** Am propus o arhitectură decuplată, introducând interfețe (`IOrderRepository`, `IPaymentGateway`, `INotificationService`) pentru a respecta Dependency Inversion și a separa logica de infrastructură.
4. **Sequence Diagrams:** Am modelat pașii executivi, folosind blocuri `alt` pentru a trata cazurile în care o comandă nu mai poate fi anulată (deja expediată) și `opt` pentru notificări.

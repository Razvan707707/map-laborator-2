sequenceDiagram
    actor Client
    participant Controller
    participant OrderService
    participant OrderRepository
    participant NotificationService

    Client->>Controller: HTTP POST /api/orders/{id}/cancel
    activate Controller
    Controller->>OrderService: cancelOrder(id)
    activate OrderService
    
    OrderService->>OrderRepository: findById(id)
    activate OrderRepository
    OrderRepository-->>OrderService: order
    deactivate OrderRepository
    
    alt Comanda este deja expediată? (Nu poate fi anulată)
        OrderService-->>Controller: throw Exception("Comanda a fost deja expediată")
        Controller-->>Client: 400 Bad Request
    else Comanda poate fi anulată
        OrderService->>order: cancel()
        OrderService->>OrderRepository: save(order)
        activate OrderRepository
        OrderRepository-->>OrderService: updated
        deactivate OrderRepository
        
        opt Notificare client
            OrderService->>NotificationService: sendCancellationEmail(user)
        end
        
        OrderService-->>Controller: success
        Controller-->>Client: 200 OK
    end
    deactivate OrderService
    deactivate Controller
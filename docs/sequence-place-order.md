sequenceDiagram
    actor Client
    participant Controller
    participant OrderService
    participant PaymentGateway
    participant OrderRepository
    participant NotificationService

    Client->>Controller: HTTP POST /api/orders
    activate Controller
    Controller->>OrderService: placeOrder(cartData)
    activate OrderService
    
    OrderService->>PaymentGateway: processPayment(amount)
    activate PaymentGateway
    PaymentGateway-->>OrderService: paymentSuccess
    deactivate PaymentGateway
    
    OrderService->>OrderRepository: save(order)
    activate OrderRepository
    OrderRepository-->>OrderService: orderSaved
    deactivate OrderRepository
    
    OrderService->>NotificationService: sendConfirmationEmail(user)
    activate NotificationService
    NotificationService-->>OrderService: emailSent
    deactivate NotificationService
    
    OrderService-->>Controller: orderDetails
    deactivate OrderService
    Controller-->>Client: 201 Created (Comandă plasată cu succes)
    deactivate Controller
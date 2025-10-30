# 类结构与关系说明

## 文本说明

- User （抽象类）：  \n  - 子类：RegularUser、VIPUser\n  - 聚合/组合 BorrowRecord、Reservation\n  - 依赖 Book, AccountStatus, UserType\n- Book：  \n  - 被 BorrowRecord、Reservation 关联\n- Library：  \n  - 管理 User、Book、InventoryService、NotificationService\n  - 连接 ExternalLibraryAPI\n- Service 类：  \n  - NotificationService, InventoryService, CreditRepairService, AutoRenewalService\n- 异常类：  \n  - 业务异常、提醒警告、通知异常等\n- 枚举类：  \n  - BookType, UserType, AccountStatus

## 类关系Mermaid图

```mermaid
classDiagram
    class User {
        <<abstract>>
        -id: String
        -name: String
        -accountStatus: AccountStatus
        -credit: int
        -userType: UserType
        +borrowBook(Book)
        +returnBook(Book)
        +reserveBook(Book)
        +payFine(double)
        +toggleAutoRenew()
        ...
    }
    class RegularUser {
        +特殊限制/行为
    }
    class VIPUser {
        +更高权限/行为
    }
    class Book {
        -title: String
        -author: String
        -bookType: BookType
        -inStock: boolean
        ...
        +borrow()
        +returnBook()
    }
    class Reservation {
        -user: User
        -book: Book
    }
    class BorrowRecord {
        -user: User
        -book: Book
        -dueDate: Date
        -returned: boolean
        +isOverdue()
    }
    class Library {
        -users: List~User~
        -books: List~Book~
        -inventoryService: InventoryService
        -notificationService: NotificationService
        +addUser(User)
        +borrowBook(User, Book)
        +returnBook(User, Book)
        +reserveBook(User, Book)
    }
    class InventoryService
    class NotificationService
    class ExternalLibraryAPI
    class CreditRepairService
    class AutoRenewalService
    class BookType
    class UserType
    class AccountStatus

    User <|-- RegularUser
    User <|-- VIPUser
    User "1" o-- "*" BorrowRecord
    User "1" o-- "*" Reservation
    Book "1" o-- "*" Reservation
    Book "1" o-- "*" BorrowRecord
    Library "1" o-- "*" User
    Library "1" o-- "*" Book
    Library "1" o-- "1" InventoryService
    Library "1" o-- "1" NotificationService
    Library "1" ..> ExternalLibraryAPI
    User ..> AccountStatus
    User ..> UserType
    Book ..> BookType
    NotificationService ..> EmailException
    NotificationService ..> SMSException
    User ..> AccountFrozenException
    User ..> BlacklistedUserException
    User ..> InsufficientCreditException
    BorrowRecord ..> OverdueFineException
    Reservation ..> ReservationNotAllowedException
    Book ..> BookNotAvailableException
    User ..> InvalidOperationException
    User ..> CreditRepairService
    User ..> AutoRenewalService
```

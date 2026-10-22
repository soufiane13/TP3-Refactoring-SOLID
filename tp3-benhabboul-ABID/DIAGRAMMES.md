# Diagrammes de Classes

## 1. Existant (Avant refactoring)

```mermaid
classDiagram
    class ReportService {
        -dbFile: URL
        -smtp: Object
        -outbox: Array
        -history: Array
        +loadOrders()
        +generate(month, format, recipient)
    }
    
    class FreeReportService {
        +generate(month, format, recipient)
    }

    ReportService <|-- FreeReportService : hérite de
    ReportService ..> orders_json : lit
    ReportService ..> fs : utilise (dépendance technique)
    ReportService ..> console : utilise (dépendance technique)
    ReportService ..> SMTP : utilise (dépendance technique)

    ## 2. Cible (Après refactoring complet)

```mermaid
classDiagram
    class ReportService {
        +generate(month, format, recipient)
    }
    
    class OrderRepository {
        <<interface>>
        +all()
    }
    
    class Formatter {
        <<interface>>
        +format(stats)
    }
    
    class Mailer {
        <<interface>>
        +send(mail)
    }
    
    class Logger {
        <<interface>>
        +info(msg)
    }
    
    class JsonFileOrderRepository
    class CsvFormatter
    class HtmlFormatter
    class SmtpMailer
    class ConsoleLogger

    ReportService --> OrderRepository : dépend de
    ReportService --> Formatter : dépend de
    ReportService --> Mailer : dépend de
    ReportService --> Logger : dépend de

    OrderRepository <.. JsonFileOrderRepository : implémente
    Formatter <.. CsvFormatter : implémente
    Formatter <.. HtmlFormatter : implémente
    Mailer <.. SmtpMailer : implémente
    Logger <.. ConsoleLogger : implémente
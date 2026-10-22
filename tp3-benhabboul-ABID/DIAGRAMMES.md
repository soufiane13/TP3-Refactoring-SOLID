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
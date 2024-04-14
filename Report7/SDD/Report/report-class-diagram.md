```mermaid
---
title: Alarm Triggering, Notifying and Resolving class diagram
---
classDiagram
  namespace event {
    class RmsHandler {
      <<interface>>
      + handle(plcData) void
    }

    class ReportHandler {
      - ReportService reportService
    } 
  }

%% realization
ReportHandler ..|> RmsHandler

%% dependency 
ReportService ..> ReportRowPersistenceService
ReportRowPersistenceService ..> ReportRowRepository

ReportHandler ..> BlueprintPersistenceService
BlueprintPersistenceService ..> BlueprintRepository

ReportHandler ..> PlcConnector

ReportHandler ..> ReportSchedulePersistenceService
ReportSchedulePersistenceService ..> ReportScheduleRepository

ReportHandler ..> ReportExcelProcessor

ReportHandler ..> ReportService
ReportService ..> ReportPersistenceService
ReportPersistenceService ..> ReportRepository






  namespace service {
    class ReportService {
        -AlarmPersistenceService alarmPersistenceService
    }
    class ReportSchedulePersistenceService  {
         - ReportService reportService
    }
    class ReportRowPersistenceService {
         - ReportService reportService
    }
    
    class BlueprintPersistenceService {
         - ReportService reportService
    }

    class ReportPersistenceService {
      - ReportService reportService
    }
    
  }

    namespace api {
        class PlcConnector  {
        <<interface>>
         - ReportService reportService
        }
    }

    namespace support {
        class ReportExcelProcessor {
         - String reportDir
         + process(type, report, rows) List

        }
    }

    namespace repository {
        class BlueprintRepository {
        <<interface>>
         - ReportService reportService  
        }

        class ReportRepository {
        <<interface>>
         - ReportService reportService  
        }
        class ReportScheduleRepository {
        <<interface>>
         - ReportService reportService   
        }
        class ReportRowRepository {
        <<interface>>
         - ReportService reportService   
        }
    }

    ReportRepository ..> Report
    ReportScheduleRepository ..> ReportSchedule
    ReportRowRepository ..> ReportRow

    namespace MODEL {
      class Report {
         - Long id
      }
     
     class ReportRow {
      - Long id
     }

     class ReportSchedule {
      - Long id
     }

     class ReportType {
      -Long id
      -String name
     }

     class ReportRowPeriod {
      <<enumeration>>
      OLD
     }

     class ReportRowShift {
      <<enumeration>>
      I
      II
     }
    }

  Report *-- ReportRow
  Report *-- ReportType
  ReportSchedule *-- ReportRowPeriod
  ReportSchedule *-- ReportRowShift
  ReportSchedule *-- ReportType
  
  ReportRow *-- ReportRowShift
  ```

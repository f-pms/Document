```mermaid
---
title: Alarm Triggering, Notifying and Resolving class diagram
---
classDiagram
  namespace event {
    class RmsHandler {
      <<interface>>
      +handle(plcData) void
    }

    class ReportHandler {
     - ReportSchedulePersistenceService reportSchedulePersistenceService
     - ReportService reportService
     - ReportExcelProcessor processor
     - BlueprintPersistenceService blueprintService
     - PlcConnector plcConnector
     + handle(context, response) void
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
    
  }

    namespace api {
        class PlcConnector  {
        <<interface>>
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
        }

        class ReportRepository {
        <<interface>>  
        }
        class ReportScheduleRepository {
        <<interface>>   
        }
        class ReportRowRepository {
        <<interface>>   
        }
    }
  ```

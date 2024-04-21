```mermaid
sequenceDiagram
  actor User 
  participant Browser
  participant ReportController
  participant ReportService
  participant ReportPersistenceService
  participant ReportRepository

    User ->>+ Browser: Click "Báo cáo thống kê"
    Browser ->>+ ReportController: GET /api/reports/charts/multi-day
    ReportController ->>+ ReportService: getMultiDayChartFigures(searchCommand)
    ReportService ->>+ ReportPersistenceService: getAll(reportCriteria)
    ReportPersistenceService ->>+ ReportRepository: findAll(spec, pagination)
    ReportRepository -->>- ReportPersistenceService: return data
    ReportPersistenceService -->>- ReportService: return reports
    loop for each report
       ReportService ->>+ ReportService: calculate and add to chartData
    end
    ReportService -->>- ReportController: return chartData
    ReportController -->>- Browser: return successObject
    Browser -->>- User: Show statistics charts
```

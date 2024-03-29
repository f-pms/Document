```mermaid
sequenceDiagram 
  title PLC Integration 
  HbcScraper ->>+ PlcDataSource: get all tags 
  PlcDataSource ->>+ BlueprintPersistenceService: get all tags
  BlueprintPersistenceService ->>- PlcDataSource: return all tags
  PlcDataSource ->>- HbcScraper: return all tags

  HbcScraper ->>+ HbcScraperTask: initScrapperTask(tags)
  HbcScraperTask ->>- HbcScraper: return scrapperTask

  HbcScraper ->>+ CronScrapeJob: getCron() 
  CronScrapeJob ->>- HbcScraper: return cron
  HbcScraper ->>+ ThreadPoolTaskScheduler: schedule(scrapperTask, cron)
  ThreadPoolTaskScheduler ->>- HbcScraper: return ScheduledFuture
```
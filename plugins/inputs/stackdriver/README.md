# Stackdriver Input Plugin

Stackdriver scrapes metrics from Google Cloud Monitoring.

### Configuration:

```toml
[[inputs.stackdriver]]
  ## GCP Project
  project = "projects/erudite-bloom-151019"

  ## API rate limit. On a default project, it seems that a single user can make
  ## ~14 requests per second. This might be configurable. Each API request can
  ## fetch every time series for a single metric type, though -- this is plenty
  ## fast for scraping all the builtin metric types (and even a handful of
  ## custom ones) every 60s.
  # rateLimit = 14

  ## Every query to stackdriver queries for data points that have timestamp t
  ## such that: (now() - lookbackSeconds) <= t < now(). Note that influx will
  ## de-dedupe points that are pulled twice, so it's best to be safe here, just
  ## in case it takes GCP awhile to get around to recording the data you seek.
  # lookbackSeconds = 600

  ## Sets whether or not to scrape all bucket counts for metrics whose value
  ## type is "distribution". If those ~70 fields per metric
  ## type are annoying to you, try out the distributionAggregationAligners
  ## configuration option, wherein you may specifiy a list of aggregate functions
  ## (e.g., ALIGN_PERCENTILE_99) that might be more useful to you.
  # scrapeDistributionBuckets = true

  ## Excluded GCP metric types. Any string prefix works.
  ## Only declare either this or includeMetricTypePrefixes
  excludeMetricTypePrefixes = [
    "agent",
    "aws",
    "custom"
  ]

  ## *Only* include these GCP metric types. Any string prefix works
  ## Only declare either this or excludeMetricTypePrefixes
  # includeMetricTypePrefixes = nil

  ## Excluded GCP metric and resource tags. Any string prefix works.
  ## Only declare either this or includeTagPrefixes
  excludeTagPrefixes = [
    "pod_id",
  ]

  ## *Only* include these GCP metric and resource tags. Any string prefix works
  ## Only declare either this or excludeTagPrefixes
  # includeTagPrefixes = nil

  ## Declares a list of aggregate functions to be used for metric types whose
  ## value type is "distribution". These aggregate values are recorded in the
  ## distribution's measurement *in addition* to the bucket counts. That is to
  ## say: setting this option is not mutually exclusive with
  ## scrapeDistributionBuckets.
  distributionAggregationAligners = [
    "ALIGN_PERCENTILE_99",
    "ALIGN_PERCENTILE_95",
    "ALIGN_PERCENTILE_50",
  ]
```
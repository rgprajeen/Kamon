# kamon-graphite

Write report metrics in graphite format.
   
## Usage
Since kamon 2.0 you only have to include the dependency to the reporter.

    libraryDependencies += "io.kamon" %% "kamon-graphite" % "2.0.0"

Example config

application.conf

    kamon {
      environment {
        service = "supercool-app"
        tags {
          env = "local"
        }
      }
      graphite {
        hostname = "127.0.0.1"
        port = 2003
      }
    }      


## Config
Per default the graphite tag support (available since graphite 1.1) is enabled (see include-tags feature flag).
These are the relevant config values that chan be adapted to your needs using a custom `application.conf`.

    kamon {
        modules {
          graphite-reporter {
            enabled = true
          }
        }
    
        graphite {
            # Hostname and port in which your Carbon daemon is running.
            hostname = "127.0.0.1"
            port = 2003
    
            # Prefix for all metrics sent to Graphite.
            metric-name-prefix = "kamon-graphite"
    
            # instead of adding tags as suffix to metric name (format graphite 1.1) metricname;tag1=value1;tag2=value2
            # the metric will be named metricname.tag1.value1.tag2.value2
            # this is useful for older graphite versions (prior to 1.1) without tagging support
            legacy-support = false
    
            # For histograms, which percentiles to count
            percentiles = [50.0,90.0,99.0]
    
            # Allow including environment information as tags on all reported metrics.
            environment-tags {
    
              # Define whether specific environment settings will be included as tags in all exposed metrics. When enabled,
              # the service, host and instance tags will be added using the values from Kamon.environment().
              include-service = yes
              include-host = yes
              include-instance = yes
    
              # Specifies which Kamon environment tags should be ignored. All unmatched tags will be always added to all metrics.
              exclude = []
            }
    
            tag-filter {
              includes = ["**"]
              excludes = []
            }
        }
    }

## Failure handling
When sending metrics at a tick interval fails the current snapshot will be dropped and the next snapshot will try to send metrics using new connection.


# The AERI Stacktraces dataset

## Presentation

The [Automated Error Reporting](https://wiki.eclipse.org/EPP/Logging) (AERI) system retrieves [information about exceptions](https://www.codetrails.com/error-analytics/manual/). It is installed by default in the [Eclipse IDE](http://www.eclipse.org/ide/) and has helped hundreds of projects better support their users and resolve bugs.  

This dataset is a dump of all records over a couple of years, with useful information about the exceptions and environment. It is composed of:

* **Incidents** When an exception occurs and is trapped by the AERI system, it constitutes an incident (or error report). An incident can be reported by several different people, can be reported multiple times, and can be linked to different environments.
* **Problems** As soon as an error report arrives on the server, it will be analyzed and subsequently assigned to one or more problems. A problem thus represents a set of (similar) error reports which usually have the same root cause – for example a bug in your software. (Extract from the [AERI system documentation](https://www.codetrails.com/error-analytics/manual/concepts/error-reports-problems-bugs-projects.html))

This dataset is published under the [Creative Commons BY-Attribution-Share Alike 4.0 (International) licence](https://creativecommons.org/licenses/by-sa/4.0/).


## Downloads

* **Problems full** [ [Download JSON](problems_full.tar.bz2) ] -- A list of all problems, exported as JSON (one problem per file).
    * Content: 125250 entries, 22 attributes
    * Size: 38M compressed, 904M raw
* **Problems extract** [ [Download CSV](problems_extract.csv.bz2) ] -- A list of all problems, exported as CSV (one big file).
    * Content: 125250 entries, 22 attributes
    * Size: 1.5M compressed, 14M raw
* **Incidents full** [ [Download JSON](incidents_full.tar.bz2) ] -- A list of all incidents, exported as JSON (one incident per file).
    * Content: 2084363 entries, 22 attributes
    * Size: 820M compressed, 19G raw
* **Incidents extract** [ [Download CSV](incidents_extract.csv.bz2) ] -- A list of all incidents, exported as CSV (one big file).
    * Content: 2084045 entries, 20 attributes
    * Size: 141M compressed, 778M raw
* **Incidents Bundles** [ [Download CSV](incidents_bundles_extract.csv.bz2) ] -- A list of all bundles found in incidents, exported as CSV. Attributes are bundle_name, bundle_version, and number of occurrences.
    * Content: 29709 entries, 3 attributes
    * Size: 220K compressed, 1.5M raw

**Documentation**

* **Stacktraces Problems analysis document** [ [Download PDF](problems_analysis.pdf) | [Download Rmd](problems_analysis.rmd) ] -- A R Markdown document to analyse the Stacktraces problem dataset, with description of the actual content and examples of usage.
* **Stacktraces Incidents analysis document** [ [Download PDF](incidents_analysis.pdf) | [Download Rmd](incidents_analysis.rmd) ] -- A R Markdown document to analyse the Stacktraces incidents dataset, with description of the actual content and examples of usage.

More information about the AERI system can be found on the [Code Trails website](https://www.codetrails.com/error-analytics/manual/).



## Privacy concerns

The result contains no email address, user id or machine id. Rather than removing the information (we are not sure that we remove all required information) we decided to simply pick relevant information from the file and push it into the output.

End users have an option to keep their own class names private. We have presently no simple means to know what stacktraces in the database extraction should be kept private, so we decided to play it safe and hide class names whose packages don't start with known prefixes [1]. All private classnames have been replaced by the HIDDEN keyword.

[1] `"ch.qos.*", "com.cforcoding.*", "com.google.*", "com.gradleware.tooling.*", "com.mountainminds.eclemma.*", "com.naef.*", "com.sun.*", "java.*", "javafx.*", "javax.*", "org.apache.*", "org.eclipse.*", "org.fordiac.*", "org.gradle.*", "org.jacoco.*", "org.osgi.*", "org.slf4j.*", "sun.*" `


## Format: problems

    {
      "summary": "",
      "osgiArch": "",
      "osgiOs": "",
      "osgiOsVersion": "",
      "osgiWs": "",
      "eclipseBuildId": "",
      "eclipseProduct": "",
      "javaRuntimeVersion": "",
      "numberOfIncidents": 0,
      "numberOfReporters": 74,
      "stacktraces": [
        [ "stacktrace for incident" ],
        [ "stacktrace for cause" ],
        [ "stacktrace for exception" ]
      ]
    }


## Format: incidents

    {
      "eclipseBuildId":"4.6.1.M20160907-1200",
      "eclipseProduct":"org.eclipse.epp.package.jee.product",
      "javaRuntimeVersion":"1.8.0_112-b15",
      "osgiArch":"x86_64",
      "osgiOs":"Windows7",
      "osgiOsVersion":"6.1.0",
      "osgiWs":"win32",
      "stacktraces":[
        [ "stacktrace" ]
      ],
      "summary": "Failed to retrieve default libraries for jre1.8.0_111"
    }


## Format: Stacktraces

The structure used in the mongodb for stacktraces has been kept as is: it is composed of fields with all information relevant to each line of the stacktrace. Each stacktrace is an array of objects as shown below:

    [
      {
        "cN": "sun.net.www.http.HttpClient",
        "mN": "parseHTTPHeader",
        "fN": "HttpClient.java",
        "lN": 786,
      }
    ]

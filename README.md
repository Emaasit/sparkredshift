Read data from Amazon Redshift into Spark DataFrames
====================================================

What is sparkredshift?
----------------------

sparkredshipt is an extension for sparklyr to read data from Amazon
Redshift into Spark DataFrames. It uses the Spark package spark-redshift
to load redshift data into Spark DataFrames.

Installation
------------

sparkredshift requires the sparklyr package to run

### Install sparklyr

I recommend the latest stable version of sparklyr available on CRAN

    install.packages("sparklyr")

### Install sparkredshift

Install the development version of sparkredshift from this Github repo
using devtools

    library(devtools)
    devtools::install_github("emaasit/sparkredshift")

Connecting to Spark
-------------------

If Spark is not already installed, use the following sparklyr command to
install your preferred version of Spark:

    library(sparklyr)
    spark_install(version = "2.0.0")

The call to `library(sparkredshift)` will make the sparkredshift
functions available on the R search path and will also ensure that the
dependencies required by the package are included when we connect to
Spark.

    library(sparkredshift) 

We can create a Spark connection as follows:

    sc <- spark_connect(master = "local")

Reading redshift files
----------------------

sparkredshift provides the function `spark_read_redshift` to read
redshift data files into Spark DataFrames. It uses a Spark package
called spark-redshift. Here's an example.

    mtcars_file <- system.file("extdata", "mtcars.redshift", package = "sparkredshift")

    mtcars_df <- spark_read_redshift(sc, path = mtcars_file, table = "redshift_table")
    mtcars_df

The resulting pointer to a Spark table can be further used in dplyr
statements.

    library(dplyr)
    mtcars_df %>% group_by(cyl) %>%
      summarise(count = n(), avg.mpg = mean(mpg), avg.displacment = mean(disp), avg.horsepower = mean(hp))

Logs & Disconnect
-----------------

Look at the Spark log from R:

    spark_log(sc, n = 100)

Now we disconnect from Spark:

    spark_disconnect(sc)

Acknowledgements
----------------

Thanks to RStudio for the sparklyr package that provides functionality
to create Extensions.

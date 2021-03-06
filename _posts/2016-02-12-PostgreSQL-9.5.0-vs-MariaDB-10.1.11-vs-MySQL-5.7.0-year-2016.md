---
layout: post
title: PostgreSQL [9.5.0] vs MariaDB [10.1.11] vs MySQL [5.7.0] year 2016
tags: [DB, benchmark]
image: images/2015121900.jpg
---

![PostgreSQL-MySQL]({{ site.baseurl }}/images/2015121900.jpg "PostgreSQL-MySQL")

<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.2/jquery.min.js">
</script>
<script src="http://code.highcharts.com/highcharts.js">
</script>
<script src="http://code.highcharts.com/modules/exporting.js">
</script>

<div id="container" style="min-width: 600px; height: 400px; margin: 0 auto"></div>
<div id="container2" style="min-width: 600px; height: 400px; margin: 0 auto"></div>

<script type="text/javascript">

$(function () {
    $('#container').highcharts({
        chart: {
            type: 'bar'
        },
        title: {
            text: 'PostgreSQL 9.5.0 vs MariaDB 10.1.11 vs MySQL 5.7.0'
        },
        subtitle: {
            text: 'Source: <a href="https://nghenglim.github.io">nghenglim.github.io</a>'
        },
        xAxis: {
            categories: ['Write (10000 rows)', 'Read (Select)', 'READ (WHERE)', 'READ (WHERE A+B>C)', 'READ (COUNT WHERE A+B>C)', 'READ (WHERE ORDER)', 'READ (%wildcard% + ORDER)'],
            title: {
                text: null
            }
        },
        yAxis: {
            min: 0,
            title: {
                text: 'Time Taken (millisecond)',
                align: 'high'
            },
            labels: {
                overflow: 'justify'
            }
        },
        tooltip: {
            valueSuffix: ' millisecond'
        },
        plotOptions: {
            bar: {
                dataLabels: {
                    enabled: true
                }
            }
        },
        legend: {
            layout: 'vertical',
            align: 'right',
            verticalAlign: 'top',
            x: 0,
            y: 90,
            floating: true,
            borderWidth: 1,
            backgroundColor: ((Highcharts.theme && Highcharts.theme.legendBackgroundColor) || '#FFFFFF'),
            shadow: true
        },
        credits: {
            enabled: false
        },
        series: [{
            name: 'MariaDB',
            data: [50.02, 3.87, 4.36, 5.18, 261.69, 741.55, 639.75]
        }, {
            name: 'MySQL',
            data: [50.86, 3.42, 3.91, 5.33, 246.77, 6686.11, 508.9]
        }, {
            name: 'PostgreSQL',
            data: [79.2, 3.27, 3.62, 4, 169.18, 229.33, 207.01]
        }]
    });
});

</script>

<script type="text/javascript">

$(function () {
    $('#container2').highcharts({
        chart: {
            type: 'bar'
        },
        title: {
            text: 'PostgreSQL 9.5.0 vs MariaDB 10.1.11 vs MySQL 5.7.0'
        },
        subtitle: {
            text: 'Source: <a href="https://nghenglim.github.io">nghenglim.github.io</a>'
        },
        xAxis: {
            categories: ['Write (10000 rows)', 'Read (Select)', 'READ (WHERE)', 'READ (WHERE A+B>C)', 'READ (COUNT WHERE A+B>C)', 'READ (WHERE ORDER)', 'READ (%wildcard% + ORDER)'],
            title: {
                text: null
            }
        },
        yAxis: {
            min: 0,
            title: {
                text: 'Time Taken in natural log(millisecond)',
                align: 'high'
            },
            labels: {
                overflow: 'justify'
            }
        },
        tooltip: {
            valueSuffix: ' log(millisecond)'
        },
        plotOptions: {
            bar: {
                dataLabels: {
                    enabled: true
                }
            }
        },
        legend: {
            layout: 'vertical',
            align: 'right',
            verticalAlign: 'top',
            x: 0,
            y: 90,
            floating: true,
            borderWidth: 1,
            backgroundColor: ((Highcharts.theme && Highcharts.theme.legendBackgroundColor) || '#FFFFFF'),
            shadow: true
        },
        credits: {
            enabled: false
        },
        series: [{
            name: 'MariaDB',
            data: [3.91242293, 1.35325451, 1.47247206, 1.64480506, 5.5671606, 6.60874259, 6.46107748]
        }, {
            name: 'MySQL',
            data: [3.92907676, 1.22964055, 1.36353737, 1.67335124, 5.50845673, 8.80778752, 6.23225153]
        }, {
            name: 'PostgreSQL',
            data: [4.3719763,  1.18478998, 1.28647403, 1.38629436, 5.13096324, 5.43516201, 5.3327671]
        }]
    });
});

</script>
### Reason to Benchmark
On Dec 2015 I have done a similar [benchmark](http://nghenglim.github.io/PostgreSQL-vs-MariaDB(MySQL)-2015/), however I had done some serious mistake on the benchmark and caused the result to be biased towards MySQL.

Therefore I had enhanced the benchmark. The benchmark script and how to setup is at [this Github Repo](https://github.com/nghenglim/database_benchmark).

### Notice & FAQ
- Why using default database configuration / configuration between databases is difference:
  - This benchmark is using default configuration that comes with the docker image.
  - Time consuming to standardize databases configuration
  - Default configuration should give a basic idea on how the database performs.
- Why missing benchmark on index, included data transfer time for benchmark, etc:
  - Is in the To Do list, however IMHO these are good to have benchmark.
  - Preferably others can help to enhance the [script](https://github.com/nghenglim/database_benchmark).
- Why run in VM which make this benchmark not so reliable:
  - Do not want to spend money to buy a physical server or cloud instances.
  - Make benchmark decentralized, everyone who had a laptop should be able to do their own benchmark instead of relying on others.
  - A rough idea on how these databases perform is very important knowledge for all the developer.

### Benchmark Environment (Vagrant)
- Ubuntu wily werewolf (15.10)
- 1 core CPU, 100% execution cap
- 1024 MB memory

### Benchmark Environment (Docker)
- mariadb:10.1.11
- mysql:5.7.10
- postgres:9.5.0

### Benchmark Details
Full detail is at [this Github Repo Folder](https://github.com/nghenglim/database_benchmark/tree/master/Mariadb10.1.11-MySQL5.7.10-Postgres9.5.0)

Total Rows: 1 million

Queries:

1. `SELECT * FROM testing LIMIT 1000`
2. `SELECT * FROM testing WHERE int_col > 5000 LIMIT 1000`
3. `SELECT * FROM testing WHERE int_col + int_col2 > 12345`
4. `SELECT COUNT(*) FROM testing WHERE int_col + int_col2 > 12345`
5. `SELECT * FROM testing WHERE int_col > 5000 ORDER BY word_col ASC LIMIT 1000`
6. `SELECT * FROM testing WHERE word_col LIKE '%lim%' ORDER BY word_col DESC LIMIT 1000`

MariaDB Summary:

~~~ 
average write time: 50.02ms/10000rows
average query time (q1): 3.87ms
average query time (q2): 4.36ms
average query time (q3): 5.18ms
average query time (q4): 261.69ms
average query time (q5): 741.55ms
average query time (q6): 639.75ms
~~~ 

MySQL Summary:

~~~ 
average write time: 50.86ms/10000rows
average query time (q1): 3.42ms
average query time (q2): 3.91ms
average query time (q3): 5.33ms
average query time (q4): 246.77ms
average query time (q5): 6686.11ms
average query time (q6): 508.9ms
~~~ 

PostgreSQL Summary:

~~~ 
average write time: 79.2ms/10000rows
average query time (q1): 3.27ms
average query time (q2): 3.62ms
average query time (q3): 4.0ms
average query time (q4): 169.18ms
average query time (q5): 229.33ms
average query time (q6): 207.01ms
~~~ 

### Conclusion
- MariabDB & MySQL has slight advantages at database writing
- PostgreSQL has slight advantages at simple database read query
- PostgreSQL has big advantages at complex database read query

As a result, I think that choosing PostgreSQL is a better options for RDBMS - provided that PostgreSQL comes with more features and following standard SQL.

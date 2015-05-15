---
layout: page 
tagline: "I'm in ur dataz, QCing ur ADNIz"
tags: [ADNI]
group: navigation
---
{% include JB/setup %}

<script type="text/javascript" src="http://code.jquery.com/jquery-1.9.1.min.js"></script>
<script type="text/javascript" src="http://code.highcharts.com/highcharts.js"></script>
<script type="text/javascript" src="http://code.highcharts.com/modules/data.js"></script>

<div id="container0" style="width: 600px; height: 400px; margin: auto auto;"></div>
<div id="container1" style="width: 600px; height: 400px; margin: auto auto;"></div>
<div id="container2" style="width: 600px; height: 400px; margin: auto auto;"></div>
<div id="container3" style="width: 600px; height: 400px; margin: auto auto;"></div>
<div id="container4" style="width: 600px; height: 400px; margin: auto auto;"></div>
<div id="container5" style="width: 600px; height: 400px; margin: auto auto;"></div>
<div id="container6" style="width: 600px; height: 400px; margin: auto auto;"></div>
<div id="container7" style="width: 600px; height: 400px; margin: auto auto;"></div>
<div id="container8" style="width: 600px; height: 400px; margin: auto auto;"></div>

<script type="text/javascript">
databr0s = [
{  csvfile:  '/spins/assets/adni_0.csv',  div:  "#container0",  x:  "Week Number",  title:  "S1",    y: "T1 Contrast" },
{  csvfile:  '/spins/assets/adni_1.csv',  div:  "#container1",  x:  "Week Number",  title:  "S2",    y: "T1 Contrast" },
{  csvfile:  '/spins/assets/adni_2.csv',  div:  "#container2",  x:  "Week Number",  title:  "S3",    y: "T1 Contrast" },
{  csvfile:  '/spins/assets/adni_3.csv',  div:  "#container3",  x:  "Week Number",  title:  "S4",    y: "T1 Contrast" },
{  csvfile:  '/spins/assets/adni_4.csv',  div:  "#container4",  x:  "Week Number",  title:  "S5",    y: "T1 Contrast" },
{  csvfile:  '/spins/assets/adni_5.csv',  div:  "#container5",  x:  "Week Number",  title:  "S2/S1", y: "T1 Ratio" },
{  csvfile:  '/spins/assets/adni_6.csv',  div:  "#container6",  x:  "Week Number",  title:  "S3/S1", y: "T1 Ratio" },
{  csvfile:  '/spins/assets/adni_7.csv',  div:  "#container7",  x:  "Week Number",  title:  "S4/S1", y: "T1 Ratio" },
{  csvfile:  '/spins/assets/adni_8.csv',  div:  "#container8",  x:  "Week Number",  title:  "S5/S1", y: "T1 Ratio" },
]; 

//$(function () {
  for (var i = 0; i < databr0s.length; i++ ) {
    (function (bro) {
      $.get(bro.csvfile, function(csv) {
        $(bro.div).highcharts({
          chart: { type: 'line' },
          data:  { csv: csv },
          title: { text: bro.title },
          yAxis: { title: { text: bro.y } }, 
          xAxis: { title: { text: bro.x } }
          });
      }, "text");
    })(bro = databr0s[i]); 
  }
//});
</script>

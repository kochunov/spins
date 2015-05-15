---
layout: default
tagline: "Testing highcharts"
tags : [highcharts]
---
<script type="text/javascript" src="http://code.jquery.com/jquery-1.9.1.min.js"></script>
<script type="text/javascript" src="http://code.highcharts.com/highcharts.js"></script>
<script type="text/javascript" src="http://code.highcharts.com/modules/data.js"></script>

<div style="margin: auto auto">
<div id="container0" style="width: 600px; height: 400px; margin: 1em 1em;"></div>
<div id="container1" style="width: 600px; height: 400px; margin: 1em 1em;"></div>
<div id="container2" style="width: 600px; height: 400px; margin: 1em 1em;"></div>
<div id="container3" style="width: 600px; height: 400px; margin: 1em 1em;"></div>
<div id="container4" style="width: 600px; height: 400px; margin: 1em 1em;"></div>
<div id="container5" style="width: 600px; height: 400px; margin: 1em 1em;"></div>
<div id="container6" style="width: 600px; height: 400px; margin: 1em 1em;"></div>
</div>

<script type="text/javascript">
databr0s = [
{  csvfile:  '/spins/assets/fmri_0.csv',  div:  "#container0",  x:  "Week Number",  title:  "Mean",               y: "mean"    },
{  csvfile:  '/spins/assets/fmri_1.csv',  div:  "#container1",  x:  "Week Number",  title:  "Standard Deviation", y: "stddev"  },
{  csvfile:  '/spins/assets/fmri_2.csv',  div:  "#container2",  x:  "Week Number",  title:  "% fluctuation",      y: "% fluctuation"  },
{  csvfile:  '/spins/assets/fmri_3.csv',  div:  "#container3",  x:  "Week Number",  title:  "Drift",              y: "Drift"          },
{  csvfile:  '/spins/assets/fmri_4.csv',  div:  "#container4",  x:  "Week Number",  title:  "SNR",                y: "SNR"            },
{  csvfile:  '/spins/assets/fmri_5.csv',  div:  "#container5",  x:  "Week Number",  title:  "SFNR",               y: "SFNR"           },
{  csvfile:  '/spins/assets/fmri_6.csv',  div:  "#container6",  x:  "Week Number",  title:  "RDC",                y: "RDC"            },
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

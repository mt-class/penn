---
layout: default
img: race
img_link: http://www.flickr.com/photos/nationaalarchief/3198249977/
title: Leaderboard
active_tab: leaderboard
---

<script src="http://code.jquery.com/jquery-1.7.1.min.js" type="text/javascript"></script>
<script src="assets/css/sorttable.js"></script>


Leaderboard
=============================================================

This page contains the assignment leaderboard, which is updated automatically
whenever a submission is received.

<table class="sortable" style="width: 100%">
  <thead style="background-color: lightgrey">
    <tr>
      <th style="text-align: center; width: 75px">
        Rank
      </th>
      <th>
        Alias
      </th>
      <th valign="top" style="text-align: center; width: 100px">
        <a href="hw0.html">HW 0</a><br/>
        <span class="small"># Correct</span>
      </th>
      <th valign="top" style="text-align: center; width: 100px">
        <a href="hw1.html">HW 1</a><br/>
        <span class="small">AER</span>
      </th>
      <th valign="top" style="text-align: center; width: 100px">
        <a href="hw2.html">HW 2</a><br/>
        <span class="small">Model Score</span>
      </th>
      <th valign="top" style="text-align: center; width: 100px">
        <a href="hw3.html">HW 3</a><br/>
        <span class="small">Accuracy</span>
      </th>
      <th valign="top" style="text-align: center; width: 100px">
        <a href="hw4.html">HW 4</a><br/>
        <span class="small">BLEU</span>
      </th>
      <th valign="top" style="text-align: center; width: 100px">
        <a href="hw5.html">HW 5</a><br/>
        <span class="small">Accuracy</span>
      </th>
    </tr>
  </thead>
  <tbody>

<script type="text/javascript" src="http://www.seas.upenn.edu/~cis526/leaderboard.js"></script>
<script type="text/javascript">

for (i = 0; i < data.length; i++){
  var rank = data[i][0];
  var alias = data[i][1];

  document.write('<tr id="' + alias + '"');
  if (i % 2 == 1) { document.write(' bgcolor="lightblue"'); }
  document.write('>');

  document.write('<td style="text-align: center">' + rank + '</td>');
  document.write('<td>' + alias + '</td>');
  document.write('<td style="text-align: center">' + data[i][3] + '</td>');
  if (data[i][2] != "") {
    document.write('<td style="text-align: center"><a href="http://www.seas.upenn.edu/~cis526/reports/hw1/' + data[i][2] + '.pdf">' + data[i][4] + '</a></td>');
  } else {
    document.write('<td style="text-align: center">' + data[i][4] + '</td>');
  }
  document.write('<td style="text-align: center">' + data[i][5] + '</td>');
  document.write('<td style="text-align: center">' + data[i][6] + '</td>');
  document.write('<td style="text-align: center"></td>');
  document.write('<td style="text-align: center"></td>');

  document.write('</tr>');
}

$("#baseline").css({'background-color': 'PaleGoldenRod'});
$("#default").css({'background-color': 'LightCoral'});
$("#oracle").css({'background-color': 'LimeGreen'});

</script>

  </tbody>
</table>

### Legend

A value of None indicates that an error occurred while processing your submission.

The <span style="background-color: LightCoral">default</span> score is what you
should get if you run the code that was provided for you.

The <span style="background-color: PaleGoldenRod">baseline</span> score is the
baseline you should strive to beat. Typically, this line depicts the effort
required to earn a B on the project. Additional points will be rewarded for
beating the baseline by a significant amount, and the most points will be awarded
for placing at or near the top of the rankings.

The <span style="background-color: LimeGreen">oracle</span> score is a metric-aware
composite of the best results from each student submission. It is not possible to
beat the oracle, but it gives you an idea of how much improvement there is for your
system to find.

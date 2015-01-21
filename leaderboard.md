---
layout: default
img: race
img_link: http://www.flickr.com/photos/nationaalarchief/3198249977/
title: Leaderboard
active_tab: leaderboard
---

<script src="http://code.jquery.com/jquery-1.7.1.min.js" type="text/javascript"></script>
<script type="text/javascript" src="http://www.seas.upenn.edu/~cis526/leaderboard.js"></script>

<div class="site">
  <div class="content">
    <h1>Leaderboard</h1>
    <div id="course" style="width: 800px">

      <p>
        This page contains the assignment leaderboard, which is updated
        automatically when a submission is received.
      </p>

      <div style="float: left">
<table>
  <thead style="background-color: lightgrey">
    <tr>
      <th colspan="2"></th>
      <th colspan="6" align="center">
        Assignments
      </th>
    </tr>
    <tr>
      <th>
        Rank
      </th>
      <th>
        Alias
      </th>
      <th valign="top">
        <a href="hw0.html">HW 0</a><br/>
        <span class="small"># Correct</span>
      </th>
      <th valign="top">
        <a href="hw1.html">HW 1</a><br/>
        <span class="small">AER</span>
      </th>
      <th valign="top">
        <a href="hw2.html">HW 2</a><br/>
        <span class="small">Model Score</span>
      </th>
      <th valign="top">
        <a href="hw3.html">HW 3</a><br/>
        <span class="small"><a href="http://en.wikipedia.org/wiki/Spearman's_rank_correlation_coefficient">Spearman's</a></span>
      </th>
      <th valign="top">
        <a href="hw4.html">HW 4</a><br/>
        <span class="small">BLEU</span>
      </th>
      <th valign="top">
        <a href="hw5.html">HW 5</a><br/>
        <span class="small">Accuracy</span>
      </th>
    </tr>
  </thead>
  <tbody>

<script type="text/javascript">
var assNo = 0;

for (i = 0; i < data.length; i++){
  var rank = data[i][0];
  var alias = data[i][1];

  document.write('<tr id="' + alias + '"');
  if (i%2==1){ document.write(' bgcolor="lightblue"'); }
  document.write('>');

  document.write('<td>' + rank + '</td>');
  document.write('<td>' + alias + '</td>');
  document.write('<td align="right">' + data[i][2] + '</td>');
  document.write('<td></td>');
  document.write('<td></td>');
  document.write('<td></td>');
  document.write('<td></td>');
  document.write('<td></td>');

  document.write('</tr>');
}

$("#baseline").css({'background-color': 'PaleGoldenRod'});
$("#default").css({'background-color': 'LightCoral'});
$("#oracle").css({'background-color': 'LimeGreen'});
</script>

  </tbody>
</table>
  </div>

  <div style="position: relative; left: 20px; z-index: -1; margin-top: 10px">
    <h3>Legend</h3>
    <p>
      A value of None indicates that the assignment file was found but
      contained invalid content.
    </p>

    <p>
      The <span style="background-color: LightCoral">light coral</span>
      line is the default system; it is what you should get if you run
      the code that was provided for you.  The row highlighted
      with <span style="background-color: PaleGoldenRod">pale golden
      rod</span> is the baseline you should strive to beat.  Typically,
      this line depicts the effort required to earn a B on the project.
      Additional points will be rewarded for beating the baseline by 
      a significant amount, and the most points will be awarded for placing
      at or near the top of the rankings.
    </p>

    <p>
      The <span style="background-color: LimeGreen">oracle
      system</span> is a metric-aware composite of the best results
      from each student submission.  It is not possible to beat the
      oracle, but it gives you an idea of how much improvement there
      is for your system to find.
    </p>
  </div>

</div>

</div>
</div>

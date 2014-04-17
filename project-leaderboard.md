---
layout: default
img: race
img_link: http://www.flickr.com/photos/nationaalarchief/3198249977/
title: Leaderboard
active_tab: leaderboard
---

   <script src="http://code.jquery.com/jquery-1.7.1.min.js" type="text/javascript"></script>
   <script type="text/javascript" src="http://cs.jhu.edu/~jonny/upenn/mt/project_leaderboard/leaderboard.js"></script>
   <script type="text/javascript" src="http://cs.jhu.edu/~jonny/upenn/mt/project_leaderboard/leaderboard-old.js"></script>

<div class="site">
  <div class="content">
    <h1>Project Leaderboard</h1>
    <div id="course" style="width: 800px">

      <p>
        This page contains the assignment leaderboard.  The
        leaderboard is updated as follows.  Every five minutes, on the
        five-minute mark, every assignment not yet past its due date is
        downloaded according the base URL and assignment number you turned
        in with <a href="assignment0.html">Assignment 0</a>.  The output
        is rescored if it changed.
      </p>

<table>
<!--
  <thead style="background-color: lightgrey">
    <tr>
      <th colspan="3"></th>
      <th colspan="5" align="center">
        Assignments
      </th>
    </tr>
    <tr>
      <th colspan="2">
        Rank
      </th>
      <th>
        Handle
      </th>
      <th valign="top">
        <a href="assignment0.html">#0</a>
      </th>
      <th valign="top">
        <a href="hw1.html">#1</a><br/>
        <span class="small">AER</span>
      </th>
      <th valign="top">
        <a href="hw2.html">#2</a><br/>
        <span class="small">model score</span>
      </th>
      <th valign="top">
        <a href="hw3.html">#3</a><br/>
        <span class="small"><a href="http://en.wikipedia.org/wiki/Spearman's_rank_correlation_coefficient">Spearman's</a></span>
      </th>
      <th valign="top">
        <a href="hw4.html">#4</a><br/>
        <span class="small">BLEU</span>
      </th>
    </tr>
  </thead>
-->
  <tbody>

<script type="text/javascript">
var assNo = 4;

var old_scoreranks = new Array();
for (i = 0; i < olddata.length; i++) {
  var prevscore = (i == 0) ? -1 : olddata[i-1][1+assNo];
  var score = olddata[i][1+assNo];
  if (score != prevscore)
    old_scoreranks[score] = i;
}

var scoreranks = new Array();
for (i=0; i<data.length; i++){
  var user = data[i][0];

  document.write('<tr id="' + user + '"');
  if (i%2==1){ document.write(' bgcolor="lightblue"'); }
  document.write('>');

  var prevscore = (i == 0) ? -1 : data[i-1][1+assNo];
  var score = data[i][1+assNo];
  if (score != prevscore) {
    scoreranks[score] = i;
    document.write('<td>' + (i+1) + '</td>');
  } else {
    document.write('<td></td>');
  }

  var rank = scoreranks[score];
  var oldrank = (score in old_scoreranks) ? old_scoreranks[score] : 100000;

  if (rank > oldrank)
    document.write('<td><img class="arrow" src="img/down.png" /></td>');
  else if (rank < oldrank)
    document.write('<td><img class="arrow" src="img/up.png" /></td>');
  else
    document.write('<td></td>');
  document.write('<td>' + data[i][0] + '</td>');
  for (j = 1; j < data[i].length; j++) {
    document.write('<td align="right">' + data[i][j] + '</td>');
  }
  document.write('</tr>');
}

$("#baseline").css({'background-color': 'PaleGoldenRod'});
$("#default").css({'background-color': 'LightCoral'});
$("#project_name").css({'background-color': 'LimeGreen'});
</script>
  </tbody>
</table>

<!--
  <div style="position: relative; bottom: 20px; z-index: -1; margin-top: 10px">
    <h3>Legend</h3>
    <p>
      A value of -1 indicates that the assignment file was found but
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
-->
</div>


</div>
</div>


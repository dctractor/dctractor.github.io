---
section-type: 2017
title: Groupround
sitemap:
  priority: 1.0
---
<h1 style="text-align: center;">2017年复赛（第二轮）</h1>
<h3 style="text-align: center;">
<form style="text-align: center;" action="" method="POST">
  <input type="radio" name="group" id="groupA" value="A" onClick="groupck();" >
  <label>甲组</label>
  <input type="radio" name="group" id="groupB" value="B" onClick="groupck();" >
  <label>乙组</label>
  <input type="radio" name="group" id="groupC" value="C" onclick="groupck();" >
  <label>丙组</label>
  <input type="radio" name="group" id="groupD" value="D" onclick="groupck();" >
  <label>丁组</label>
</form>
</h3>

<style>
svg {
    width: 100%;
    height: 100%;
    position: center;
}

#chord {
   background-color: white;
}

.group-tick line {
  stroke: #000;
}

.team-label {
  font-size: 25px;
  font-family: "仿宋";
  font-weight: bold;
  text-anchor: middle;
}

.ribbons {
  fill-opacity: 0.5;
}

</style>

<svg id="chord" width="900" height="1000"></svg>

<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://code.jquery.com/jquery-3.1.0.min.js"></script>
<script
  src="https://code.jquery.com/ui/1.12.1/jquery-ui.min.js"
  integrity="sha256-VazP97ZCwtekAsvgPBSUwPFKdrwD3unUfSGVYrahUqU="
  crossorigin="anonymous"></script>

<script>
function groupck() {
$("#chord").empty();
var matrixes = {
           "A": [
                [0,3,3,3],
                [1,0,3,3],
                [1,1,0,3],
                [0,2,1,0]
               ],
           "B": [
                [0,3,3,1],
                [1,0,1,3],
                [1,3,0,3],
                [3,1,2,0]               ],
           "C": [
                [0,1,1,3],
                [3,0,3,3],
                [3,1,0,3],
                [0,1,2,0]
               ],
           "D": [
                [0,1,1,3],
                [3,0,3,1],
                [3,1,0,3],
                [1,3,1,0]
                ]
               };    
var teams = {
        "A": ["清华索赔专家一队（B组）","华山队（A组）","五湖四海队（D组）","武大一队（C组）"],
        "B": ["盛达地产队（C组）","南大一队（D组）","湖北六队（B组）","长江一队（A组）"],
        "C": ["科堡一队（A组）","喜马拉雅生物一队（B组）","川渝二队（C组）","清华索赔专家三队（D组）"],
        "D": ["爱玩一族（D组）","清华索赔专家二队（C组）","喜马拉雅生物二队（A组）","湖北同济队（B组）"]
          }; 
var matrix, team;
    if (document.getElementById("groupA").checked == true) {
        //var matrix = matrixes["A"];
        //var team = teams["A"];     
        var matrix = matrixes[$('input[name="group"]:checked').val()];
        var team = teams[$('input[name="group"]:checked').val()];
    }
    else if (document.getElementById("groupB").checked == true) {
        var matrix = matrixes[$('input[name="group"]:checked').val()];
        var team = teams[$('input[name="group"]:checked').val()];
    }
    else if (document.getElementById("groupC").checked == true) {
        var matrix = matrixes[$('input[name="group"]:checked').val()];
        var team = teams[$('input[name="group"]:checked').val()];
    }
    else {
        var matrix = matrixes[$('input[name="group"]:checked').val()];
        var team = teams[$('input[name="group"]:checked').val()];
        };
//d3.select("input[value=\"A\"]").property("checked", true);
var margin = {top: 30, right: 50, bottom: 30, left: 50};
var width = document.getElementById("chord").getBoundingClientRect().width;
var height = 600;
var outerRadius = Math.min(width, height) * 0.5 - 80;
var innerRadius = outerRadius - 15;
var color = ["#083E77", "#567235", "#8B161C", "#DF7C00"];
var formatValue = d3.formatPrefix(",.0", 1);
var chord = d3.chord()
    .padAngle(0.1)
    .sortGroups(d3.descending);
var arc = d3.arc()
    .innerRadius(innerRadius)
    .outerRadius(outerRadius);
var ribbon = d3.ribbon()
    .radius(innerRadius);    
var color = d3.scaleOrdinal().domain(d3.range(team)).range(color);
var g = d3.select("#chord")
        .append("g")
        .attr("transform", "translate(" + width/2 + "," + height/2 + ")") 
        .datum(chord(matrix));

var group = g.append("g")
    .attr("class", "groups")
    .selectAll("g")
    .data(function(chords) { return chords.groups; })
    .enter()
    .append("g");

group.append("path")
    .style("fill", function(d) { return color(d.index); })
    .style("stroke", function(d) { return d3.rgb(color(d.index)).darker(); })
    .attr("d", arc)
    .on("mouseover", fade(.1))
    .on("mouseout", fade(1));

group.append("title").text(function(d) {
        return groupTip(d);
    });
    
//Add labels to each group
group.append("text")
     .attr("dy", ".25em")
     .attr("class", "team-label")
     .attr("transform", function(d,i) { 
        d.angle = (d.startAngle + d.endAngle) / 2;
        d.name = team[i]; 
        return "rotate(" + (d.angle * 180 / Math.PI) + ")" +
          "translate(0," + -1.1 * (outerRadius + 10) + ")" +
          ((d.angle > Math.PI * 3 / 4 && d.angle < Math.PI * 5 / 4) ? "rotate(180)" : "");
      }) 
      .text(function(d){return d.name;});
      
//Draw the ribbons that go from group to group
var ribbons = g.append("g")
              .attr("class", "ribbons")
              .selectAll("path")
              .data(function(chords) { return chords; })
              .enter().append("path")
              .attr("d", ribbon)
              .style("fill", function(d) { return color(d.target.index); })
              .style("stroke", function(d) { return d3.rgb(color(d.target.index)).darker(); });
     
ribbons.append("title")
       .text(function(d){return chordTip(d);});

//Define tick marks to run along each arc
var groupTick = group.selectAll(".group-tick")
                .data(function(d) { return groupTicks(d, 1) ;})
                .enter().append("g")
                .attr("class", "group-tick")
                .attr("transform", function(d){ 
                return "rotate(" + (d.angle * 180 / Math.PI - 90) + ") translate(" + outerRadius + ",0)"; 
                });

groupTick.append("line")
         .attr("x2",0);

groupTick
  .filter(function(d) { return d.value % 1 === 0; })
  .append("text")
    .attr("x", 8)
    .attr("dy", ".35em")
    .attr("transform", function(d) { return d.angle > Math.PI ? "rotate(180) translate(-16)" : null; })
    .style("text-anchor", function(d) { return d.angle > Math.PI ? "end" : null; })
    .text(function(d) { return formatValue(d.value); });

var groupTick0 = group.selectAll(".group-tick0")
                .data(function(d) { return groupTicks0(d, 1) ;})
                .enter().append("g")
                .attr("class", "group-tick0")
                .attr("transform", function(d){ 
                return "rotate(" + (d.angle * 180 / Math.PI - 90) + ") translate(" + outerRadius + ",0)"; 
                });

groupTick0.append("line")
         .attr("x2",4);
         
groupTick0
  .filter(function(d) { return d.value === 0; })
  .append("text")
    .attr("x", 8)
    .attr("dy", ".35em")
    .attr("transform", function(d) { return d.angle > Math.PI ? "rotate(180) translate(-16)" : null; })
    .style("text-anchor", function(d) { return d.angle > Math.PI ? "end" : null; })
    .text(function(d) { return formatValue(d.value); });

// Returns an array of tick angles and values for a given group and step.
function groupTicks(d, step) {
  var k = (d.endAngle - d.startAngle) / d.value;
  return d3.range(d.value, d.value+1).map(function(value) {
    return {value: value, angle: value * k + d.startAngle,};
  });
}

function groupTicks0(d, step) {
  var k = (d.endAngle - d.startAngle) / d.value;
  return d3.range(0, d.value).map(function(value) {
    return {value: value, angle: value * k + d.startAngle,};
  });
}

function chordTip(d){
  var p = d3.format(".0"), q = d3.formatPrefix(",.0", 1)
     return "比赛结果:\n"
        + team[d.source.index] + "负" + team[d.target.index] + ": 得" + q(d.target.value) + "分\n"
        + team[d.target.index] + "胜" + team[d.source.index] + ": 得" + q(d.source.value) + "分";
}

function groupTip(d) {
        var q = d3.formatPrefix(",.0", 1)
        return  ""+ team[d.index] + "小组赛总得分 :\n" + q(d.value) + "分"
    }

function fade(opacity) {
  return function(d, i) {
    ribbons
        .filter(function(d) {
          return d.source.index != i && d.target.index != i;
        })
      .transition()
        .style("opacity", opacity);
  };
}
};

</script>
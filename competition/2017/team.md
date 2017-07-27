---
section-type: 2017
title: Team
sitemap:
  priority: 1.0
---
<h1 style="text-align: center;"> 2017战队</h1>

<style>
svg {
    width: 100%;
    height: 100%;
    position: center;
}

#cloud {
   background-color: white;
}

</style>

<svg id="cloud" width="900" height="1000"></svg>

<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="/js/d3.layout.cloud.js"></script>
<script>
var margin = {top: 30, right: 50, bottom: 30, left: 50};
var width = document.getElementById("cloud").getBoundingClientRect().width-100;
var height = 600;

var g = d3.select("#cloud")
        .append("g")
        .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

d3.csv("/data/2017/Team_Info.csv",function(data){
var color = d3.scaleOrdinal(d3.schemeCategory20);
var categories = d3.keys(d3.nest().key(function(d) { return d.State; }).map(data));
var fontSize = d3.scalePow().exponent(5).domain([0,1]).range([40,80]);
var fontStyle = d3.scaleLinear().domain([categories]).range(['楷体','仿宋']);
  
var layout = d3.layout.cloud()
      .size([width, height])
      .timeInterval(20)
      .words(data)
      .rotate(function(d) { return 0; })
      .fontSize(function(d,i) { return fontSize(Math.random()); })
      //.fontStyle(function(d,i) { return fontSyle(Math.random()); })
      .fontWeight(["bold"])
      .text(function(d) { return d.Team_CN; })
      .spiral("rectangular") // "archimedean" or "rectangular"
      .on("end", draw)
      .start();

   var wordcloud = g.append("g")
      .attr('class','wordcloud')
      .attr("transform", "translate(" + width/2 + "," + height/2 + ")");
      
   g.append("g")
      .attr("class", "axis")
      .attr("transform", "translate(0," + height + ")")
      .selectAll('text')
      .style('font-size','20px')
      .style('fill',function(d) { return color(d); })
      .style('font','sans-serif');

function draw(words) {
    wordcloud.selectAll("text")
        .data(words)
        .enter().append("text")
        .attr('class','word')
        .style("fill", function(d, i) { return color(i); })
        .style("font-size", function(d) { return d.size + "px"; })
        .style("font-family", function(d) { return d.font; })
        //.style("fill", function(d) { 
            //var paringObject = data.filter(function(obj) { return obj.Team_CN === d.text});
           // return color(paringObject[0].category); 
        //})
        .attr("text-anchor", "middle")
        .attr("transform", function(d) { return "translate(" + [d.x, d.y] + ")rotate(" + d.rotate + ")"; })
        .text(function(d) { return d.text; });
};
  
});
</script> 




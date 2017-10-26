---
section-type: 2017
title: Championship
sitemap:
  priority: 1.0
---
<h1 style="text-align: center;">2017年淘汰赛（第三轮）</h1>

<style>
svg {
    width: 100%;
    height: 100%;
    position: center;
}

#tree {
   background-color: white;
}

.link {
  fill: none;
  stroke: #ccc;
  stroke-width: 2px;
   }
   
.linkl {
  fill: none;
  stroke: #ccc;
  stroke-width: 2px;
   }
   
.text-anchor { font-size: 18px; font-family: "仿宋"; font-weight: bold; }

</style>

<svg id="tree" width="900" height="1000"></svg>

<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://code.jquery.com/jquery-3.1.0.min.js"></script>

<script>
var treeCham =   [{"leftTree": {
                  "name": "清华索赔专家一队（B组 -》甲组）",
                  "value": "7",
                  "level": "navy",
                  "children": [
                              {
                              "name": "清华索赔专家一队（B组 -》甲组）",
                              "value": "14",
                              "level": "navy",
                              "children": [
                                          {
                                          "name": "清华索赔专家一队（B组 -》甲组）",
                                          "value": "7",
                                          "level": "navy"
                                          },
                                          {
                                          "name": "湖北六队（B组 -》 乙组）",
                                          "value": "7",
                                          "level": "pink"
                                          }
                                          ]
                                },
                               {
                               "name": "喜马拉雅生物二队（A组 -》丁组）",
                               "value": "3",
                               "level": "purple",
                               "children": [
                                           {
                                           "name": "喜马拉雅生物二队（A组 -》丁组）",
                                          "value": "8",
                                          "level": "purple"
                                           },
                                           {
                                           "name": "喜马拉雅生物一队（B组 -》丙组）",
                                           "value": "5",
                                           "level": "brown"
                                          }
                                          ]
                                }
                                ]
                    }},
                 {"rightTree": {
                  "name": "盛达地产队（C组 -》乙组）",
                  "value": "10",
                  "level": "red",
                  "children": [
                              {
                              "name": "盛达地产队（C组 -》乙组）",
                              "value": "14",
                              "level": "red",
                              "children": [
                                          {
                                          "name": "盛达地产队（C组 -》乙组）",
                                          "value": "9",
                                          "level": "red"
                                          },
                                          {
                                          "name": "华山队（A组 -》甲组）",
                                          "value": "8",
                                          "level": "green"
                                          }
                                          ]
                                },
                               {
                               "name": "清华索赔专家二队（C组 -》丁组）",
                               "value": "4",
                               "level": "steelblue",
                               "children": [
                                           {
                                           "name": "川渝二队（C组 -》丙组）",
                                          "value": "6",
                                          "level": "yellow"
                                           },
                                           {
                                           "name": "清华索赔专家二队（C组 -》丁组）",
                                           "value": "11",
                                           "level": "steelblue"
                                          }
                                          ]
                                }
                                ]
                    }
       }
      ];

var margin = {top: 30, right: 50, bottom: 30, left: 200};
var width = 960 - margin.left - margin.right;
var height = 1000 - margin.top - margin.bottom;
    
var tree = d3.tree()
	.size([height -400, width - 600]);
var treel = d3.tree()
	.size([height -400, width - 800]);
    
var nodes = d3.hierarchy(treeCham[1].rightTree, function(d) {
    return d.children;
  });
var nodesl = d3.hierarchy(treeCham[0].leftTree, function(d) {
    return d.children;
  });
  
nodes = tree(nodes);
nodesl = treel(nodesl);

var g = d3.select("#tree")
        .append("g")
        .attr("transform", "translate(" + margin.left*4 + "," + margin.top + ")");

var gl = d3.select("#tree")
        .append("g")
        .attr("transform", "translate(" + margin.left*2.5 + "," + margin.top + ")");
        
var link = g.selectAll(".link")
      .data(nodes.descendants().slice(1))
      .enter().append("path")
      .attr("class", "link")
      .style("stroke", function(d) { return d.data.level; })
      .attr("d", function(d) {
       return "M" + d.y + "," + d.x
         + "C" + (d.y + d.parent.y) / 2 + "," + d.x
         + " " + (d.y + d.parent.y) / 2 + "," + d.parent.x
         + " " + d.parent.y + "," + d.parent.x;
       });

var linkl = gl.selectAll(".linkl")
      .data(nodesl.descendants().slice(1))
      .enter().append("path")
      .attr("class", "linkl")
      .style("stroke", function(d) { return d.data.level; })
      .attr("d", function(d) {
       return "M" + d.y + "," + d.x
         + "C" + (d.y + d.parent.y) / 2 + "," + d.x
         + " " + (d.y + d.parent.y) / 2 + "," + d.parent.x
         + " " + d.parent.y + "," + d.parent.x;
       });
       
var node = g.selectAll(".node")
      .data(nodes.descendants())
      .enter().append("g")
      .attr("class", function(d) { return "node" + (d.children ? " node--internal" : " node--leaf");})
      .attr("transform", function(d) {
        return "translate(" + d.y + "," + d.x + ")"; 
      })

var nodel = gl.selectAll(".nodel")
      .data(nodesl.descendants())
      .enter().append("g")
      .attr("class", function(d) { return "node" + (d.children ? " node--internal" : " node--leaf");})
      .attr("transform", function(d) {
        return "translate(" + d.y + "," + d.x + ")"; 
      })
      
  node.append("circle")
      .attr("r", function(d) { return d.data.value; })
      .style("fill", function(d) { return d.data.level; });

  nodel.append("circle")
      .attr("r", function(d) { return d.data.value; })
      .style("fill", function(d) { return d.data.level; });
      
  node.append("text")
      .attr("dy", 3)
      .attr("x", function(d) { return d.children ? (d.data.value ) * -1 : d.data.value})
      .text(function(d) { return d.data.name; })
      .style("text-anchor", function(d) { return d.children ? "middle" : "start"; });
    
  nodel.append("text")
      .attr("dy", 3)
      .attr("x", function(d) { return d.children ? (d.data.value ) * -1 : d.data.value})
      .text(function(d) { return d.data.name; })
      .style("text-anchor", function(d) { return d.children ? "middle" : "end"; });
    

</script>
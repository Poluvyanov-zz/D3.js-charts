<template>
  <div>
 <h2>{{id}}</h2>
    <h2>Количество лицензий</h2>
    <p>Количество приборов по городам</p>
   <svg width="1200" height="600" :id="id"></svg>
  </div>
</template>

<script>

import * as d3 from "d3-3";

export default {
  props: ['data', 'id'],





mounted(){


this.createGraph();

},

methods: {

  
createGraph() {

var data = this.data;

var id = this.id;
console.log('this.id', this.id);

  var div = d3.select("body").append("div").attr("class", "toolTipPieLicenses");

var width = 900,
  height = 600,
  margin = 20,
  radius = Math.min(width - 2 * margin, height - 2 * margin) / 2;

// var color = d3.scale.ordinal()
//     .range(["#98abc5", "#8a89a6", "#7b6888", "#6b486b", "#a05d56"]);

var color = d3.scale.category20b();

var arc = d3.svg.arc()
  .outerRadius(radius)
  .innerRadius(radius - 250); //0

// var arc = d3.svg.arc()
//     .outerRadius(radius)
//     .innerRadius(0);

var pie = d3.layout.pie()
  .sort(null)
  .startAngle(1.1 * Math.PI)
  .endAngle(3.1 * Math.PI)
  .value(function (d) {
    return d.count_licenses;
  });

console.log('32323232323',d3.select("svg#"+id+""));
var svg = d3.select("svg#"+id+"")
  .attr("width", width)
  .attr("height", height)
  .append("g")
  .attr("transform", "translate(" + width / 3 + "," + height / 2 + ")");


var g = svg.selectAll(".arc")
  .data(pie(data))
  .enter().append("g")
  .attr("class", "arc");


var legendRectSize = (radius * 0.05);
var legendSpacing = radius * 0.03;

g.append("path")
  .style("fill", function (d) {
    return color(d.data.regionname);
  })
  .transition().delay(function (d, i) {
    return i * 200;
  }).duration(700)
  .attrTween('d', function (d) {
    var i = d3.interpolate(d.startAngle, d.endAngle); //startAngle +0.1
    return function (t) {
      d.endAngle = i(t);
      return arc(d)
    }
  });
g.append("text")
  .attr("transform", function (d) {
    return "translate(" + arc.centroid(d) + ")";
  })
  .attr("dy", ".35em")
  .transition()
  .delay(6000)
  .text(function (d) {
    return d.data.percent + "%";
  });

d3.selectAll("path").on("mousemove", function (d) {
  div.style("left", d3.event.pageX + 10 + "px");
  div.style("top", d3.event.pageY - 25 + "px");
  div.style("display", "inline-block");
  div.html((d.data.regionname) + "<br>" + (d.data.count_licenses) + "<br>" + (d.data.percent) + "%");
});

d3.selectAll("path").on("mouseout", function (d) {
  div.style("display", "none");
});


// LEGEND

var legend = svg.selectAll('.legend')
  .data(color.domain())
  .enter()
  .append('g')
  .attr('class', 'legend')
  .attr('transform', function (d, i) {
    var height = legendRectSize + legendSpacing;
    var offset = height * color.domain().length / 2;
    var horz = 25 * legendRectSize;
    var vert = i * height - offset;
    return 'translate(' + horz + ',' + vert + ')';
  });

legend.append('rect')
  .attr('width', legendRectSize)
  .attr('height', legendRectSize)
  .style('fill', color)
  .style('stroke', color);

legend.append('text')
  .attr('x', legendRectSize + legendSpacing)
  .attr('y', legendRectSize - legendSpacing)
  .text(function (d) {
    return d;
  });

//d3.select("body").transition().style("background-color", "#d3d3d3");
function type(d) {
  d.count_licenses = +d.count_licenses;
  return d;
}

}

}

}

</script>


  <style>


.arc text {
  font: 10px sans-serif;
  text-anchor: middle;
}

.arc path {
  stroke: #fff;
}

.toolTipPieLicenses {
    position: absolute;
    display: none;
    width: auto;
    height: auto;
    background: none repeat scroll 0 0 white;
    border: 0 none;
    border-radius: 8px 8px 8px 8px;
    box-shadow: -3px 3px 15px #888888;
    color: black;
    font: 12px sans-serif;
    padding: 5px;
    text-align: center;
}

</style>
<template>
  <div>
    <h2>Количество приборов</h2>
    <p>Количество приборов по городам</p>
   <svg width="1200" height="600" id="hist"></svg>
  </div>
</template>

<script>
import * as d3 from "d3";

 var data = [
    {"regionname":"Астана","count_licenses":3120,"count_devices":1797,"percent":"57.6"},
    {"regionname":"Алматы","count_licenses":4128,"count_devices":1978,"percent":"47.9"},
    {"regionname":"Костанай","count_licenses":3122,"count_devices":1322,"percent":"42.3"},
    {"regionname":"\u0410\u043a\u043c\u043e\u043b\u0438\u043d\u0441\u043a\u0430\u044f", "count_licenses":2798,"count_devices":1137,"percent":"40.6"},
    {"regionname":"\u042e\u041a\u041e","count_licenses":2443,"count_devices":929,"percent":"38.0"},
    {"regionname":"Кызылопдинская","count_licenses":1106,"count_devices":392,"percent":"35.4"},
    {"regionname":"\u0410\u0442\u044b\u0440\u0430\u0443\u0441\u043a\u0430\u044f","count_licenses":2043,"count_devices":712,"percent":"34.9"},
    {"regionname":"\u0433. \u0410\u0441\u0442\u0430\u043d\u0430","count_licenses":3038,"count_devices":1015,"percent":"33.4"},
    {"regionname":"\u041c\u0430\u043d\u0433\u0438\u0441\u0442\u0430\u0443\u0441\u043a\u0430\u044f","count_licenses":1599,"count_devices":523,"percent":"32.7"},
    {"regionname":"\u0410\u043b\u043c\u0430\u0442\u0438\u043d\u0441\u043a\u0430\u044f","count_licenses":5134,"count_devices":1672,"percent":"32.6"},
    {"regionname":"\u0412\u041a\u041e","count_licenses":6154,"count_devices":1846,"percent":"30.0"},
    {"regionname":"\u0417\u041a\u041e","count_licenses":2364,"count_devices":694,"percent":"29.4"},
    {"regionname":"\u0421\u041a\u041e","count_licenses":3277,"count_devices":913,"percent":"27.9"},
    {"regionname":"\u0433. \u0410\u043b\u043c\u0430\u0442\u044b","count_licenses":5150,"count_devices":1217,"percent":"23.6"},
    {"regionname":"\u0410\u043a\u0442\u044e\u0431\u0438\u043d\u0441\u043a\u0430\u044f","count_licenses":2620,"count_devices":504,"percent":"19.2"}
];


export default {

props: ['data'],

mounted(){

this.createGraph();

},

methods: {

createGraph() {

  var data = this.data;
  
  var svg = d3.select("svg#hist"),
    margin = {
      top: 20,
      right: 20,
      bottom: 30,
      left: 100
    },
    width = 1200 - margin.left - margin.right,
    height = 600 - margin.top - margin.bottom;

  var tooltip = d3.select("body").append("div").attr("class", "toolTip");

  var x = d3.scaleLinear().range([0, width]);

  var y = d3.scaleBand().range([height, 0]);

  var g = svg.append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");


  var color;

  function getColor(percent) {
    if (percent < 49) {
      color = "not_many"
    }

    if (percent > 49 && percent < 75) {
      color = "normal"
    }

    if (percent > 75) {
      color = "many"
    }

    return color;
  }


  data.sort(function (a, b) {
    return a.count_devices - b.count_devices;
  });

  x.domain([0, d3.max(data, function (d) {
    return d.count_devices;
  })]);

  y.domain(data.map(function (d) {
    return d.regionname;
  })).padding(0.1);

  g.append("g")
    .attr("class", "x axis")
    .attr("transform", "translate(0," + height + ")")
    .call(d3.axisBottom(x).ticks(7).tickFormat(function (d) {
      return parseInt(d / 1);
    }).tickSizeInner([-height])); ////////////////НИЖНЯЯ ПОЛОСКА 

  g.append("g")
    .attr("class", "y axis")
    .call(d3.axisLeft(y)); //ЛЕВАЯ ПООСКА


  g.selectAll(".bar")
    .data(data)
    .enter().append("g").attr("class", "line");

  g.selectAll(".line")
    .append("rect")
    .attr("class", "bar")
    .attr("class", function (d) {
      return getColor(d.percent);
    })
    .attr("x", 0)
    .attr("height", y.bandwidth())
    .attr("y", function (d) {
      return y(d.regionname);
    })
    .on("mouseover", function (d) {
      d3.select(this).attr("class", "hover");
    })
    .on("mouseout", function (d) {
      d3.select(this).attr("class", function (d) {
        return getColor(d.percent);
      });
    })
    .transition().duration(2000)
    .delay(function (d, i) {
      return i * 150;
    })
    .attr("width", function (d) {
      return x(d.count_devices);
    });


  g.selectAll(".line")
    .append("text")
    .text(function (d) {
      return (d.count_devices) + " (" + (d.percent) + "%)";
    })
    .attr("class", "text")
    .attr("x", function (d) {
      return x(d.count_devices) - 10;
    })
    .transition().duration(2000)
    .delay(function (d, i) {
      return i * 150;
    })
    .attr("height", y.bandwidth())
    .attr("y", function (d) {
      return y(d.regionname) + 13;
    })
    .attr("dy", ".35em")
    .attr("width", function (d) {
      return x(d.count_devices);
    });
}

}

}

</script>


  <style>

body {
  margin: 15px;
}

.bar {}

.many {
  fill: #10ab0e;
}

.normal {
  fill: #c1be45;
}

.not_many {
  fill: #db3434;
}

.bar text {
  font-size: 12px;
  text-align: right;
  color: #f0f8ff;
}

.axis path,
.axis line {
  fill: none;
  stroke: #D4D8DA;
  stroke-width: 1px;
  shape-rendering: crispEdges;
}

.x path {
  /*display: none;*/
}

.toolTip {
  position: absolute;
  display: none;
  min-width: 100px;
  height: auto;
  background: none repeat scroll 0 0 #ffffff;
  border: 1px solid #6F257F;
  padding: 14px;
  text-align: center;
}

.y.axis g {}

.line text {
  fill: white;
  font: 10px sans-serif;
  text-anchor: end;
}

.hover {
  fill: #004f61;
}

</style>
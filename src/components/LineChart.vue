<template>
   <div>
      <h2>Динамика приборов</h2>
      <p>Количество приборов по датам</p>
      <svg id="line"></svg>
   </div>
</template>

<script>

import * as d3 from "d3-3";

var data = [
{"date": "2017-10-01", "count_devices": "1000"},
{"date": "2017-10-02", "count_devices": "1822"},
{"date": "2017-10-03", "count_devices": "1010"},
{"date": "2017-10-04", "count_devices": "512"},
{"date": "2017-10-05", "count_devices": "1080"},
{"date": "2017-10-06", "count_devices": "1200"},
{"date": "2017-10-07", "count_devices": "695"},
{"date": "2017-10-08", "count_devices": "800"},
{"date": "2017-10-09", "count_devices": "1200"},
{"date": "2017-10-10", "count_devices": "200"},
{"date": "2017-10-11", "count_devices": "200"},
{"date": "2017-10-12", "count_devices": "200"},
{"date": "2017-10-13", "count_devices": "200"},
{"date": "2017-10-14", "count_devices": "200"},
{"date": "2017-10-15", "count_devices": "200"},
{"date": "2017-10-16", "count_devices": "454"},
{"date": "2017-10-17", "count_devices": "200"},
{"date": "2017-10-18", "count_devices": "210"},
{"date": "2017-10-19", "count_devices": "4512"},
{"date": "2017-10-20", "count_devices": "200"},
{"date": "2017-10-21", "count_devices": "200"},
{"date": "2017-10-22", "count_devices": "1475"},
{"date": "2017-10-23", "count_devices": "200"},
{"date": "2017-10-24", "count_devices": "200"},
{"date": "2017-10-25", "count_devices": "1245"},
{"date": "2017-10-26", "count_devices": "212"},
{"date": "2017-10-27", "count_devices": "200"},
{"date": "2017-10-28", "count_devices": "200"},
{"date": "2017-10-29", "count_devices": "200"},
{"date": "2017-10-30", "count_devices": "1212"},
{"date": "2017-10-31", "count_devices": "200"},
{"date": "2017-11-01", "count_devices": "124"},
{"date": "2017-11-02", "count_devices": "2121"}
];


export default {

props: ['data'],

mounted(){

this.createGraph();

},

methods: {

createGraph() {
	
var data = this.data;

	console.log(d3.version);


	// Set the dimensions of the canvas / graph
	var
		margin = {
			top: 30,
			right: 20,
			bottom: 30,
			left: 50
		},
		width = 1500 - margin.left - margin.right,
		height = 270 - margin.top - margin.bottom;


	// Parse the date / time
	var parseDate = d3.time.format("%Y-%m-%d").parse;


	// Set the ranges
	var x = d3.time.scale().range([0, width]);
	var y = d3.scale.linear().range([height, 0]);

	// Define the axes
	var xAxis = d3.svg.axis().scale(x)
		.orient("bottom").ticks(10);

	var yAxis = d3.svg.axis().scale(y)
		.orient("left").ticks(6);


	// Define the line
	var valueline = d3.svg.line()

		.x(function (d) {
			return x(d.date);
		})
		.y(function (d) {
			return y(d.count_devices);
		});

	// Adds the svg canvas
	var svg = d3.select("svg#line")
		.attr("width", width + margin.left + margin.right)
		.attr("height", height + margin.top + margin.bottom)
		.style("display", "block").style("margin", "auto")
		.append("g")

		.attr("transform", "translate(" + margin.left + "," + margin.top + ")");

	// Get the data


	// Add the clip path.
	svg.append("clipPath")
		.attr("id", "clip")
		.append("rect")
		.attr("width", width)
		.attr("height", height);


	data.forEach(function (d) {
		d.date = parseDate(d.date);
		d.count_devices = +d.count_devices;
	});

	// Scale the range of the data
	x.domain(d3.extent(data, function (d) {
		return d.date;
	}));
	y.domain([0, d3.max(data, function (d) {
		return d.count_devices;
	})]).nice();


	// Add the valueline path.
	// svg.append("path")
	//        .attr("class", "line")
	//        .attr("d", valueline(data));

	var colors = d3.scale.category10();

	svg.append("path")
		.attr("class", "line")
		.attr('clip-path', 'url(#clip)')
		.attr("d", valueline(data));

	// Add the X Axis
	svg.append("g")
		.attr("class", "x axis")
		.attr("transform", "translate(0," + height + ")")
		.call(xAxis);

	// Add the Y Axis
	svg.append("g")
		.attr("class", "y axis")
		.call(yAxis);


	/* Add 'curtain' rectangle to hide entire graph */
	var curtain = svg.append('rect')
		.attr('x', -1 * width)
		.attr('y', -1 * (height - 2))
		.attr('height', height - 10)
		.attr('width', width)
		.attr('class', 'curtain')
		.attr('transform', 'rotate(180)')
		.style('fill', '#FFFFFF');

	var t = svg.transition()
		.delay(750)
		.duration(4000)
		.ease('linear')
		.each('end', function () {
			d3.select('line.guide')
				.transition()
				.style('opacity', 0)
				.remove()
		});

	t.select('rect.curtain')
		.attr('width', 0);
	t.select('line.guide')
		.attr('transform', 'translate(' + width + ', 0)');


}

	}

}

</script>


<style>

body {
	font: 12px Arial;
}

path {
	stroke: steelblue;
	stroke-width: 2;
	fill: none;
}

.axis path,
.axis line {
	fill: none;
	stroke: grey;
	stroke-width: 1;
	shape-rendering: crispEdges;
}

.curtain {}

</style>
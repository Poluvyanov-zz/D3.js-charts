<template>
   <div class="hello">

    <div class="row">
      <div class="col-md-12"><h1>{{ msg }}</h1></div>     
       <div class="col-md-12">
         <hist-chart :data="histData"></hist-chart>
       </div>
       <div class="col-md-6">
        <pie-chart-licenses class="pie-chart-licenses" :data="histData" :id="licence"></pie-chart-licenses>
      </div>
       <div class="col-md-6">
        <pie-chart-devices class="pie-chart-devices" :data="histData" :id="device"></pie-chart-devices>
      </div>
       <div class="col-md-12">
        <line-chart class="line-chart" :data="lineData"></line-chart>
      </div>
    </div>



     
   </div>
</template>

<script>
import Vue from 'vue';
export default {
  components: {
    'hist-chart': require('./HistChart.vue').default,
    'line-chart': require('./LineChart.vue').default,
    'pie-chart-licenses': require('./PieChartLicenses.vue').default,
    'pie-chart-devices': require('./PieChartDevices.vue').default,
  },
  name: 'HelloWorld',
  data() {
    return {
      msg: 'Графики',
      licence: 'licence',
      device: 'device',
      histData: [
        {"regionname":"Астана","count_licenses":3120,"count_devices":3120,"percent":"57.6"},
        {"regionname":"Алматы","count_licenses":4128,"count_devices":4128,"percent":"47.9"},
        {"regionname":"Костанай","count_licenses":3122,"count_devices":520,"percent":"42.3"},
        {"regionname":"\u0410\u043a\u043c\u043e\u043b\u0438\u043d\u0441\u043a\u0430\u044f", "count_licenses":456,"count_devices":1137,"percent":"40.6"},
        {"regionname":"\u042e\u041a\u041e","count_licenses":2443,"count_devices":929,"percent":"38.0"},
        {"regionname":"Кызылопдинская","count_licenses":1106,"count_devices":392,"percent":"35.4"},
        {"regionname":"\u0410\u0442\u044b\u0440\u0430\u0443\u0441\u043a\u0430\u044f","count_licenses":6541,"count_devices":712,"percent":"34.9"},
        {"regionname":"\u0433. \u0410\u0441\u0442\u0430\u043d\u0430","count_licenses":3038,"count_devices":1015,"percent":"33.4"},
        {"regionname":"\u041c\u0430\u043d\u0433\u0438\u0441\u0442\u0430\u0443\u0441\u043a\u0430\u044f","count_licenses":1599,"count_devices":523,"percent":"32.7"},
        {"regionname":"\u0410\u043b\u043c\u0430\u0442\u0438\u043d\u0441\u043a\u0430\u044f","count_licenses":114,"count_devices":1672,"percent":"32.6"},
        {"regionname":"\u0412\u041a\u041e","count_licenses":6154,"count_devices":1846,"percent":"30.0"},
        {"regionname":"\u0417\u041a\u041e","count_licenses":2364,"count_devices":694,"percent":"29.4"},
        {"regionname":"\u0421\u041a\u041e","count_licenses":3277,"count_devices":913,"percent":"27.9"},
        {"regionname":"\u0433. \u0410\u043b\u043c\u0430\u0442\u044b","count_licenses":5150,"count_devices":1217,"percent":"23.6"},
        {"regionname":"\u0410\u043a\u0442\u044e\u0431\u0438\u043d\u0441\u043a\u0430\u044f","count_licenses":2620,"count_devices":504,"percent":"19.2"}
      ],
    lineData: [
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
      ]
                
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

h1,h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.line-chart {
  margin: 125px 0 0 0;
}

.pie-chart-licenses {
  margin: 125px 0 0 0;
}

.pie-chart-devices {
  margin: 125px 0 0 0;
}


</style>

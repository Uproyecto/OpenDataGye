<template>
  <div id="Inamhi">
    <v-container justify="center" align="center">
      <v-row class="mb-6">
        <v-col class="pa-2">
          <v-menu
            ref="menu_initial_date"
            v-model="menu_initial_date"
            :close-on-content-click="false"
            :return-value.sync="initial_date"
            transition="scale-transition"
            offset-y
            min-width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="format_date_initial"
                label="Start Date"
                prepend-icon="mdi-event"
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="initial_date"
              @input="
                $refs.menu_initial_date.save(initial_date);
                menu_initial_date = false;
              "
            >
              <v-spacer></v-spacer>
            </v-date-picker>
          </v-menu>
        </v-col>
        <v-col class="pa-2">
          <v-menu
            ref="menu_end_date"
            v-model="menu_end_date"
            :close-on-content-click="false"
            :return-value.sync="end_date"
            transition="scale-transition"
            offset-y
            min-width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="format_date_end"
                label="End Date"
                prepend-icon="mdi-event"
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="end_date"
              @input="
                $refs.menu_end_date.save(end_date);
                menu_end_date = false;
              "
            >
              <v-spacer></v-spacer>
            </v-date-picker>
          </v-menu>
        </v-col>
        <v-col class="pa-2">
          <v-select :items="items" label="Select" v-model="selector"></v-select>
        </v-col>
        <v-col class="pa-2" justify="center" align="center" align-self="center">
          <!-- <v-btn small color="primary" @click="select">Buscar</v-btn> -->
          <v-row justify="center" align="center" align-self="center">
            <v-col cols="8" sm="4" md="4">
              <v-btn small color="primary" @click="select">Search</v-btn>
            </v-col>
            <v-col cols="8" sm="4" md="4">
              <v-btn
                color="blue"
                class="ma-2 white--text"
                fab
                @click="download"
              >
                <v-icon dark>mdi-cloud-download</v-icon>
              </v-btn>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
      <v-card class="rounded-lg" tile>
        <v-list-item two-line>
          <v-list-item-content>
            <v-list-item-title>{{ this.selector.name }}</v-list-item-title>
            <v-list-item-subtitle>{{
              end_date | moment("dddd, MMMM Do YYYY")
            }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-card-text>
          <v-row align="center">
            <v-col class="display-3" cols="6"
              >{{ resume.temperatura_inst }}&deg;C</v-col
            >
            <v-col cols="6">
              <v-img
                src="https://cdn.vuetifyjs.com/images/cards/sun.png"
                alt="Sunny image"
                width="92"
              ></v-img>
            </v-col>
          </v-row>
        </v-card-text>
        <v-list class="transparent">
          <v-row>
            <v-col cols="18" sm="6">
              <v-divider></v-divider>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-white-balance-sunny</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Temperature Max/Min</v-list-item-title>

                <v-list-item-subtitle class="text-right"
                  >{{ resume.temperatura_min }}/{{
                    resume.temperatura_max
                  }}</v-list-item-subtitle
                >
              </v-list-item>
            </v-col>
            <v-col cols="18" sm="6">
              <v-divider></v-divider>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-white-balance-sunny</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Instant Humidity (%)</v-list-item-title>
                <v-list-item-subtitle class="text-right">{{
                  resume.humedad_inst
                }}</v-list-item-subtitle>
              </v-list-item>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="18" sm="6">
              <v-divider></v-divider>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-white-balance-sunny</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Humidity Max/Min</v-list-item-title>
                <v-list-item-subtitle class="text-right"
                  >{{ resume.humedad_max }}/{{
                    resume.humedad_min
                  }}</v-list-item-subtitle
                >
              </v-list-item>
            </v-col>
            <v-col cols="18" sm="6">
              <v-divider></v-divider>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-white-balance-sunny</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Precipitation (mm)</v-list-item-title>
                <v-list-item-subtitle class="text-right">{{
                  resume.precipitacion
                }}</v-list-item-subtitle>
              </v-list-item>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="18" sm="6">
              <v-divider></v-divider>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-white-balance-sunny</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Presure (mm)</v-list-item-title>
                <v-list-item-subtitle class="text-right">{{
                  resume.presion_atm
                }}</v-list-item-subtitle>
              </v-list-item>
            </v-col>
            <v-col cols="18" sm="6">
              <v-divider></v-divider>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-white-balance-sunny</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Wind Speed(m/s)</v-list-item-title>
                <v-list-item-subtitle class="text-right">{{
                  resume.velocidad_viento
                }}</v-list-item-subtitle>
              </v-list-item>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="18" sm="6">
              <v-divider></v-divider>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-white-balance-sunny</v-icon>
                </v-list-item-icon>
                <v-list-item-title>Wind direction</v-list-item-title>
                <v-list-item-subtitle class="text-right">{{
                  resume.direccion_viento
                }}</v-list-item-subtitle>
              </v-list-item>
            </v-col>
            <v-col cols="18" sm="6"></v-col>
          </v-row>
        </v-list>
      </v-card>

      <v-container fluid v-show="current_type === 'METEOROLOGICA'">
        <v-row>
          <v-col cols="18" sm="6">
            <v-card class="rounded-lg" tile>
              <v-card-title>Atmospheric Pressure</v-card-title>
              <line-chart
                :chartData="presion_atmosferica_data"
                :options="presion_atmosferica_options"
              />
            </v-card>
          </v-col>
          <v-col cols="18" sm="6">
            <v-card class="rounded-lg" tile>
              <v-card-title>Relative Humidity of air</v-card-title>
              <line-chart
                :chartData="humedad_relativa_aire_data"
                :options="humedad_relativa_aire_options"
              />
            </v-card>
          </v-col>
        </v-row>
      </v-container>
      <v-container fluid v-show="current_type === 'METEOROLOGICA'">
        <v-row>
          <v-col cols="18" sm="6">
            <v-card class="rounded-lg" tile>
              <v-card-title>Air Temperature</v-card-title>
              <line-chart
                :chartData="temperatura_aire_data"
                :options="temperatura_options"
              />
            </v-card>
          </v-col>
          <v-col cols="18" sm="6"></v-col>
        </v-row>
      </v-container>
      <v-container fluid v-show="current_type === 'HIDROLOGICA'">
        <v-row>
          <v-col cols="18" sm="6">
            <v-card class="rounded-lg" tile>
              <v-card-title>Water Level</v-card-title>
              <bar-chart
                :chartData="nive_agua_data"
                :options="nive_agua_options"
              />
            </v-card>
          </v-col>
          <v-col cols="18" sm="6"></v-col>
        </v-row>
      </v-container>

      <v-data-table
        :loading="table_loading"
        :headers="headers"
        :items="data"
        :items-per-page="10"
        class="elevation-1"
        style="margin-top: 40px"
      >
        <template v-slot:top>
          <h3>Undated Data (Real Time): {{ datos_perdidos }}</h3>
        </template>
        <template v-slot:item.fecha="{ item }">
          <v-chip dark>{{ item.fecha | moment("DD/MM, h a") }}</v-chip>
        </template>
      </v-data-table>
    </v-container>
  </div>
</template>

<script>
import LineChart from "../../components/charts/LineChart";
import BarChart from "../../components/charts/BarChart";
import dayjs from "dayjs";
import moment from "moment";
import _ from "lodash";
import Papa from "papaparse";
dayjs.locale("es");

export default {
  components: {
    LineChart,
    BarChart,
  },
  name: "Inamhi",
  methods: {
    download() {
      const cleanData = [];
      for (const data of this.data) {
        let tmp = [];
        for (const key in data) {
          tmp.push(data[key]);
        }
        cleanData.push(tmp);
      }
      const csv = Papa.unparse(
        {
          fields: [
            "Date - Time (GMT-5)",
            "Relative Humidity of air (%) INST",
            "Relative Humidity of air (%) MAX",
            "Relative Humidity of air (%) MIN",
            "Precipitation (mm) SUM",
            "Atmospheric Pressure (hPa) INST",
            "Air Temperature (°C) INST",
            "Air Temperature (°C) MAX",
            "Air Temperature (°C) MIN",
            "Wind direction INST",
            "Wind Speed (m/s) INST",
          ],
          data: cleanData,
        },
        {}
      );
      const csvData = new Blob([csv], { type: "text/csv;charset=utf-8;" });
      const url = window.URL.createObjectURL(csvData);
      let anchor = document.createElement("a");
      anchor.download = `${this.selector.text}.csv`;
      anchor.href = url;
      anchor.click();
    },
    getByRange(date) {
      console.log(date);
    },
    presionAtmosfericaChar(labels, data) {
      return {
        labels: _.reverse(labels),
        datasets: [
          {
            label: "Atmospheric Pressure",
            backgroundColor: "#ffce56",
            borderColor: "#ffce56",
            data: _.reverse(data),
            fill: false,
          },
        ],
      };
    },
    precipitacionChar(labels, data) {
      return {
        labels: _.reverse(labels),
        datasets: [
          {
            label: "Precipitation (mm)",
            backgroundColor: "#36a2eb",
            borderColor: "#36a2eb",
            data: _.reverse(data),
            fill: false,
          },
        ],
      };
    },
    nivelAguaChar(labels, data) {
      return {
        labels: _.reverse(labels),
        datasets: [
          {
            label: "Nivel agua(INST)",
            backgroundColor: "#36a2eb",
            borderColor: "#36a2eb",
            data: _.reverse(data),
            fill: false,
          },
        ],
      };
    },
    humedadRelativaAireChar(labels, datacharinst, datacharmax, datacharmin) {
      return {
        labels: _.reverse(labels),
        datasets: [
          {
            label: "Relative Humidity of air (%) INST",
            backgroundColor: "#ffce56",
            borderColor: "#ffce56",
            data: _.reverse(datacharinst),
            fill: false,
          },
          {
            label: "Relative Humidity of air (%) MAX",
            backgroundColor: "#ff6384",
            borderColor: "#ff6384",
            data: _.reverse(datacharmax),
            fill: false,
          },
          {
            label: "Relative Humidity of air (%) MIN",
            backgroundColor: "#36a2eb",
            borderColor: "#36a2eb",
            data: _.reverse(datacharmin),
            fill: false,
          },
        ],
      };
    },
    temperaturaAireChar(labels, datacharinst, datacharmax, datacharmin) {
      return {
        labels: _.reverse(labels),
        datasets: [
          {
            label: "Air Temperature (°C) INST",
            backgroundColor: "#ffce56",
            borderColor: "#ffce56",
            data: _.reverse(datacharinst),
            fill: false,
          },
          {
            label: "Air Temperature (°C) MAX",
            backgroundColor: "#ff6384",
            borderColor: "#ff6384",
            data: _.reverse(datacharmax),
            fill: false,
          },
          {
            label: "Air Temperature (°C) MIN",
            backgroundColor: "#36a2eb",
            borderColor: "#36a2eb",
            data: _.reverse(datacharmin),
            fill: false,
          },
        ],
      };
    },
    select() {
      this.table_loading = true;
      this.current_type = this.selector.type;
      this.$store
        .dispatch("inamhi/inamhi_page", {
          id_esta: this.selector.id_esta,
          name: this.selector.name,
          type: this.selector.type,
          init_date: this.format_date_initial,
          end_date: this.format_date_end,
        })
        .then((data) => {
          this.table_loading = false;
          this.inamhi = data;
        })
        .catch((err) => {
          this.table_loading = false;
          console.error(err);
        });
    },
  },
  mounted() {
    this.select();
  },
  computed: {
    format_date_initial() {
      const m = moment();
      const roundDown = m.startOf("hour");
      return `${this.initial_date} ${roundDown.format("HH:mm:ss")}`;
    },
    format_date_end() {
      const m = moment();
      const roundUp =
        m.minute() || m.second() || m.millisecond()
          ? m.add(1, "hour").startOf("hour")
          : m.startOf("hour");
      return `${this.end_date} ${roundUp.format("HH:mm:ss")}`;
    },
    presion_atmosferica_data() {
      const data = this.$store.getters["inamhi/inamhi_page_get"];
      const labels = [];
      const datachar = [];
      for (let index = 0; index < data.length; index++) {
        const head = {};
        for (let y = 0; y < data[index].headers.length; y++) {
          if (y === 0) {
            head.fecha = data[index].data[y];
            labels.push(head.fecha);
            continue;
          }
          if (data[index].headers[y] === "PRESION ATMOSFERICA (hPa) INST") {
            datachar.push(data[index].data[y]);
          }
        }
      }
      const res = this.presionAtmosfericaChar(labels, datachar);
      return res;
    },
    resume() {
      const data = this.$store.getters["inamhi/inamhi_page_get"];
      try {
        return {
          temperatura_max: _.last(data).data[7],
          temperatura_min: _.last(data).data[8],
          temperatura_inst: _.last(data).data[6],
          humedad_inst: _.last(data).data[1],
          humedad_max: _.last(data).data[2],
          humedad_min: _.last(data).data[3],
          precipitacion: _.last(data).data[4],
          presion_atm: _.last(data).data[5],
          velocidad_viento: _.last(data).data[9],
          direccion_viento: _.last(data).data[10],
        };
      } catch (error) {
        return {
          temperatura_max: 0,
          temperatura_min: 0,
          temperatura_inst: 0,
          humedad_inst: 0,
          humedad_max: 0,
          humedad_min: 0,
          precipitacion: 0,
          presion_atm: 0,
          velocidad_viento: 0,
          direccion_viento: 0,
        };
      }
    },
    datos_perdidos() {
      const data = this.$store.getters["inamhi/inamhi_page_get"];
      let cont = 0;
      for (let row of data) {
        for (let actualdata of row.data) {
          if (_.isEmpty(actualdata)) {
            cont++;
          }
        }
      }
      return cont;
    },
    temperatura_aire_data() {
      const data = this.$store.getters["inamhi/inamhi_page_get"];
      const labels = [];
      const datacharinst = [];
      const datacharmax = [];
      const datacharmin = [];
      for (let index = 0; index < data.length; index++) {
        const head = {};
        for (let y = 0; y < data[index].headers.length; y++) {
          if (y === 0) {
            head.fecha = data[index].data[y];
            labels.push(head.fecha);
            continue;
          }
          if (data[index].headers[y] === "TEMPERATURA AIRE (&#xB0;C) INST") {
            datacharinst.push(data[index].data[y]);
          }
          if (data[index].headers[y] === "TEMPERATURA AIRE (&#xB0;C) MAX") {
            datacharmax.push(data[index].data[y]);
          }
          if (data[index].headers[y] === "TEMPERATURA AIRE (&#xB0;C) MIN") {
            datacharmin.push(data[index].data[y]);
          }
        }
      }
      const res = this.temperaturaAireChar(
        labels,
        datacharinst,
        datacharmax,
        datacharmin
      );
      return res;
    },
    humedad_relativa_aire_data() {
      const data = this.$store.getters["inamhi/inamhi_page_get"];
      const labels = [];
      const datacharinst = [];
      const datacharmax = [];
      const datacharmin = [];
      for (let index = 0; index < data.length; index++) {
        const head = {};
        for (let y = 0; y < data[index].headers.length; y++) {
          if (y === 0) {
            head.fecha = data[index].data[y];
            labels.push(head.fecha);
            continue;
          }
          if (data[index].headers[y] === "HUMEDAD RELATIVA DEL AIRE (%) INST") {
            datacharinst.push(data[index].data[y]);
          }
          if (data[index].headers[y] === "HUMEDAD RELATIVA DEL AIRE (%) MAX") {
            datacharmax.push(data[index].data[y]);
          }
          if (data[index].headers[y] === "HUMEDAD RELATIVA DEL AIRE (%) MIN") {
            datacharmin.push(data[index].data[y]);
          }
        }
      }
      const res = this.humedadRelativaAireChar(
        labels,
        datacharinst,
        datacharmax,
        datacharmin
      );
      return res;
    },
    precipitacion_data() {
      const data = this.$store.getters["inamhi/inamhi_page_get"];
      const labels = [];
      const datachar = [];
      for (let index = 0; index < data.length; index++) {
        const head = {};
        for (let y = 0; y < data[index].headers.length; y++) {
          if (y === 0) {
            head.fecha = data[index].data[y];
            labels.push(head.fecha);
            continue;
          }
          if (data[index].headers[y] === "PRECIPITACION (mm) SUM") {
            datachar.push(data[index].data[y]);
          }
        }
      }
      const res = this.precipitacionChar(labels, datachar);
      return res;
    },
    nive_agua_data() {
      const data = this.$store.getters["inamhi/inamhi_page_get"];
      const labels = [];
      const datachar = [];
      for (let index = 0; index < data.length; index++) {
        const head = {};
        for (let y = 0; y < data[index].headers.length; y++) {
          if (y === 0) {
            head.fecha = data[index].data[y];
            labels.push(head.fecha);
            continue;
          }
          if (data[index].headers[y] === "NIVEL DEL AGUA (m) INST") {
            datachar.push(data[index].data[y]);
          }
        }
      }
      const res = this.nivelAguaChar(labels, datachar);
      return res;
    },
    data() {
      const data = this.$store.getters["inamhi/inamhi_page_get"];
      const parse = [];
      const labels = [];
      const datachar = [];
      for (let index = 0; index < data.length; index++) {
        const head = {};
        for (let y = 0; y < data[index].headers.length; y++) {
          if (y === 0) {
            head.fecha = data[index].data[y];
            labels.push(head.fecha);
            continue;
          }
          if (data[index].headers[y] === "PRESION ATMOSFERICA (hPa) INST") {
            datachar.push(data[index].data[y]);
          }
          head[data[index].headers[y]] = data[index].data[y];
        }
        parse.push(head);
      }
      return parse;
    },
    headers() {
      const data = this.$store.getters["inamhi/inamhi_page_get"];
      const parse = [];
      const text_headers = [
        "Date - Time (GMT-5)",
        "Relative Humidity of air (%) INST",
        "Relative Humidity of air (%) MAX",
        "Relative Humidity of air (%) MIN",
        "Precipitation (mm) SUM",
        "Atmospheric Pressure (hPa) INST",
        "Air Temperature (°C) INST",
        "Air Temperature (°C) MAX",
        "Air Temperature (°C) MIN",
        "Wind direction INST",
        "Wind Speed (m/s) INST",
      ];
      let cont = 0;
      if (!data[0]) {
        return [];
      }
      for (const head of data[0].headers) {
        if (cont === 0) {
          parse.push({
            text: text_headers[cont],
            value: "fecha",
          });
          cont++;
          continue;
        }
        parse.push({
          text: text_headers[cont],
          value: head,
        });
        cont++;
      }
      return parse;
    },
  },
  data() {
    return {
      table_loading: false,
      menu_initial_date: false,
      menu_end_date: false,
      initial_date: moment().subtract(1, "d").format("YYYY-MM-DD"),
      end_date: moment().format("YYYY-MM-DD"),
      selector: {
        text: "GUAYAQUIL (FACULTAD CCNN) - METEOROLOGICA",
        value: {
          id_esta: 63813,
          name: "GUAYAQUIL (FACULTAD CCNN)",
          type: "METEOROLOGICA",
        },
        id_esta: 63813,
        name: "GUAYAQUIL (FACULTAD CCNN)",
        type: "METEOROLOGICA",
      },
      current_type: "METEOROLOGICA",
      items: [
        {
          text: "GUAYAQUIL (FACULTAD CCNN) - METEOROLOGICA",
          value: {
            id_esta: 63813,
            name: "GUAYAQUIL (FACULTAD CCNN)",
            type: "METEOROLOGICA",
          },
        },
        {
          text: "COE-MONTE BELLO - METEOROLOGICA",
          value: {
            id_esta: 63806,
            name: "COE-MONTE BELLO",
            type: "METEOROLOGICA",
          },
        },
        {
          text: "PUERTO HONDO - METEOROLOGICA",
          value: {
            id_esta: 63802,
            name: "PUERTO HONDO",
            type: "METEOROLOGICA",
          },
        },
      ],
      presion_atmosferica_options: {
        responsive: true,
        title: {
          display: false,
          text: "PRESION ATMOSFERICA",
        },
        tooltips: {
          mode: "index",
          intersect: false,
        },
        hover: {
          mode: "nearest",
          intersect: true,
        },
        scales: {
          xAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "Fecha",
              },
            },
          ],
          yAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "PRESION ATMOSFERICA(hPa) INST",
              },
            },
          ],
        },
      },
      humedad_relativa_aire_options: {
        responsive: true,
        title: {
          display: false,
          text: "HUMEDAD RELATIVA DEL AIRE",
        },
        tooltips: {
          mode: "index",
          intersect: false,
        },
        hover: {
          mode: "nearest",
          intersect: true,
        },
        scales: {
          xAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "Fecha",
              },
            },
          ],
          yAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "HUMEDAD RELATIVA DEL AIRE (%) ",
              },
            },
          ],
        },
      },
      temperatura_options: {
        responsive: true,
        title: {
          display: false,
          text: "TEMPERATURA DEL AIRE",
        },
        tooltips: {
          mode: "index",
          intersect: false,
        },
        hover: {
          mode: "nearest",
          intersect: true,
        },
        scales: {
          xAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "Fecha",
              },
            },
          ],
          yAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "TEMPERATURA DEL AIRE (°C)",
              },
            },
          ],
        },
      },
      precipitacion_options: {
        responsive: true,
        title: {
          display: false,
          text: "PRECIPITACION(mm)",
        },
        tooltips: {
          mode: "index",
          intersect: false,
        },
        hover: {
          mode: "nearest",
          intersect: true,
        },
        scales: {
          xAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "Fecha",
              },
            },
          ],
          yAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "Precipitación (mm)",
              },
            },
          ],
        },
      },
      nive_agua_options: {
        responsive: true,
        title: {
          display: false,
          text: "NIVEL DEL AGUA (mm)",
        },
        tooltips: {
          mode: "index",
          intersect: false,
        },
        hover: {
          mode: "nearest",
          intersect: true,
        },
        scales: {
          xAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "Fecha",
              },
            },
          ],
          yAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "NIVEL DEL AGUA INST",
              },
            },
          ],
        },
      },
      // delete
    };
  },
};
</script>
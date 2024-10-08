---
Title: Charts

menus:
  main:
    weight: 1
    title: Project
layout: splash
classes: wide
---

#### Playground

<style>
      #selectDataSet{
        color: #252A34;
      }

      #chartContainer {
        border-radius: 5px;
        position: relative;
      }
      #legendDivContainer {
        color: #929294;
        margin: 10px;
        position: absolute;
        top: 20px;
        left: 20px;
        background-color: #343a46;
        padding: 10px;
        box-shadow: 1px 1px 15px 1px rgba(0, 0, 0, 0.3);
        border-radius: 5px;
        opacity: 0.7;
        font-size: 0.7rem;
      }
      .legendDiv {
        display: flex;
        width: 300px;
        justify-content: space-around;

      }
      .freqLegend {
        width: 10px;
        border-radius: 3px;
        height: 10px;
        background: tomato;
      }

</style>

<div>
      <select name="selectDataSet" id="selectDataSet">
        <option value="dataset00">Dataset 00</option>
        <option value="dataset01">Dataset 01</option>
        <option value="dataset02">Dataset 02</option>
        <option value="dataset03">Dataset 03</option>
      </select>
</div>
<div id="chartContainer">
  <div id="legendDivContainer">
    <div class="legendDiv">
      <div id="maxFreqLegend" class="freqLegend"></div>
      <div>Max</div>
      <div id="maxFreq"></div>
  </div>
  <div class="legendDiv">
    <div id="minFreqLegend" class="freqLegend"></div>
    <div>Min</div>
      <div id="minFreq"></div>
    </div>
  </div>
  <canvas id="myChart"></canvas>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js@2.9.3"></script>
<script src="https://cdn.jsdelivr.net/npm/hammerjs@2.0.8"></script>
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-zoom@0.7.7"></script>

<script>
      let chart = undefined;
      window.addEventListener("load", async function (e) {
        // for initial plot
        let timeJsonURL = timePath_3;
        let frequencyJsonURL = frequencyPath_3;
        makePlot(timeJsonURL, frequencyJsonURL);

        // ------------------
        var selectDataSet = document.getElementById("selectDataSet");
        selectDataSet.addEventListener("change", handleDataSetChange);

        async function handleDataSetChange(e) {


          if (e.currentTarget.value === "dataset00") {
            let timeJsonURL = timePath_0;
            let frequencyJsonURL = frequencyPath_0;
            // makePlot(timeJsonURL, frequencyJsonURL);

            updateChart(timeJsonURL, frequencyJsonURL);

          } else if (e.currentTarget.value === "dataset01") {
            let timeJsonURL = timePath_1;
            let frequencyJsonURL = frequencyPath_1;
            // makePlot(timeJsonURL, frequencyJsonURL);

            updateChart(timeJsonURL, frequencyJsonURL);


          } else if (e.currentTarget.value === "dataset02") {
            let timeJsonURL = timePath_2;
            let frequencyJsonURL = frequencyPath_2;
            // makePlot(timeJsonURL, frequencyJsonURL);

            updateChart(timeJsonURL, frequencyJsonURL);


          } else if (e.currentTarget.value === "dataset03") {
            let timeJsonURL = timePath_3;
            let frequencyJsonURL = frequencyPath_3;
            // makePlot(timeJsonURL, frequencyJsonURL);

            updateChart(timeJsonURL, frequencyJsonURL);


          }

          // const { xData, yData } = await getXYData(xDataSourcePath, yDataSourcePath);
        }
      });

      async function updateChart(timeJsonURL, frequencyJsonURL){
        const {xData, yData}  = await getXYData(timeJsonURL, frequencyJsonURL);
            chart.data.xLabels = xData;
            chart.data.datasets[0].data = yData;
            updateMaxMin(yData)
            chart.update()
      }

      let timePath_0 =
        "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/time_0.json";
      let timePath_1 =
        "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/time_1.json";
      let timePath_2 =
        "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/time_2.json";
      let timePath_3 =
        "https://raw.githubusercontent.com/galibhassan/sample-json/master/sampleDataForPowerGridFrequencyWebsitePlayground/time.json";

      let frequencyPath_0 =
        "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/frequency_0.json";
      let frequencyPath_1 =
        "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/frequency_1.json";
      let frequencyPath_2 =
        "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/frequency_2.json";
      let frequencyPath_3 =
        "https://raw.githubusercontent.com/galibhassan/sample-json/master/sampleDataForPowerGridFrequencyWebsitePlayground/frequency.json";

      const MAX_COLOR = "tomato";
      const MIN_COLOR = "#042e82";
      const DEFAULT_COLOR = "#00ADB5";

      document.getElementById("maxFreqLegend").style.backgroundColor = MAX_COLOR;
      document.getElementById("minFreqLegend").style.backgroundColor = MIN_COLOR;

      async function getXYData(xDataSourcePath, yDataSourcePath) {
        const xData = await fetch(xDataSourcePath).then((response) => response.json());
        const yData = await fetch(yDataSourcePath).then((response) => response.json());

        return new Promise((resolve, reject) => {
          resolve({ xData, yData });
        });
      }

      function updateMaxMin(yData){

        const maxFrequency = yData.reduce(function (a, b) {
          return Math.max(a, b);
        });

        const minFrequency = yData.reduce(function (a, b) {
          return Math.min(a, b);
        });

        document.getElementById("maxFreq").innerHTML = maxFrequency;
        document.getElementById("minFreq").innerHTML = minFrequency;

        return {maxFrequency, minFrequency}

      }

      async function makePlot(xDataSourcePath, yDataSourcePath) {
        const { xData, yData } = await getXYData(xDataSourcePath, yDataSourcePath);
        const {maxFrequency, minFrequency} = updateMaxMin(yData)

        function customRadius(context) {
          let index = context.dataIndex;
          let value = context.dataset.data[index];
          if (value === maxFrequency) {
            console.log(value)
            return 8;
          } else if (value === minFrequency) {
            return 8;
          } else {
            return 0;
          }
        }

        function customBackgroundColor(context) {
          let index = context.dataIndex;
          let value = context.dataset.data[index];
          if (value === maxFrequency) {
            return MAX_COLOR;
          } else if (value === minFrequency) {
            return MIN_COLOR;
          } else {
            return DEFAULT_COLOR;
          }
        }

        var ctx = document.getElementById("myChart").getContext("2d");

        chart = new Chart(ctx, {
          type: "line",
          data: {
            xLabels: xData,
            datasets: [
              {
                label: "Frequency deviation [mHz]",
                data: yData,
                fill: false,
                borderColor: "#00ADB5",
                borderWidth: 2,
                backgroundColor: "#00ADB5",
                steppedLine: false,
                pointStyle: "circ",
                lineTension: 0,
              },
            ],
          },
          options: {
            legend: {
              display: false,
            },
            elements: {
              point: {
                radius: 0,
                display: true,
              },
            },
            scales: {
              yAxes: [
                {
                  scaleLabel: {
                    display: true,
                    labelString: "Frequency deviation [mHz]",
                    fontSize: 20,
                  },
                },
                {
                  ticks: {
                    display: false,
                    beginAtZero: true,
                    fontColor: "#00ADB5",
                  },
                },
              ],
              xAxes: [
                {
                  scaleLabel: {
                    display: true,
                    labelString: "Time",
                    fontSize: 20,
                  },
                  ticks: {
                    display: true,
                    autoSkip: true,
                    maxTicksLimit: 10,
                  },
                },
              ],
            },
            plugins: {
              zoom: {
                pan: {
                  enabled: true,
                  mode: "xy",
                },
                zoom: {
                  enabled: true,
                  mode: "xy",
                  sensitivity: 0,
                },
              },
            },
          },
        });
      }<!DOCTYPE html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Embedded Power BI Report</title>
    <style>
        #selectDataSet {
            color: #252A34;
        }
        #chartContainer {
            border-radius: 5px;
            position: relative;
        }
        #legendDivContainer {
            color: #929294;
            margin: 10px;
            position: absolute;
            top: 20px;
            left: 20px;
            background-color: #343a46;
            padding: 10px;
            box-shadow: 1px 1px 15px 1px rgba(0, 0, 0, 0.3);
            border-radius: 5px;
            opacity: 0.7;
            font-size: 0.7rem;
        }
        .legendDiv {
            display: flex;
            width: 300px;
            justify-content: space-around;
        }
        .freqLegend {
            width: 10px;
            border-radius: 3px;
            height: 10px;
            background: tomato;
        }
        #powerBiReportContainer {
            height: 600px;
            width: 100%;
            margin-top: 20px;
        }
    </style>
</head>
<body>

    <div id="powerBiReportContainer"></div>

    <select id="selectDataSet">
        <option value="dataset00">Dataset 00</option>
        <option value="dataset01">Dataset 01</option>
        <option value="dataset02">Dataset 02</option>
        <option value="dataset03">Dataset 03</option>
    </select>

    <div id="chartContainer">
        <canvas id="myChart"></canvas>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js@2.9.3"></script>
    <script src="https://cdn.jsdelivr.net/npm/hammerjs@2.0.8"></script>
    <script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-zoom@0.7.7"></script>
    <script src="https://cdn.jsdelivr.net/npm/powerbi-client@2.9.0/dist/powerbi.min.js"></script>

    <script>
        let chart = undefined;

        window.addEventListener("load", async function (e) {
            // Initial plot
            let timeJsonURL = timePath_3;
            let frequencyJsonURL = frequencyPath_3;
            makePlot(timeJsonURL, frequencyJsonURL);

            var selectDataSet = document.getElementById("selectDataSet");
            selectDataSet.addEventListener("change", handleDataSetChange);

            async function handleDataSetChange(e) {
                if (e.currentTarget.value === "dataset00") {
                    let timeJsonURL = timePath_0;
                    let frequencyJsonURL = frequencyPath_0;
                    updateChart(timeJsonURL, frequencyJsonURL);
                } else if (e.currentTarget.value === "dataset01") {
                    let timeJsonURL = timePath_1;
                    let frequencyJsonURL = frequencyPath_1;
                    updateChart(timeJsonURL, frequencyJsonURL);
                } else if (e.currentTarget.value === "dataset02") {
                    let timeJsonURL = timePath_2;
                    let frequencyJsonURL = frequencyPath_2;
                    updateChart(timeJsonURL, frequencyJsonURL);
                } else if (e.currentTarget.value === "dataset03") {
                    let timeJsonURL = timePath_3;
                    let frequencyJsonURL = frequencyPath_3;
                    updateChart(timeJsonURL, frequencyJsonURL);
                }
            }

            async function updateChart(timeJsonURL, frequencyJsonURL) {
                const { xData, yData } = await getXYData(timeJsonURL, frequencyJsonURL);
                chart.data.xLabels = xData;
                chart.data.datasets[0].data = yData;
                updateMaxMin(yData);
                chart.update();
            }

            let timePath_0 = "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/time_0.json";
            let timePath_1 = "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/time_1.json";
            let timePath_2 = "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/time_2.json";
            let timePath_3 = "https://raw.githubusercontent.com/galibhassan/sample-json/master/sampleDataForPowerGridFrequencyWebsitePlayground/time.json";
            let frequencyPath_0 = "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/frequency_0.json";
            let frequencyPath_1 = "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/frequency_1.json";
            let frequencyPath_2 = "https://raw.githubusercontent.com/galibhassan/power-grid-frequency-data-automation/master/output/frequency_2.json";
            let frequencyPath_3 = "https://raw.githubusercontent.com/galibhassan/sample-json/master/sampleDataForPowerGridFrequencyWebsitePlayground/frequency.json";

            async function getXYData(xDataSourcePath, yDataSourcePath) {
                const xData = await fetch(xDataSourcePath).then(response => response.json());
                const yData = await fetch(yDataSourcePath).then(response => response.json());
                return { xData, yData };
            }

            function updateMaxMin(yData) {
                const maxFrequency = Math.max(...yData);
                const minFrequency = Math.min(...yData);
                document.getElementById("maxFreq").innerHTML = maxFrequency;
                document.getElementById("minFreq").innerHTML = minFrequency;
            }

            async function makePlot(xDataSourcePath, yDataSourcePath) {
                const { xData, yData } = await getXYData(xDataSourcePath, yDataSourcePath);
                const maxFrequency = Math.max(...yData);
                const minFrequency = Math.min(...yData);

                var ctx = document.getElementById("myChart").getContext("2d");
                chart = new Chart(ctx, {
                    type: "line",
                    data: {
                        xLabels: xData,
                        datasets: [{
                            label: "Frequency deviation [mHz]",
                            data: yData,
                            fill: false,
                            borderColor: "#00ADB5",
                            borderWidth: 2,
                            backgroundColor: "#00ADB5",
                            steppedLine: false,
                            pointStyle: "circ",
                            lineTension: 0
                        }]
                    },
                    options: {
                        legend: { display: false },
                        elements: { point: { radius: 0, display: true } },
                        scales: {
                            yAxes: [{ scaleLabel: { display: true, labelString: "Frequency deviation [mHz]", fontSize: 20 } }],
                            xAxes: [{ scaleLabel: { display: true, labelString: "Time", fontSize: 20 }, ticks: { display: true, autoSkip: true, maxTicksLimit: 10 } }]
                        },
                        plugins: {
                            zoom: {
                                pan: { enabled: true, mode: "xy" },
                                zoom: { enabled: true, mode: "xy", sensitivity: 0 }
                            }
                        }
                    }
                });
            }

            // Embed Power BI report
            var embedUrl = 'https://app.powerbi.com/reportEmbed?reportId=YOUR_REPORT_ID&groupId=YOUR_GROUP_ID';
            var embedToken = 'YOUR_EMBED_TOKEN';

            var config = {
                type: 'report',
                id: '9c37f0ab-48da-4ee1-8546-f330b830b3ec',
                embedUrl: embedUrl,
                accessToken: embedToken,
                tokenType: models.TokenType.Embed,
                permissions: models.Permissions.All,
                settings: {
                    filterPaneEnabled: false,
                    navContentPaneEnabled: false
                }
            };

            var reportContainer = document.getElementById('powerBiReportContainer');
            var report = powerbi.embed(reportContainer, config);
        });
    </script>

</body>
</html>

</script>


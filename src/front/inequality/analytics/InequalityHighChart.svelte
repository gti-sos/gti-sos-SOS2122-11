<script>
    import{Nav, NavItem, NavLink } from "sveltestrap";
    const BASE_API_PATH = "/api/v2";
    let inequalityData=[];
    let inequalityChartCountryYear = [];
    let inequalityChartCoefficients = [];
    let inequalityCharteducations = [];
    let inequalityChartlifes = [];
 
		let errorMsg="Tiene que cargar los datos para visualizar las analíticas.";
    let cargados = false;
    async function loadChart() {
        console.log("Fetching data...");
        const res = await fetch(BASE_API_PATH + "/inequality-stats");
        inequalityData = await res.json();
        if (res.ok) {
          inequalityData.forEach(stat => {
            inequalityChartCountryYear.push(stat.country+"-"+stat.year);
            inequalityChartCoefficients.push(parseFloat(stat.coefficients));
            inequalityCharteducations.push(parseFloat(stat.educations));
            inequalityChartlifes.push(parseFloat(stat.lifes));
            });
            cargados=true;
        }
        
    console.log("inequality Chart data: " + inequalityData);
            
    Highcharts.chart('container', {
      chart: {
          type: 'column'
      },
      title: {
          text: 'Porcentajes de de desigualdad Recogidas por la UNDP'
      },
      xAxis: {
          categories: inequalityChartCountryYear,
          crosshair: true
      },
      yAxis: {
          min: 0,
          title: {
              text: 'Rainfall (mm)'
          }
      },
      tooltip: {
          headerFormat: '<span style="font-size:10px">{point.key}</span><table>',
          pointFormat: '<tr><td style="color:{series.color};padding:0">{series.name}: </td>' +
              '<td style="padding:0"><b>{point.y:.1f} mm</b></td></tr>',
          footerFormat: '</table>',
          shared: true,
          useHTML: true
      },
      plotOptions: {
          column: {
              pointPadding: 0.2,
              borderWidth: 0
          }
      },
      series: [{
          name: 'Coefficients',
          data: inequalityChartCoefficients
      }, {
          name: 'Educations',
          data: inequalityCharteducations
      }, {
          name: 'Lifes',
          data: inequalityChartlifes
      }]
  });
    }
  </script>
  
  <svelte:head>
  <script src="https://code.highcharts.com/highcharts.js"></script>
  <script src="https://code.highcharts.com/modules/series-label.js"></script>
  <script src="https://code.highcharts.com/modules/exporting.js"></script>
  <script src="https://code.highcharts.com/modules/export-data.js"></script>
  <script
    src="https://code.highcharts.com/modules/accessibility.js"
    on:load={loadChart}></script>
  </svelte:head>
  
  <main>
    <Nav>
        <NavItem>
          <NavLink href="#/info">Página Principal</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="#/integrations">Integrations</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="#/inequality-stats">Datos</NavLink>
        </NavItem>
    </Nav>
  
    <div>
        <h2>
          Análiticas
        </h2>
      </div>
  
    <div>
        <figure class="highcharts-figure">
          <div id="container" />
          <p class="highcharts-description">
            Gráfico de columnas que muestran los porcentajes de desigualdad.
          </p>
        </figure>
    </div>
    
  
    <div>
      {#if !cargados}
        <p class="error">{errorMsg}</p>
      {/if}
    </div>
  </main>
  
  <style>
    main {
        text-align: center;
        padding: 30px;       
    }
    p.error{
      color: red; 
      text-align:center;
      font-size: 20px;
      margin-top:80px;
    }
    
   
  </style>
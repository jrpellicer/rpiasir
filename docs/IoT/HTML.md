---
layout: page
title: Programación de la página web
nav_order: 11
---
# Programación de la página web
 
Sólo falta crear la página web. Para ello, en la carpeta donde se alojará la web y que creamos al crear el contenedor de apache, vamos a crear un fichero llamado index.php con el cófigo HTML y PHP de nuestra página web.

    $ cd ~/estacion/htdocs
    $ nano index.php

Y copiamos el siguiente código:

```php
<?php
    /* Database connection settings */
    $host = '192.168.1.27';
    $user = 'root';
    $pass = 'P@ssw0rd';
    $db = 'meteo';
    $mysqli = new mysqli($host,$user,$pass,$db) or die($mysqli->error);

    $data1 = '';
    $data2 = '';
    $data3 = '';

    $temp_min=9999;
    $temp_max=0;

    $ult_temp='';
    $ult_hum='';
    $ult_fch='';
    
    //Query para obtener datos las últimas 300 lecturas
    $sql = "SELECT * FROM `mediciones` ORDER BY fecha DESC LIMIT 300 ";
    $result = mysqli_query($mysqli, $sql);

    //Bucle para procesar los datos del Query
    while ($row = mysqli_fetch_array($result)) {
        $data1 = '"'. $row['temperatura'].'",'. $data1;
        $data2 = '"'. $row['humedad'].'",'. $data2;
        $data3 = '"'. $row['fecha'].'",'. $data3;

        //Almacenamos temperatura máxima del gráfico
        if ($temp_max < $row['temperatura']) {
            $temp_max = $row['temperatura'];
        }

        //Almacenamos temperatura mínima del gráfico
        if ($temp_min > $row['temperatura']) {
            $temp_min = $row['temperatura'];
        }

        //Almacenamos última lectura para mostrar en la cabecera
        if ($ult_temp == '') {
            $ult_temp = $row['temperatura'];
            $ult_hum = $row['humedad'];
            $ult_fch = $row['fecha'];
        }
    }

    $data1 = trim($data1,",");
    $data2 = trim($data2,",");
    $data3 = trim($data3,",");
?>
```
```html
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.2/Chart.bundle.min.js"></script>
    <title>El tiempo en Sagunto</title>
    <style>
        h2 {text-align: center;}
        p {text-align: center;}
    </style>
</head>

<body>       
    <h2>Temperatura: <?php echo $ult_temp; ?>ºC</h2>
    <h2>Humedad: <?php echo $ult_hum; ?>%</h2>
    <p><?php echo $ult_fch; ?></p>
    
    <canvas id="chart" style="width: 80%; height: 65vh; margin-top: 10px;"></canvas>
    <script>
        var ctx = document.getElementById("chart").getContext('2d');
            var myChart = new Chart(ctx, {
                type: 'line',
        data: {
            labels: [<?php echo $data3; ?>],
            datasets:
            [{
                label: 'Temperatura',
                data: [<?php echo $data1; ?>],
                borderColor:'rgba(112,122,255)',
                borderWidth: 1,
                backgroundColor:'rgba(189,193,255)',
                pointRadius:1,
            }]
        },
        
        options: {
            scales:{
                yAxes: [{beginAtZero: true, ticks: {min: <?php echo round($temp_min - 3); ?>, max: <?php echo round($temp_max + 3); ?>}}],
                xAxes: [{type: "time", time: {unit: 'minute', displayFormats: {day: 'h:mm'}}, ticks: {autoSkip: true, maxTicksLimit: 5}}],
                },
            tooltips:{mode: 'index'},
            legend:{display: false, position: 'top'},
            title: {display: true, text: 'Temperatura'}
        }
        });
    </script>

    <canvas id="chart2" style="width: 80%; height: 65vh; margin-top: 10px;"></canvas>
    <script>
        var ctx2 = document.getElementById("chart2").getContext('2d');
            var myChart2 = new Chart(ctx2, {
                type: 'line',
        data: {
            labels: [<?php echo $data3; ?>],
            datasets:
             [{
                label: 'Humedad',
                data: [<?php echo $data2; ?>],
                borderColor:'rgba(112,122,255)',
                borderWidth: 1,
                backgroundColor:'rgba(189,193,255)',
                pointRadius:1,
            }]
        },
        
        options: {
            scales:{
                yAxes: [{beginAtZero: true, ticks: {min: 0, max: 100}}],
                xAxes: [{type: "time", time: {unit: 'minute', displayFormats: {day: 'h:mm'}}, ticks: {autoSkip: true, maxTicksLimit: 5}}],
                },
            tooltips:{mode: 'index'},
            legend:{display: false, position: 'top'},
            title: {display: true, text: 'Humedad'}
            }
        });
    </script>
</body>
</html>
```
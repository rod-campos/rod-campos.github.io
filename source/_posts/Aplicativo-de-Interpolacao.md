---
title: Aplicativo de Interpolação
date: 2024-06-21 17:25:28
tags:
---

Nesta aula/atividade, utilizamos para desenvolver um app que calcula interpolação usando a formula de newton.
![Interpolação de Newton](images/iterpolacao-newton.png)

Nesse front, você escolhe o número de pontos, coloca o ponto a ser interpolado, e também os pontos conhecidos. Ao clicar interpolar, o javascript pega os pontos dos inputs, trata e aplica as funçoes de diferenças divididas e usa elas para obter o resultado da interpolação.

```bash
<!DOCTYPE html>
<html>
<head>
    <title>Interpolação de Newton</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f2f2f2;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .container {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            max-width: 500px;
            width: 100%;
        }
        h1 {
            text-align: center;
            color: #333;
        }
        form {
            display: flex;
            flex-direction: column;
        }
        label {
            margin-bottom: 5px;
            color: #555;
        }
        input[type="number"], input[type="text"] {
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 16px;
            width: calc(100% - 22px);
        }
        button {
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            font-size: 16px;
            cursor: pointer;
        }
        button[type="button"] {
            background-color: #2196F3;
            margin-bottom: 10px;
        }
        button:hover {
            opacity: 0.9;
        }
        h2 {
            text-align: center;
            color: #333;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Interpolação de Newton</h1>
        <form method="POST" action="">
            <label for="num_values">Numeros de valores:</label>
            <input type="number" id="num_values" name="num_values" required>
            <label for="x_value">Valores de X:</label>
            <input type="text" id="x_value" name="x_value" required>
            <div id="values_input"></div>
            <button type="button" onclick="addInput()">Adicionar Valores</button>
            <button type="submit">Interpolar</button>
        </form>

        <?php
        if ($_SERVER['REQUEST_METHOD'] == 'POST') {
            $numValues = (int) $_POST['num_values'];
            $xValue = (float) $_POST['x_value'];
            $values = [];

            for ($i = 1; $i <= $numValues; $i++) {
                if (!empty($_POST["xy_values_$i"])) {
                    $values[] = $_POST["xy_values_$i"];
                }
            }

            $tuplesArray = [];
            foreach ($values as $value) {
                list($x, $y) = array_map('floatval', explode(',', $value));
                $tuplesArray[] = [$x, $y];
            }

            $diferences = dividedDifferences($tuplesArray);
            $result = newtonInterpolation($xValue, $tuplesArray, $diferences);

            echo "<h2>Result: $result</h2>";
        }

        function dividedDifferences($points) {
            $n = count($points);
            $diffTable = array_fill(0, $n, array_fill(0, $n, 0));

            for ($i = 0; $i < $n; $i++) {
                $diffTable[$i][0] = $points[$i][1];
            }

            for ($j = 1; $j < $n; $j++) {
                for ($i = 0; $i < $n - $j; $i++) {
                    $diffTable[$i][$j] = ($diffTable[$i + 1][$j - 1] - $diffTable[$i][$j - 1]) / ($points[$i + $j][0] - $points[$i][0]);
                }
            }

            $coefficients = [];
            for ($i = 0; $i < $n; $i++) {
                $coefficients[] = $diffTable[0][$i];
            }

            return $coefficients;
        }

        function newtonInterpolation($x, $points, $coefficients) {
            $n = count($points);
            $result = $coefficients[0];

            for ($i = 1; $i < $n; $i++) {
                $term = $coefficients[$i];
                for ($j = 0; $j < $i; $j++) {
                    $term *= ($x - $points[$j][0]);
                }
                $result += $term;
            }

            return $result;
        }
        ?>

        <script>
            function addInput() {
                const numValues = document.getElementById('num_values').value;
                const valuesInput = document.getElementById('values_input');
                valuesInput.innerHTML = '';
                for (let i = 1; i <= numValues; i++) {
                    valuesInput.innerHTML += `<label for="xy_values_${i}">Value ${i} (x,y):</label>
                                              <input type="text" id="xy_values_${i}" name="xy_values_${i}" required><br>`;
                }
            }
        </script>
    </div>
</body>
</html>
```

![Interpolação de Newton](images/atividade-1.png)

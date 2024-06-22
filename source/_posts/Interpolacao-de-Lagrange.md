---
title: Interpolação de Lagrange
date: 2024-06-21 18:10:27
tags:
---

Codigo e front da inteporlação de Lagrange
![Atividade da Folha](images/interpolacao-lagrange.png)

```bash
<!DOCTYPE html>
<html>
<head>
    <title>Interpolação de Lagrange</title>
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
        <h1>Interpolação de Lagrange</h1>
        <form method="POST" action="">
            <label for="num_values">Number of values:</label>
            <input type="number" id="num_values" name="num_values" required>
            <label for="x_value">X Value:</label>
            <input type="text" id="x_value" name="x_value" required>
            <div id="values_input"></div>
            <button type="button" onclick="addInput()">Add Values</button>
            <button type="submit">Interpolate</button>
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

            $result = lagrangeInterpolation($xValue, $tuplesArray);

            echo "<h2>Result: $result</h2>";
        }

        function lagrangeInterpolation($x, $points) {
            $n = count($points);
            $result = 0;

            for ($i = 0; $i < $n; $i++) {
                $term = $points[$i][1];
                for ($j = 0; $j < $n; $j++) {
                    if ($j != $i) {
                        $term *= ($x - $points[$j][0]) / ($points[$i][0] - $points[$j][0]);
                    }
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

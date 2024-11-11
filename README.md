<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>마진 계산기</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
            background-color: #f4f4f9;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        input[type="number"], input[type="text"] {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            padding: 10px 20px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        .result {
            margin-top: 20px;
            font-size: 18px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>마진 계산기</h2>
        
        <!-- 판매가 입력란 -->
        <label for="salePrice">판매 가격 (원):</label>
        <input type="number" id="salePrice" placeholder="판매 가격 입력">
        
        <!-- 중국 원가 입력란 -->
        <label for="chinaCost">중국 원가 (원):</label>
        <input type="number" id="chinaCost" placeholder="중국 원가 입력">
        
        <!-- 그로스 수수료 입력란 -->
        <label for="grossFee">그로스 수수료 (%):</label>
        <input type="number" id="grossFee" placeholder="그로스 수수료 입력">
        
        <button onclick="calculateMargin()">마진 계산</button>
        
        <div class="result" id="result"></div>
    </div>

    <script>
        function calculateMargin() {
            // 입력값 받기
            const salePrice = parseFloat(document.getElementById('salePrice').value);
            const chinaCost = parseFloat(document.getElementById('chinaCost').value);
            const grossFee = parseFloat(document.getElementById('grossFee').value);

            // 유효성 검사
            if (isNaN(salePrice) || isNaN(chinaCost) || isNaN(grossFee)) {
                document.getElementById('result').innerHTML = "모든 값을 올바르게 입력해주세요!";
                return;
            }

            // 수입원가 계산 (중국 원가 * 300)
            const importCost = chinaCost * 300;

            // 판매 수수료 계산 (판매가 * 12%)
            const saleFee = salePrice * 0.12;

            // 부가세 계산 (판매가 / 11 - 수입원가 / 11)
            const vat = (salePrice / 11) - (importCost / 11);

            // 순수익 계산 (판매가 - 수입원가 - 판매수수료 - 그로스수수료 - 부가세)
            const netProfit = salePrice - importCost - saleFee - (salePrice * (grossFee / 100)) - vat;

            // 마진율 계산 (순수익 / 판매가)
            const marginRate = (netProfit / salePrice) * 100;

            // 엔드로아스 계산 (판매가 / 순수익)
            const endros = salePrice / netProfit;

            // 결과 출력
            document.getElementById('result').innerHTML = `
                판매가: ${salePrice.toLocaleString()} 원<br>
                중국 원가: ${chinaCost.toLocaleString()} 원<br>
                그로스 수수료: ${grossFee}%<br>
                <br>
                수입 원가: ${importCost.toLocaleString()} 원<br>
                판매 수수료: ${saleFee.toLocaleString()} 원<br>
                부가세: ${vat.toLocaleString()} 원<br>
                순수익: ${netProfit.toLocaleString()} 원<br>
                마진율: ${marginRate.toFixed(2)}%<br>
                엔드로아스: ${endros.toFixed(2)}<br>
            `;
        }
    </script>
</body>
</html>

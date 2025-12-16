<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Chinese Words</title>

  <script src="https://cdn.jsdelivr.net/npm/xlsx/dist/xlsx.full.min.js"></script>

  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #f5f5f5;
      display: flex;
      justify-content: center;
    }

    .container {
      margin-top: 40px;
      width: 100%;
      max-width: 600px;   /* 모바일 기준 폭 */
      padding: 0 12px;
      text-align: center;
    }

    table {
      width: 100%;        /* 화면에 맞게 */
      border-collapse: collapse;
      background: white;
    }

    td {
      border: none;
      padding: 12px 6px;  /* 모바일 터치 고려 */
      text-align: center;
      font-size: 17px;    /* 글씨 살짝 키움 */
      word-break: break-word;
    }

    /* 📱 아주 작은 화면 */
    @media (max-width: 480px) {
      td {
        font-size: 18px;
        padding: 14px 4px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div id="table-container"></div>
  </div>

  <script>
    fetch("final.xlsx")
      .then(res => res.arrayBuffer())
      .then(data => {
        const workbook = XLSX.read(data, { type: "array" });
        const sheet = workbook.Sheets[workbook.SheetNames[0]];
        let json = XLSX.utils.sheet_to_json(sheet, { header: 1 });

        // 첫 행 제거
        json.shift();

        renderTable(json);
      });

    function renderTable(data) {
      let html = "<table>";

      data.forEach(row => {
        html += "<tr>";
        row.forEach((cell, colIndex) => {
          // 2열 제거
          if (colIndex === 1) return;
          html += `<td>${cell ?? ""}</td>`;
        });
        html += "</tr>";
      });

      html += "</table>";
      document.getElementById("table-container").innerHTML = html;
    }
  </script>
</body>
</html>

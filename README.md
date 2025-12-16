<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <title>Chinese Words</title>

  <!-- SheetJS (xlsx 읽기용) -->
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
      margin-top: 60px;
      text-align: center;
    }

    table {
      border-collapse: collapse;
      background: white;
    }

    td {
      border: 1px solid #ccc;
      padding: 10px 18px;
      text-align: center;
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

        // ❌ 첫 번째 행 제거
        json.shift();

        renderTable(json);
      });

    function renderTable(data) {
      let html = "<table>";

      data.forEach(row => {
        html += "<tr>";
        row.forEach((cell, colIndex) => {
          // ❌ 2번째 열 제외 (원하면 이 줄 지워도 됨)
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

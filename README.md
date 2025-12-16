<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>CHINESE</title>

  <!-- Google Font : Poppins -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">

  <!-- SheetJS -->
  <script src="https://cdn.jsdelivr.net/npm/xlsx/dist/xlsx.full.min.js"></script>

  <style>
    body {
      margin: 0;
      font-family: 'Poppins', sans-serif; /* ✅ 글씨체 변경 */
      background-color: #f5f5f5;
      display: flex;
      justify-content: center;
    }

    .container {
      margin-top: 32px;
      width: 100%;
      max-width: 600px;
      padding: 0 12px;
      text-align: center;
    }

    /* 제목 */
    .title {
      font-size: 28px;
      font-weight: 600;
      letter-spacing: 2px;
      margin-bottom: 24px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      background: white;
    }

    td {
      border: none;                 /* 줄 제거 */
      padding: 12px 6px;
      text-align: center;
      font-size: 17px;
      word-break: break-word;
    }

    /* 모바일에서 글씨 살짝 더 크게 */
    @media (max-width: 480px) {
      td {
        font-size: 18px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- ✅ CHINESE 대문자 제목 -->
    <div class="title">CHINESE</div>

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
          // 2번째 열 제거
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

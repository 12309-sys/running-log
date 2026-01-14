# running-log
index.html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Running Log</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <style>
    body {
      font-family: -apple-system, BlinkMacSystemFont, sans-serif;
      background: #f2f2f2;
      margin: 0;
      padding: 16px;
    }
    h1 {
      text-align: center;
    }
    .card {
      background: #fff;
      padding: 16px;
      border-radius: 14px;
      margin-bottom: 16px;
    }
    input, button {
      width: 100%;
      padding: 12px;
      margin-top: 8px;
      font-size: 16px;
      border-radius: 10px;
      border: 1px solid #ddd;
    }
    button {
      background: black;
      color: white;
      border: none;
    }
    table {
      width: 100%;
      font-size: 14px;
      border-collapse: collapse;
    }
    th, td {
      padding: 6px;
      text-align: center;
      border-bottom: 1px solid #eee;
    }
  </style>
</head>
<body>

<h1>🏃 러닝 기록</h1>

<div class="card">
  <input type="date" id="date">
  <input type="number" id="distance" placeholder="거리 (km)" step="0.01">
  <input type="number" id="time" placeholder="시간 (분)">
  <input type="text" id="memo" placeholder="메모 (선택)">
  <button onclick="addRun()">기록 추가</button>
</div>

<div class="card">
  <table>
    <thead>
      <tr>
        <th>날짜</th>
        <th>거리</th>
        <th>시간</th>
        <th>페이스</th>
      </tr>
    </thead>
    <tbody id="log"></tbody>
  </table>
</div>

<script>
  let runs = JSON.parse(localStorage.getItem("runs")) || [];
  const log = document.getElementById("log");

  function pace(t, d) {
    let m = Math.floor(t / d);
    let s = Math.round(((t / d) - m) * 60);
    return m + "'" + s.toString().padStart(2, "0") + '"';
  }

  function render() {
    log.innerHTML = "";
    runs.forEach(r => {
      log.innerHTML += `
        <tr>
          <td>${r.date}</td>
          <td>${r.distance}km</td>
          <td>${r.time}분</td>
          <td>${pace(r.time, r.distance)}</td>
        </tr>
      `;
    });
  }

  function addRun() {
    const run = {
      date: date.value,
      distance: Number(distance.value),
      time: Number(time.value),
      memo: memo.value
    };
    if (!run.date || !run.distance || !run.time) {
      alert("모두 입력해줘!");
      return;
    }
    runs.unshift(run);
    localStorage.setItem("runs", JSON.stringify(runs));
    render();
    distance.value = time.value = memo.value = "";
  }

  render();
</script>

</body>
</html>

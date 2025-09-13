<!DOCTYPE html><html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>search norlang word</title>
  <style>
    body { font-family: sans-serif; padding: 20px; }
    input, button { padding: 8px; margin: 6px 0; font-size: 14px }
    .result { margin-top: 15px; }
    pre { background:#f7f7f7; padding:10px; border-radius:6px }
  </style>
</head>
<body>
  <h1>search norlang word</h1>
  <input type="text" id="searchInput" placeholder="input">
  <button onclick="searchWord()">search</button>  <div class="result" id="result"></div>  <script>
    
    let dic=``;
    let lines=``;
    
    const url = "https://raw.githubusercontent.com/Fhres126/nl/main/nl.txt";

        fetch(url)
            .then(response => {
                if (!response.ok) throw new Error("we met error we cant find txt file.");
                return response.text();
            })
            .then(text => {
                dic=text;
                lines = dic.split(/\r?\n/);
            })
            .catch(err => {
                dic=err;
                lines = dic.split(/\r?\n/);
            });
    function searchWord() {
      const inputRaw = document.getElementById("searchInput").value.trim();
      const resultDiv = document.getElementById("result");
      resultDiv.innerHTML = "";

      if (!inputRaw) {
        resultDiv.innerHTML = "search";
        return;
      }

      const input = inputRaw.toLowerCase();
      const results = [];

      for (let i = 0; i < lines.length; i++) {
        const line = lines[i];
        if (!line) continue;

        const parts = line.split('=');
        if (parts.length < 2) continue;

        const rhs = parts.slice(1).join('=');

        const beforeDot = rhs.split('.')[0];

        const tokens = beforeDot.split(/[^A-Za-z0-9가-힣]+/).filter(Boolean);

        let matched = false;
        for (let t of tokens) {
          if (t.toLowerCase() === input) { 
            matched = true;
            break;
          }
        }

        if (matched) {
          results.push(line.trim());
        }
      }

      if (results.length > 0) {
        const unique = [...new Set(results)];
        resultDiv.innerHTML = `<b>srarch result:</b><br>` + unique.map(r => `<pre>${r}</pre>`).join('');} else {
    resultDiv.innerHTML = "result is undefined";
  }
}

  </script>
</body>
</html>


















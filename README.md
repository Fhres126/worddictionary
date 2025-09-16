<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8">
  <title>search norlang word</title>
  <style>
    body { font-family: sans-serif; padding: 20px; color:#FFFFFF; background:#000000}
    input, button { padding: 8px; margin: 6px 0; font-size: 14px }
    .result { margin-top: 15px; }
    pre { background:#008800; padding:10px; border-radius:6px; display:inline-block; whitespace: pre; font-size: 1.5vh; }
  </style>
</head>
<body>
  <h1>search norlang word</h1>
  <input type="text" id="searchInput" placeholder="search verb instead of Interjection and adverb(eg hello->greet)"; size="68";>
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
                dic=dic.substring(dic.indexOf('//ewo'), dic.indexOf('//eewo'));
                lines = dic.split(/\r?\n/);
            })
            .catch(err => {
                dic=err;
                lines = dic.split(/\r?\n/);
            });
    function searchWord() {
      let inputRaw = document.getElementById("searchInput").value.trim();
      let resultDiv = document.getElementById("result");
      resultDiv.innerHTML = "";

      if (!inputRaw) {
        resultDiv.innerHTML = "search";
        return;
      }

      let input = inputRaw.toLowerCase();
      let results = [];

      for (let i = 0; i < lines.length; i++) {
        let line = lines[i];
        if (!line) continue;

        let parts = line.split('=');
        if (parts.length < 2) continue;

        let rhs = parts.slice(0).join('');

        let beforeDot = rhs.split('.')[0];

        let tokens = beforeDot.split(/[^A-Za-z0]+/).filter(Boolean);

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
        let unique = [...new Set(results)];
        resultDiv.innerHTML = `<b>srarch result:</b><br>` + unique.map(r => `<pre>${r}</pre>`).join('');} else {
    resultDiv.innerHTML = "result is undefined";
  }
}

  </script>
</body>
</html>


















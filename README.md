<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<title>모스부호 변환기</title>

<style>
  body { font-family: sans-serif; padding: 20px; line-height: 1.5; }
  textarea { width: 100%; height: 120px; font-size: 16px; }
  #result { margin-top: 15px; padding: 10px; border: 1px solid #333; min-height: 40px; font-size: 20px; white-space: pre-wrap;}
  button { margin-top: 10px; padding: 8px 14px; font-size: 15px; cursor: pointer; }
  table { margin-top: 20px; border-collapse: collapse; width: 100%; max-width: 400px; }
  th, td { border: 1px solid #333; padding: 6px; text-align: center; }
  th { background: #eee; }
</style>

</head>
<body>

<h2>모스부호 → 영어 변환기</h2>

<p>
점(<b>.</b>) = 짧은 신호<br>
선(<b>-</b>) = 긴 신호<br><br>
<b>글자 사이</b>는 한 칸 공백<br>
<b>단어 사이</b>는 <b>/</b> 사용
</p>

<textarea id="morseInput" placeholder="예) .... . .-.. .-.. --- / .-- --- .-. .-.. -.."></textarea>

<br>
<button onclick="convertMorse()">변환하기</button>

<div id="result"></div>

<!-- 모스부호 테이블 -->
<h3> 알파벳 기본 모스부호 표</h3>

<table>
  <tr><th>알파벳</th><th>모스부호</th></tr>
  <tr><td>A</td><td>.-</td></tr>
  <tr><td>B</td><td>-...</td></tr>
  <tr><td>C</td><td>-.-.</td></tr>
  <tr><td>D</td><td>-..</td></tr>
  <tr><td>E</td><td>.</td></tr>
  <tr><td>F</td><td>..-.</td></tr>
  <tr><td>G</td><td>--.</td></tr>
  <tr><td>H</td><td>....</td></tr>
  <tr><td>I</td><td>..</td></tr>
  <tr><td>J</td><td>.---</td></tr>
  <tr><td>K</td><td>-.-</td></tr>
  <tr><td>L</td><td>.-..</td></tr>
  <tr><td>M</td><td>--</td></tr>
  <tr><td>N</td><td>-.</td></tr>
  <tr><td>O</td><td>---</td></tr>
  <tr><td>P</td><td>.--.</td></tr>
  <tr><td>Q</td><td>--.-</td></tr>
  <tr><td>R</td><td>.-.</td></tr>
  <tr><td>S</td><td>...</td></tr>
  <tr><td>T</td><td>-</td></tr>
  <tr><td>U</td><td>..-</td></tr>
  <tr><td>V</td><td>...-</td></tr>
  <tr><td>W</td><td>.--</td></tr>
  <tr><td>X</td><td>-..-</td></tr>
  <tr><td>Y</td><td>-.--</td></tr>
  <tr><td>Z</td><td>--..</td></tr>
</table>

<script>
  const morseMap = {
    ".-":"A","-...":"B","-.-.":"C","-..":"D",".":"E","..-.":"F","--.":"G","....":"H","..":"I",
    ".---":"J","-.-":"K",".-..":"L","--":"M","-.":"N","---":"O",".--.":"P","--.-":"Q",".-.":"R",
    "...":"S","-":"T","..-":"U","...-":"V",".--":"W","-..-":"X","-.--":"Y","--..":"Z",
    "-----":"0",".----":"1","..---":"2","...--":"3","....-":"4",".....":"5",
    "-....":"6","--...":"7","---..":"8","----.":"9"
  };

  function convertMorse() {
    let text = document.getElementById("morseInput").value.trim();
    let words = text.split(" / ");
    let result = words.map(word => {
      return word.split(" ").map(code => morseMap[code] || "?").join("");
    }).join(" ");
    document.getElementById("result").textContent = result;
  }
</script>

</body>
</html>

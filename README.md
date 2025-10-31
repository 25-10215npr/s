<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<title>모스부호 변환기</title>

<!-- 최소 스타일 -->
<style>
  body { font-family: sans-serif; padding: 20px; }
  textarea { width: 100%; height: 120px; font-size: 16px; }
  #result { margin-top: 15px; padding: 10px; border: 1px solid #333; min-height: 40px; font-size: 20px; white-space: pre-wrap;}
  button { margin-top: 10px; padding: 8px 14px; font-size: 15px; }
</style>

</head>
<body>

<h2>모스부호 → 영어 변환기</h2>

<p>· (점) 과 - (선)을 사용하세요.<br>
글자 사이 = 공백, 단어 사이 = /</p>

<textarea id="morseInput" placeholder="예) .... . .-.. .-.. --- / .-- --- .-. .-.. -.."></textarea>

<br>
<button onclick="convertMorse()">변환하기</button>

<div id="result"></div>

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

    // 단어 구분 / → 공백
    let words = text.split(" / ");

    let result = words.map(word => {
      return word.split(" ").map(code => morseMap[code] || "?").join("");
    }).join(" ");

    document.getElementById("result").textContent = result;
  }
</script>

</body>
</html>

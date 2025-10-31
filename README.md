<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>모스부호 → 영어 변환기</title>
  <style>
    :root { --radius: 14px; }
    * { box-sizing: border-box; }
    body {
      font-family: system-ui, -apple-system, Segoe UI, Roboto, "Noto Sans", Arial, "Apple SD Gothic Neo", "Malgun Gothic", sans-serif;
      margin: 0; padding: 24px;
      background: #0f172a; color: #e5e7eb;
      display: grid; place-items: center; min-height: 100dvh;
    }
    .card {
      width: min(900px, 96vw);
      background: #111826cc;
      backdrop-filter: blur(6px);
      border: 1px solid #1f2937;
      border-radius: var(--radius);
      box-shadow: 0 10px 30px rgba(0,0,0,.35);
      overflow: hidden;
    }
    header {
      padding: 18px 22px;
      background: linear-gradient(180deg, #111827, #0b1220);
      border-bottom: 1px solid #1f2937;
    }
    h1 { margin: 0; font-size: 20px; letter-spacing: .2px; }
    main { padding: 22px; display: grid; gap: 18px; }
    label { font-weight: 600; color: #cbd5e1; display: block; margin-bottom: 8px; }
    textarea, input[type="text"] {
      width: 100%; background: #0b1220; color: #e5e7eb;
      border: 1px solid #334155; border-radius: 10px;
      padding: 12px 14px; font-size: 15px; line-height: 1.5;
      outline: none; transition: border-color .15s ease, box-shadow .15s ease;
    }
    textarea:focus, input:focus { border-color: #60a5fa; box-shadow: 0 0 0 4px #60a5fa22; }
    .hint { color: #94a3b8; font-size: 13px; margin-top: 6px; }
    .row { display: grid; gap: 16px; grid-template-columns: 1fr; }
    .btns { display: flex; gap: 10px; flex-wrap: wrap; }
    button {
      background: #1f2937; color: #e5e7eb; border: 1px solid #334155;
      border-radius: 10px; padding: 10px 14px; font-size: 14px;
      cursor: pointer; transition: transform .05s ease, background .15s ease, border-color .15s ease;
    }
    button:hover { background: #243244; border-color: #3b82f6; }
    button:active { transform: translateY(1px); }
    .output {
      min-height: 68px; white-space: pre-wrap; word-break: break-word;
      background: #0b1220; border: 1px dashed #334155; border-radius: 10px;
      padding: 12px 14px; font-size: 18px; letter-spacing: .5px;
    }
    .grid2 { display: grid; gap: 16px; grid-template-columns: 1fr; }
    .kbd { font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; background:#0b1220; padding:2px 6px; border:1px solid #334155; border-radius:6px; }
    footer { padding: 16px 22px; color:#94a3b8; font-size: 12px; border-top:1px solid #1f2937; background:#0b1220; }
    @media (min-width: 820px){
      .row { grid-template-columns: 1fr 1fr; }
      .grid2 { grid-template-columns: 1fr 1fr; }
    }
    .badge { display:inline-block; font-size: 12px; padding:3px 8px; border:1px solid #334155; border-radius:999px; color:#cbd5e1; }
    .error { color:#fecaca; }
  </style>
</head>
<body>
  <div class="card" role="region" aria-label="모스부호 변환기">
    <header>
      <h1>모스부호 → 영어 변환기 <span class="badge" aria-hidden="true">dot=. dash=-</span></h1>
    </header>

    <main>
      <section class="grid2" aria-labelledby="input-label">
        <div>
          <label id="input-label" for="morse">모스부호 입력</label>
          <textarea id="morse" rows="5" placeholder=". 과 - 만 사용하세요. 글자 사이 공백, 단어 사이 / 사용
예) .... . .-.. .-.. --- / .-- --- .-. .-.. -.." aria-describedby="hint"></textarea>
          <div id="hint" class="hint">글자 구분: 공백(space) · 단어 구분: 슬래시(<span class="kbd">/</span>) · 허용문자: <span class="kbd">.</span> <span class="kbd">-</span> <span class="kbd">/</span> <span class="kbd">공백</span></div>
          <div id="error" class="hint error" role="alert" aria-live="polite"></div>
          <div class="btns" style="margin-top:10px">
            <button id="convertBtn" title="변환 (Ctrl/⌘ + Enter)"><strong>변환</strong> (Ctrl/⌘+Enter)</button>
            <button id="clearBtn" title="입력/출력 지우기">지우기</button>
            <button id="sampleBtn" title="샘플 입력 넣기">샘플 넣기</button>
            <button id="copyBtn" title="영어 결과 복사">결과 복사</button>
          </div>
        </div>
        <div>
          <label for="english">영어 결과</label>
          <div id="english" class="output" aria-live="polite"></div>
          <div class="hint">알 수 없는 조합은 <span class="kbd">?</span> 로 표시됩니다.</div>
        </div>
      </section>

      <details>
        <summary>지원 표 보기 (A–Z, 0–9, 기본 문장부호)</summary>
        <div class="row" style="margin-top:10px">
          <div class="hint">
            A .- · B -... · C -.-. · D -.. · E . · F ..-. · G --. · H .... · I .. · J .--- · K -.- · L .-.. · M -- · N -. · O --- · P .--. · Q --.- · R .-. · S ... · T - · U ..- · V ...- · W .-- · X -..- · Y -.-- · Z --..
          </div>
          <div class="hint">
            0 ----- · 1 .---- · 2 ..--- · 3 ...-- · 4 ....- · 5 ..... · 6 -.... · 7 --... · 8 ---.. · 9 ----. · . .-.-.- · , --..-- · ? ..--.. · ! -.-.-- · / -..-. · : ---... · ' .----. · " .-..-. · ( -.--. · ) -.--.- · & .-... · = -...- · + .-.-. · - -....- · _ ..--.-
          </div>
        </div>
      </details>
    </main>

    <footer>
      모스부호는 국제 표준(ITU) 배열을 따릅니다. 입력에 탭·한글·영문자 등 비허용 문자가 포함되면 무시됩니다.
    </footer>
  </div>

  <script>
    // ITU Morse map: Morse (.-) -> English
    const MORSE_TO_EN = {
      // Letters
      ".-":"A","-...":"B","-.-.":"C","-..":"D",".":"E","..-.":"F","--.":"G","....":"H","..":"I",
      ".---":"J","-.-":"K",".-..":"L","--":"M","-.":"N","---":"O",".--.":"P","--.-":"Q",".-.":"R",
      "...":"S","-":"T","..-":"U","...-":"V",".--":"W","-..-":"X","-.--":"Y","--..":"Z",
      // Digits
      "-----":"0",".----":"1","..---":"2","...--":"3","....-":"4",".....":"5","-....":"6","--...":"7","---..":"8","----.":"9",
      // Punctuation (common)
      ".-.-.-":".","--..--":",","..--..":"?","-.-.--":"!","-..-.":"/","---...":":",".----.":"'"," .-..-. ".trim():'"',
      "-.--.":"(","-.--.-":")",".-...":"&","-...-":"=",".-.-.":"+","-....-":"-","..--.-":"_"
    };

    const inputEl = document.getElementById('morse');
    const outEl   = document.getElementById('english');
    const errEl   = document.getElementById('error');
    const btnConvert = document.getElementById('convertBtn');
    const btnClear   = document.getElementById('clearBtn');
    const btnCopy    = document.getElementById('copyBtn');
    const btnSample  = document.getElementById('sampleBtn');

    function sanitize(raw){
      // 허용: . - / 공백, 줄바꿈. 그 외는 제거
      return raw.replace(/[^\.\-\/\s]/g, "");
    }

    function translateMorseToEnglish(raw){
      errEl.textContent = "";
      const clean = sanitize(raw).trim();
      if (!clean) return "";

      // 줄바꿈을 공백으로 통일
      const normalized = clean.replace(/\s+/g, " ").replace(/\s*\/\s*/g, " / ");
      const tokens = normalized.split(" ");

      let result = "";
      let unknowns = [];

      for(const token of tokens){
        if(token === "/"){
          result += " ";
          continue;
        }
        if(token === "") continue;

        const letter = MORSE_TO_EN[token];
        if(letter){
          result += letter;
        } else {
          result += "?";
          unknowns.push(token);
        }
      }

      if(unknowns.length){
        errEl.textContent = `알 수 없는 패턴 ${unknowns.length}개: ${[...new Set(unknowns)].slice(0,10).join(", ")}${unknowns.length>10?" …":""}`;
      }
      return result;
    }

    function convert(){
      const english = translateMorseToEnglish(inputEl.value);
      outEl.textContent = english;
    }

    btnConvert.addEventListener('click', convert);
    btnClear.addEventListener('click', () => {
      inputEl.value = ""; outEl.textContent = ""; errEl.textContent = "";
      inputEl.focus();
    });
    btnSample.addEventListener('click', () => {
      inputEl.value = ".... . .-.. .-.. --- / .-- --- .-. .-.. -..";
      convert();
    });
    btnCopy.addEventListener('click', async () => {
      const txt = outEl.textContent || "";
      if(!txt){ errEl.textContent = "복사할 결과가 없습니다."; return; }
      try { await navigator.clipboard.writeText(txt);
            errEl.textContent = "결과를 클립보드에 복사했어요."; }
      catch { errEl.textContent = "복사에 실패했어요. 브라우저 권한을 확인해주세요."; }
    });

    // 단축키: Ctrl/⌘ + Enter 변환
    document.addEventListener('keydown', (e) => {
      if((e.ctrlKey || e.metaKey) && e.key === 'Enter'){ convert(); }
    });

    // 실시간 옵션: 입력할 때 즉시 변환하고 싶으면 아래 주석 해제
    // inputEl.addEventListener('input', convert);
  </script>
</body>
</html>







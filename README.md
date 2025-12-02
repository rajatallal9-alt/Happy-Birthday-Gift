<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Happy Birthday Laraib — Card</title>
<style>
  /* UPDATED COLOR THEME (Brighter Pink/Gold for Birthday) */
  :root{
    --accent-1: #ff69b4; /* Brighter Pink (Hot Pink) */
    --accent-2: #800080; /* Purple */
    --card-bg: #fff7ff;
    --gold: #FFD700;
  }

  html,body{
    height:100%;
    margin:0;
    font-family: "Segoe UI", Roboto, Arial, sans-serif;
    /* Changed background to light, warm gradient */
    background: linear-gradient(180deg,#fff0f5 0%, #fff 60%); 
    -webkit-font-smoothing:antialiased;
    -moz-osx-font-smoothing:grayscale;
    color:#222;
    overflow:hidden;
  }

  /* Layout for scrolling on mobile */
  .section {
    position:absolute;
    inset:0;
    display:flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    padding:20px;
    box-sizing:border-box;
    opacity:0;
    transform: translateY(24px);
    transition: opacity .45s ease, transform .45s ease;
    pointer-events:none;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
    z-index: 1;
  }
  
  /* Center Section 9 (Cake) and INTRO Section */
  #sec9.section, #intro.section {
      justify-content: center;
  }

  .section.active{
    opacity:1;
    transform: translateY(0);
    pointer-events:auto;
    z-index:5;
  }

  /* ------------------------------------------------ */
  /* NEW INTRO DOOR STYLES */
  /* ------------------------------------------------ */
  #intro {
      background: linear-gradient(180deg, #ffd9eb 0%, #ffffff 100%);
      transition: opacity 1.5s ease;
      z-index: 10;
  }
  
  .door-container {
      position: relative;
      width: 90%;
      max-width: 500px;
      height: 60vh;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: auto;
  }

  .door {
      position: absolute;
      top: 0;
      width: 50%;
      height: 100%;
      background: linear-gradient(180deg, #800080, #c458c4); /* Purple Door */
      transform-origin: center center;
      transition: transform 1.5s cubic-bezier(0.86, 0, 0.07, 1);
      box-shadow: 0 10px 30px rgba(0,0,0,0.4);
      border-radius: 8px;
  }

  .door.left {
      left: 0;
      transform: translateX(-50%) rotateY(0deg); 
  }

  .door.right {
      right: 0;
      transform: translateX(50%) rotateY(0deg); 
  }

  .door-open .door.left {
      transform: translateX(-100%) rotateY(-130deg);
  }

  .door-open .door.right {
      transform: translateX(100%) rotateY(130deg);
  }

  .greeting-text {
      position: absolute;
      font-size: 2.5rem;
      font-weight: bold;
      color: var(--accent-1);
      text-shadow: 0 0 10px rgba(255, 255, 255, 0.8);
      opacity: 0;
      transition: opacity 0.8s ease;
      text-align: center;
      z-index: 5;
  }
  .door-open .greeting-text {
      opacity: 1;
  }


  /* ------------------------------------------------ */
  /* GENERAL CARD STYLES (Envelopes) */
  /* ------------------------------------------------ */
  .card-wrap{
    width:100%;
    max-width:920px;
    display:flex;
    flex-direction:column;
    align-items:center;
    gap:18px;
    position: relative;
    margin: auto;
    padding-bottom: 40px;
  }

  .envelope {
    width: 720px;
    max-width: 94%;
    height: 420px;
    position: relative;
    perspective: 1400px;
  }

  .envelope .body {
    position:absolute;
    inset:0;
    border-radius:12px;
    background: linear-gradient(180deg, #fff, #fff7fb);
    box-shadow: 0 18px 60px rgba(20,10,60,0.16);
    overflow:visible;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:28px;
    box-sizing:border-box;
  }

  .envelope .flap {
    position:absolute;
    top:0;
    left:0;
    width:100%;
    height:58%;
    transform-origin: top center;
    background: linear-gradient(180deg,var(--accent-1), #ff99c8); /* Brighter Flap */
    border-top-left-radius:14px;
    border-top-right-radius:14px;
    box-shadow: 0 12px 40px rgba(255, 105, 180, 0.18);
    transform-style: preserve-3d;
    transition: transform .8s cubic-bezier(.2,.9,.3,1);
    backface-visibility: hidden;
    z-index:8;
    display:flex;
    align-items:flex-end;
    justify-content:center;
  }
  
  .envelope .flap::after{
    content:"";
    display:block;
    width:78%;
    height:26px;
    margin-bottom:22px;
    background: linear-gradient(90deg, rgba(75,46,131,0.08), rgba(139,46,255,0.12));
    border-radius:6px;
    opacity:.95;
  }
  
  .envelope .letter {
    position:absolute;
    left:6%;
    width:88%;
    height:72%;
    top:16%;
    background: linear-gradient(180deg,#fff,#fffefc);
    border-radius:8px;
    box-shadow: 0 8px 30px rgba(30,10,60,0.06);
    transform-origin: bottom center;
    transform: translateY(28px) scale(.98);
    opacity:0;
    transition: transform .8s cubic-bezier(.2,.9,.3,1), opacity .6s;
    padding:22px;
    box-sizing:border-box;
    z-index:6;
    display:flex;
    flex-direction:column;
    justify-content:flex-start;
    align-items:flex-start;
    overflow:auto;
    scrollbar-width: thin;
  }

  .envelope.opened .flap {
    transform: rotateX(-180deg) translateY(-6%);
  }
  .envelope.opened .letter {
    transform: translateY(-6px) scale(1);
    opacity:1;
  }
  
  .card-content{
    width:100%;
    color:var(--accent-2);
    text-align:left;
  }
  .card-content h1,
  .card-content h2 {
    margin:0 0 8px 0;
    font-size:22px;
    color:var(--accent-2);
    text-align:center;
    width:100%;
  }
  .card-content p{
    margin:0 0 12px 0;
    font-size:18px;
    line-height:1.55;
    color:#222;
    white-space: pre-wrap;
  }
  .quote{
    margin-top:8px;
    font-style:italic;
    color:#333;
    text-align:left;
  }
  
  .controls{
    display:flex;
    gap:12px;
    margin-top:8px;
    z-index: 10;
  }
  .btn{
    appearance:none;
    border:0;
    padding:12px 22px; /* Increased padding */
    border-radius:12px; /* Increased corner radius */
    background:linear-gradient(90deg,var(--accent-1), #800080);
    color:#fff;
    cursor:pointer;
    font-size:18px; /* Increased font size */
    box-shadow: 0 6px 18px rgba(255, 105, 180, 0.3);
  }
  .btn.secondary{
    background:transparent;
    color:var(--accent-2);
    box-shadow:none;
    border:2px solid rgba(75,46,131,0.2); /* Slightly thicker border */
  }
  .btn:disabled {
      opacity: 0.5;
      cursor: not-allowed;
  }
  
  /* ------------------------------------------------ */
  /* CAKE AND KNIFE STYLES (Guaranteed Centering) */
  /* ------------------------------------------------ */
  #cake-container {
      position: relative;
      max-width: 500px;
      margin: 20px auto; 
      display: flex;
      justify-content: center; 
      align-items: center;
      height: 450px; 
  }
  #cake { 
      max-width:100%; 
      width:500px; 
      transition: transform .2s ease; 
      border-radius:14px; 
      z-index: 2;
  }
  #knife { 
      width:130px; 
      position:absolute; 
      z-index: 5;
      left:-200px; 
      top:10%; 
      transform: rotate(-15deg);
      transition: all 0.5s ease-out;
      opacity: 0;
  }
  
  /* Celebration Text and Glow */
  @keyframes pulseGlow {
      from { text-shadow: 0 0 10px var(--accent-1), 0 0 20px var(--accent-1); }
      to { text-shadow: 0 0 20px var(--accent-1), 0 0 30px #fff; }
  }
  #celebrationText {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%); 
    width: 100%;
    text-align: center;
    font-size: 3.5rem; 
    font-weight: bold;
    color: var(--accent-1); 
    text-shadow: 0px 0px 20px rgba(255, 255, 255, 0.9), 0px 4px 12px rgba(139,46,255,0.3);
    opacity: 0;
    z-index: 10;
    pointer-events: none;
    transition: opacity 1.5s ease-in-out;
    background: rgba(255,255,255,0.85); 
    padding: 20px 0;
    border-radius: 20px;
    backdrop-filter: blur(4px);
    animation: pulseGlow 1.5s infinite alternate; 
  }


  @media (max-width:900px){
    .envelope { width: 92%; height: 420px; }
    .envelope .letter { height:70%; top:18%; padding:16px; }
    .card-content p{ font-size:16px; }
    #cake { width:380px; } 
    #cake-container { height: 400px; max-width: 380px; } 
    #celebrationText { font-size: 2.5rem; }
    .btn { padding: 10px 18px; font-size: 16px; }
  }

  @media (max-width:520px){
    .envelope{ height:360px; }
    .envelope .letter { height:70%; top:15%; }
    .card-content h1{ font-size:20px; }
    .card-content p{ font-size:15px; }
    #celebrationText { font-size: 2rem; }
    #cake { width: 280px; } 
    #cake-container { height: 300px; max-width: 280px; } 
    .btn { padding: 10px 18px; font-size: 16px; }
  }
</style>
</head>
<body>

<section id="intro" class="section active" aria-label="Introduction Screen">
    <div class="door-container">
        <div class="door left"></div>
        <div class="door right"></div>
        <h1 class="greeting-text">Welcome, Laraib!</h1>
    </div>
    </section>

<section id="sec2" class="section" aria-label="Section 2">
  <div class="card-wrap">
    <div class="envelope" data-index="2" aria-hidden="false">
      <div class="flap" aria-hidden="true"></div>
      <div class="letter" role="article" aria-labelledby="title2">
        <div class="card-content">
          <h1 id="title2">✨ **Happy Birthday, Laraib!** ✨</h1>
          <p>آج کا دن آپ کے لیے روشنیوں سے بھرا ہوا ہے، Laraib—</p>
          <div class="quote">"Aaj ka din waqai bohot khaas hai."</div>
        </div>
      </div>
      <div class="body"></div>
    </div>

    <div class="controls">
      <button class="btn" onclick="openEnvelope(2)">Open Card</button>
      <button class="btn secondary" onclick="skipOpen(2)">Skip</button>
    </div>
  </div>
</section>

<section id="sec3" class="section" aria-label="Section 3">
  <div class="card-wrap">
    <div class="envelope" data-index="3">
      <div class="flap"></div>
      <div class="letter" role="article" aria-labelledby="title3">
        <div class="card-content">
          <h2 id="title3">Aap Jaisi Khoobsurat Insaan</h2>
          <p>آپ کے اخلاق، آپ کی سچائی، آپ کی نرمی اور آپ کی باریک حسِ جمال-آپ اُن چند لوگوں میں سے ہیں جو چہرے سے زیادہ دل کے خوبصورت ہوتے ہیں۔</p>
          <div class="quote">"Aap jaisi achi, pyari, seedhi aur sachi insaan ko hamesha duniya ki sab se behtareen cheezein milni chahiye. Aapka ikhlaq, lehja aur soch aap ko sab se alag banati hain"</div>
        </div>
      </div>
      <div class="body"></div>
    </div>

    <div class="controls">
      <button class="btn" onclick="openEnvelope(3)">Open Card</button>
      <button class="btn secondary" onclick="skipOpen(3)">Skip</button>
    </div>
  </div>
</section>

<section id="sec4" class="section" aria-label="Section 4">
  <div class="card-wrap">
    <div class="envelope" data-index="4">
      <div class="flap"></div>
      <div class="letter" role="article" aria-labelledby="title4">
        <div class="card-content">
          <h2 id="title4">Yaadein Jo Reh Gayi</h2>
          <p>آپ ہمیشہ سب کے لیے اچھا سوچنے والی، ہر ایک کے کام آنے والی، اور دوسروں کی خوشی میں خوش ہونے والی لڑکی ہیں، اور ایسے لوگ واقعی کم ہوتے ہیں۔ 

"Mujhe abhi tak woh din yaad hai jab hum shed se wapis aa rahe thay aur barish ho rahi thi… aur mere mana karne ke bawajood ap ne pani me jump kiya."
"Aur phir aap ke haath ka banaya hua pulao aur custard — abhi tak uski khushboo yaad aati hai."</p>
        </div>
      </div>
      <div class="body"></div>
    </div>

    <div class="controls">
      <button class="btn" onclick="openEnvelope(4)">Open Card</button>
      <button class="btn secondary" onclick="skipOpen(4)">Skip</button>
    </div>
  </div>
</section>

<section id="sec5" class="section" aria-label="Section 5">
  <div class="card-wrap">
    <div class="envelope" data-index="5">
      <div class="flap"></div>
      <div class="letter" role="article" aria-labelledby="title5">
        <div class="card-content">
          <h2 id="title5">Aap Ki Aankhein</h2>
          <p>آپ کی آنکھیں—وہ گہرا سیاہ رنگ جو عام نہیں، ایک ایسے راز کی طرح ہے جو صرف خوبصورتی نہیں… گہرائی بھی رکھتا ہے۔ 

"Aap ki aankhein woh gehra kaala rang jo na sirf khoobsurat hain balkay puri kainat in ma samai hoi ha."
"Aap ki aankhon me koi ajeeb si khamosh chamak hai jo dekhne wale ko rok leti hai."

نور ہی نور سے مکھڑے پہ وہ نوری آنکھیں
    
اس کے انجیل سے چہرے پہ زبوری آنکھیں</p>
        </div>
      </div>
      <div class="body"></div>
    </div>

    <div class="controls">
      <button class="btn" onclick="openEnvelope(5)">Open Card</button>
      <button class="btn secondary" onclick="skipOpen(5)">Skip</button>
    </div>
  </div>
</section>

<section id="sec6" class="section" aria-label="Section 6">
  <div class="card-wrap">
    <div class="envelope" data-index="6">
      <div class="flap"></div>
      <div class="letter" role="article" aria-labelledby="title6">
        <div class="card-content">
          <h2 id="title6">Duaen & Motivation</h2>
          <p>میں دعا کرتا ہوں کہ اللہ تعالیٰ آپ کی زندگی کو آسانیوں سے بھر دے۔</p>
          <div class="quote">"Main dua karta hoon ke Allah aap ke tamam goals aasaan kar de."
"Aap jahan bhi jaayein, izzat, mohabbat aur achi niyat wale log milain.Aapka dil hamesha halka aur khush rahe.Laraib… aap intelligent aur sincere hain.
“Jahan niyat saaf hoti hai, wahan raasta ban hi jaata hai.”
“Aap kamzor nahi — bas nazuk dil ki hain. Aur nazuk dil wale hi asli strong hote hain.”"</div>
        </div>
      </div>
      <div class="body"></div>
    </div>

    <div class="controls">
      <button class="btn" onclick="openEnvelope(6)">Open Card</button>
      <button class="btn secondary" onclick="skipOpen(6)">Skip</button>
    </div>
  </div>
</section>

<section id="sec7" class="section" aria-label="Section 7">
  <div class="card-wrap">
    <div class="envelope" data-index="7">
      <div class="flap"></div>
      <div class="letter" role="article" aria-labelledby="title7">
        <div class="card-content">
          <h2 id="title7">End Note</h2>
          <p> اللہ آپ کو خوشیوں، مسکراہٹوں، کامیابیوں اور محبتوں سے نوازے۔ 

Happy Birthday once again, Laraib! Allah kare yeh saal aap ki zindagi ka sab se behtareen saal ho. Aap hamesha muskurayein, chamkain aur khush rahein.</p>
        </div>
      </div>
      <div class="body"></div>
    </div>

    <div class="controls">
      <button class="btn" onclick="openEnvelope(7)">Open Card</button>
      <button class="btn secondary" onclick="skipOpen(7)">Skip</button>
    </div>

    <audio id="bgMusic" src="assets/ma_agar_kahon_tum_sa_haseen.mp3" loop preload="auto" aria-hidden="true"></audio>
  </div>
</section>

<section id="sec8" class="section" aria-label="Section 8">
  <div class="card-wrap" style="align-items:center">
    <div class="envelope" data-index="8">
      <div class="flap"></div>
      <div class="letter" role="article" aria-labelledby="title8">
        <div class="card-content">
          <h2 id="title8">🎂 Surprise & Celebration</h2>
          <p>Happy Birthday once again, Laraib! Enjoy the surprise — press "**Next**" to cut the cake 🎉</p>
        </div>
      </div>
      <div class="body"></div>
    </div>

    <div class="controls" style="margin-bottom:14px;">
      <button class="btn" onclick="openEnvelope(8)">Open Card</button>
      <button class="btn secondary" onclick="skipOpen(8)">Skip</button>
    </div>
  </div>
</section>

<section id="sec9" class="section" aria-label="Section 9 - Cake Cutting">
    <div class="card-wrap" style="align-items:center">
        <h2 style="color:var(--accent-2); margin-bottom:10px;">Let's Cut the Cake!</h2>

        <div id="cake-container">
            <img id="knife" src="assets/knife.png" alt="" aria-hidden="true" />
            <img id="cake" src="assets/cake.png" alt="Birthday cake" />
            <h1 id="celebrationText">Happy Birthday Laraib!</h1>
        </div>
    
        <div style="margin-top:20px; padding-bottom: 20px;">
            <button id="cutBtn" class="btn" onclick="cutCake()">**Cut Cake 🎂**</button>
        </div>
    
        <audio id="finalMusic" src="assets/happy_birthday_song.mp3" preload="auto" aria-hidden="true"></audio>
        <audio id="sliceSound" src="assets/cake_cut.mp3" preload="auto" aria-hidden="true"></audio>
    </div>
</section>

<script>
  // Total sections now 9 (Intro + 7 Envelopes + Cake)
  const totalSections = 9;
  let current = 1; 
  let bgStarted = false;

  function showSection(i){
    // Determine the element ID (intro for 1, secX for others)
    for(let s=1;s<=totalSections;s++){
      const id = s === 1 ? 'intro' : 'sec' + s;
      const el = document.getElementById(id);
      if(!el) continue;
      if(s === i) el.classList.add('active');
      else el.classList.remove('active');
    }
    current = i;
  }

  // AUTOMATIC FUNCTION: Open Door and Start Music/Transition
  function openDoor(){
    const doorContainer = document.querySelector('#intro .door-container');
    
    // 1. Start Music (This should be allowed by the browser since it's user-initiated on load, though we'll keep the promise check)
    const bg = document.getElementById('bgMusic');
    if(bg && !bgStarted){
      // Added a promise check/catch for better browser support
      bg.play().catch(err => console.warn('bgMusic play failed:', err));
      bgStarted = true;
    }
    
    // 2. Open the doors (CSS animation)
    doorContainer.classList.add('door-open');
    
    // 3. Proceed to the first envelope (Section 2) after the animation delay
    setTimeout(() => {
        showSection(2); 
    }, 2200); 
  }

  // Open envelope
  function openEnvelope(idx){
    const env = document.querySelector(`#sec${idx} .envelope`);
    if(!env) return;

    env.classList.add('opened');

    // replace controls
    const controls = env.closest('.card-wrap').querySelector('.controls');
    if(controls){
      setTimeout(()=> {
        controls.innerHTML = `<div><button class="btn" onclick="goNext(${idx})">Next →</button></div>`;
      }, 600);
    }
  }

  function skipOpen(idx){
    const env = document.querySelector(`#sec${idx} .envelope`);
    if(env) env.classList.add('opened');
    const controls = env.closest('.card-wrap').querySelector('.controls');
    if(controls){
      controls.innerHTML = `<div><button class="btn" onclick="goNext(${idx})">Next →</button></div>`;
    }
  }

  function goNext(fromIdx){
    let next = fromIdx + 1;
    if(next > totalSections) next = totalSections;
    showSection(next);
  }

  // Cut cake sequence 
  function cutCake(){
    const knife = document.getElementById('knife');
    const cake = document.getElementById('cake');
    const slice = document.getElementById('sliceSound');
    const final = document.getElementById('finalMusic');
    const celebrationText = document.getElementById('celebrationText');
    const btn = document.getElementById('cutBtn');

    if(btn){ btn.disabled = true; btn.style.opacity = .7; }

    if(slice){ slice.currentTime = 0; slice.play().catch(()=>{}); }
    
    if(final){
      final.currentTime = 0;
      final.play().catch(e => console.warn('finalMusic play failed', e));
    }

    celebrationText.style.opacity = '1';

    knife.style.opacity = '1';
    knife.style.left = '50%';
    knife.style.transform = 'translate(-50%, -80px) rotate(-15deg)'; 

    setTimeout(() => {
        knife.style.transform = 'translate(-50%, 20px) rotate(-5deg)';
        
        setTimeout(()=>{
            cake.style.transform = 'scale(.96)';
            setTimeout(()=>{
                cake.src = 'assets/cake-sliced.png';
                cake.style.transform = 'scale(1)';
            }, 100);
        }, 200);

        setTimeout(() => {
            knife.style.opacity = '0';
            knife.style.left = '120%';
        }, 1500);

    }, 600);

    launchConfetti(250);

    // 14 Second Timer to Stop Everything
    setTimeout(()=>{
      celebrationText.style.opacity = '0';
      if(final){
        final.pause();
        final.currentTime = 0;
      }
      showClosingOverlay();
    }, 14000); 
  }

  function launchConfetti(n){
    const colors = ['#f94144','#ff69b4','#f9c74f','#90be6d','#577590','#b983ff','#ffb3c6'];
    for(let i=0;i<n;i++){
      const el = document.createElement('div');
      el.className = 'confetti';
      el.style.left = (Math.random()*100) + 'vw';
      el.style.background = colors[Math.floor(Math.random()*colors.length)];
      el.style.width = (6 + Math.random()*14) + 'px';
      el.style.height = (8 + Math.random()*16) + 'px';
      el.style.top = '-20px';
      el.style.borderRadius = (Math.random()>0.5? '2px':'50%');
      el.style.opacity = 1;
      el.style.transform = `rotate(${Math.random()*360}deg)`;
      el.style.transition = `transform ${1.8 + Math.random()*1.4}s linear, top ${1.8 + Math.random()*1.4}s linear, opacity 1s ease`;
      document.body.appendChild(el);

      setTimeout(()=>{
        el.style.top = (70 + Math.random()*30) + 'vh';
        el.style.transform = `translateY(${120 + Math.random()*60}vh) rotate(${Math.random()*720}deg)`;
      }, 20);

      setTimeout(()=> el.remove(), 4200 + Math.random()*800);
    }
  }

  function showClosingOverlay(){
    const overlay = document.createElement('div');
    overlay.style.position = 'fixed';
    overlay.style.inset = '0';
    overlay.style.display = 'flex';
    overlay.style.alignItems = 'center';
    overlay.style.justifyContent = 'center';
    overlay.style.background = 'rgba(0,0,0,0.85)';
    overlay.style.color = '#fff';
    overlay.style.zIndex = 99999;
    overlay.style.fontSize = '24px';
    overlay.style.fontFamily = 'Segoe UI, Roboto, Arial, sans-serif';
    overlay.style.opacity = '0';
    overlay.style.transition = 'opacity .5s';
    overlay.innerHTML = '<h1>🎉 Celebration Complete! 🎉</h1><p>Happy Birthday Laraib</p>';
    overlay.style.flexDirection = 'column';
    overlay.style.textAlign = 'center';
    
    document.body.appendChild(overlay);
    requestAnimationFrame(()=> overlay.style.opacity = '1');

    setTimeout(()=> {
      overlay.style.opacity = '0';
      setTimeout(()=> overlay.remove(), 600);
    }, 4000);
  }

  (function init(){
    showSection(1);
    // Automatic start: Calls openDoor immediately after the initial section display
    setTimeout(openDoor, 50); 
  })();
</script>
</body>
</html>

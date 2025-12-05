<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Happy Birthday Laraib — Card</title>
<style>
    /* SIMPLIFIED COLOR THEME (Hardcoded for maximum compatibility) */
    
    /* Original Colors */
    --ACCENT-PINK: #ff69b4; 
    --ACCENT-PURPLE: #800080;
    --ACCENT-GOLD: #FFD700;
    --DARK-TEXT: #4b2e83;
    
    html,body{
        height:100%;
        margin:0;
        font-family: "Segoe UI", Roboto, Arial, sans-serif;
        background: #f7eaf0; /* Pale background for the body */
        background-image: radial-gradient(circle at top right, #fff0f5 10%, transparent 50%),
            linear-gradient(180deg, #ffd9eb 0%, #ffffff 80%);
        -webkit-font-smoothing:antialiased;
        -moz-osx-font-smoothing:grayscale;
        color:var(--DARK-TEXT); /* Still using the dark text variable */
        overflow:hidden;
    }
    
    /* Confetti Styles */
    .confetti {
        position: absolute;
        width: 10px;
        height: 10px;
        opacity: 0;
        pointer-events: none;
        z-index: 9999;
        box-shadow: 0 1px 3px rgba(0,0,0,0.2);
    }

    /* Layout for scrolling on mobile (Applied to active sections) */
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
    /* INTRO DOOR STYLES (Kept for the opening screen) */
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
        background: linear-gradient(180deg, #4b2e83, #800080); /* Deep Purple Door */
        transform-origin: center center;
        transition: transform 1.5s cubic-bezier(0.86, 0, 0.07, 1);
        box-shadow: 0 15px 45px rgba(0,0,0,0.6);
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

    #intro.door-open .door.left {
        transform: translateX(-100%) rotateY(-130deg);
    }

    #intro.door-open .door.right {
        transform: translateX(100%) rotateY(130deg);
    }

    .greeting-text {
        position: absolute;
        font-size: 2.8rem; 
        font-weight: bold;
        color: #ff69b4; /* ACCENT-PINK */
        text-shadow: 0 0 10px rgba(255, 105, 180, 0.6), 0 0 20px #fff, 2px 2px 4px #800080;
        opacity: 0;
        transition: opacity 0.8s ease 0.5s;
        text-align: center;
        z-index: 5;
    }
    #intro.door-open .greeting-text {
        opacity: 1;
    }


    /* ------------------------------------------------ */
    /* GENERAL CARD STYLES (ENVELOPE - NEW DESIGN) */
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
        padding-top: 40px;
    }

    .envelope {
        width: 720px;
        max-width: 94%;
        height: 420px;
        position: relative;
        perspective: 1400px;
        cursor: pointer;
    }

    .envelope .body {
        position:absolute;
        inset:0;
        border-radius:16px; 
        background: #f7eaf0; /* Pale Pink Body */
        box-shadow: 0 12px 40px rgba(20,10,60,0.1);
        overflow:visible;
        display:flex;
        align-items:center;
        justify-content:center;
        padding:28px;
        box-sizing:border-box;
    }

    /* NEW FLAP STYLE */
    .envelope .flap {
        position:absolute;
        top:0;
        left:0;
        width:100%;
        height:100%;
        transform-origin: top center;
        background: linear-gradient(135deg, #ff69b4, #ff4d94); /* Pink to Lighter Pink */
        border-radius:16px;
        border-top: 5px solid #FFD700; /* Gold border */
        box-shadow: 0 15px 45px rgba(255, 105, 180, 0.6);
        transform-style: preserve-3d;
        transition: transform .8s cubic-bezier(.2,.9,.3,1);
        backface-visibility: hidden;
        z-index:8;
        display:flex;
        align-items:center;
        justify-content:center;
    }
    
    /* ENHANCED: Flap content */
    .envelope .flap::after{
        content: "✨ Tap to Open Card ✨";
        display:block;
        font-size: 26px;
        font-weight: 900;
        color: #FFD700; /* Gold color for text */
        text-shadow: 0 0 10px rgba(255,215,0,0.5), 0 2px 4px rgba(0,0,0,0.5);
    }
    
    /* LETTER STYLE */
    .envelope .letter {
        position:absolute;
        left:6%;
        width:88%;
        height:80%;
        top:10%;
        background: #ffffff;
        border-radius:10px;
        box-shadow: 0 10px 40px rgba(30,10,60,0.1), inset 0 0 10px rgba(0,0,0,0.05);
        transform-origin: bottom center;
        transform: translateY(28px) scale(.98);
        opacity:0;
        transition: transform .8s cubic-bezier(.2,.9,.3,1), opacity .6s;
        padding:28px; 
        box-sizing:border-box;
        z-index:6;
        display:flex;
        flex-direction:column;
        justify-content:flex-start;
        align-items:flex-start;
        overflow-y:auto;
        scrollbar-width: thin;
        -webkit-overflow-scrolling: touch;
    }

    .envelope.opened .flap {
        transform: rotateX(-180deg);
    }
    .envelope.opened .letter {
        transform: translateY(0) scale(1);
        opacity:1;
    }
    
    .card-content{
        width:100%;
        color:#4b2e83; 
        text-align:left;
    }
    .card-content h1,
    .card-content h2 {
        margin:0 0 10px 0;
        font-size:24px;
        color:#800080; /* ACCENT-PURPLE */
        text-align:center;
        width:100%;
        text-shadow: 1px 1px 0 #FFD700, 0 0 2px rgba(128,0,128,0.2);
        line-height: 1.2;
    }
    .card-content h1 {
        font-size: 30px;
        padding-bottom: 5px;
        border-bottom: 2px dashed #ffe6f2;
    }
    .card-content p{
        margin:0 0 14px 0;
        font-size:18px;
        line-height:1.6;
        color:#4b2e83; 
        white-space: pre-wrap;
    }
    .quote{
        margin-top:10px;
        font-style:italic;
        color:#ff69b4; /* ACCENT-PINK */
        text-align:center;
        background: #fff0f5;
        padding: 10px 15px;
        border-radius: 8px;
        border-left: 5px solid #ff69b4; /* ACCENT-PINK */
        margin-bottom: 15px;
    }
    
    .controls{
        display:flex;
        gap:15px;
        margin-top:12px;
        z-index: 10;
    }
    .btn{
        appearance:none;
        border:0;
        padding:12px 24px; 
        border-radius:12px; 
        background:linear-gradient(45deg, #ff69b4, #9933cc); /* Pink to Royal Purple */
        color:#fff;
        cursor:pointer;
        font-size:18px; 
        font-weight: bold;
        box-shadow: 0 8px 20px rgba(255, 105, 180, 0.4), 0 2px 4px rgba(0,0,0,0.1);
        transition: all 0.2s ease;
    }
    .btn:hover:not(:disabled) {
        transform: translateY(-2px);
        box-shadow: 0 10px 25px rgba(255, 105, 180, 0.6);
    }
    .btn.secondary{
        background:transparent;
        color:#4b2e83;
        box-shadow:none;
        border:2px solid #800080;
    }
    .btn.secondary:hover:not(:disabled) {
        background: #fffde7;
        transform: translateY(-1px);
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    .btn:disabled {
        opacity: 0.5;
        cursor: not-allowed;
    }
    
    /* ------------------------------------------------ */
    /* CAKE STYLES (Guaranteed Centering) */
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
        box-shadow: 0 10px 30px rgba(0,0,0,0.2);
    }
    
    /* Celebration Text (ONLY GLOW EFFECT - Removed opaque background) */
    @keyframes pulseGlow {
        from { text-shadow: 0 0 10px #ff69b4, 0 0 30px #FFD700, 0 0 50px #ff69b4; }
        to { text-shadow: 0 0 20px #ff69b4, 0 0 40px #FFD700, 0 0 60px #fff; }
    }
    #celebrationText {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%); 
        width: 100%;
        text-align: center;
        font-size: 4rem; 
        font-weight: 900; 
        color: #FFD700; /* Gold text */
        text-shadow: 0px 0px 30px rgba(255, 215, 0, 0.9), 0px 4px 12px rgba(139,46,255,0.3);
        opacity: 0;
        z-index: 10;
        pointer-events: none;
        transition: opacity 1.5s ease-in-out;
        animation: pulseGlow 1.5s infinite alternate; 
    }


    @media (max-width:900px){
        .envelope { width: 92%; height: 420px; }
        .envelope .letter { height:80%; top:10%; padding:20px; }
        .card-content p{ font-size:16px; }
        #cake { width:380px; } 
        #cake-container { height: 400px; max-width: 380px; } 
        #celebrationText { font-size: 3rem; }
        .btn { padding: 10px 20px; font-size: 16px; }
    }

    @media (max-width:520px){
        .envelope{ height:360px; }
        .envelope .letter { height:80%; top:10%; padding:16px; }
        .card-content h1{ font-size:24px; }
        .card-content h2{ font-size:20px; }
        .card-content p{ font-size:15px; }
        #celebrationText { font-size: 2.5rem; }
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
        <div class="envelope" data-index="2" onclick="openEnvelope(2)">
            <div class="flap"></div>
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
        <div class="envelope" data-index="3" onclick="openEnvelope(3)">
            <div class="flap"></div>
            <div class="letter" role="article" aria-labelledby="title3">
                <div class="card-content">
                    <h2 id="title3">Aap Jaisi Khoobsurat Insaan</h2>
                    <p>آپ کے اخلاق، آپ کی سچائی، آپ کی نرمی اور آپ کی باریک حسِ جمال-آپ اُن چند لوگوں میں سے ہیں جو چہرے سے زیادہ دل کے خوبصورت ہوتے ہیں۔</p>
                    <div class="quote">"Aap jaisi achi, pyari, seedhi aur sachi insaan ko hamesha duniya ki sab se behtareen cheezein milni chahiye. Aapka ikhlaq, lehja aur soch aap ko sab se alag banati hain"</div>
                    <p>یہ سچ ہے کہ وقت بدل جاتا ہے، لیکن کچھ رشتے اور کچھ خوبصورت یادیں ہمیشہ دل میں محفوظ رہتی ہیں۔ آپ کی معصومیت اور وہ خلوص جو آپ کی باتوں میں ہمیشہ نظر آیا، وہ ایک ایسا سرمایہ ہے جو بہت کم لوگوں کے پاس ہوتا ہے۔ اللہ کرے آپ کے ہر قدم پر آسانیاں ہوں اور آپ کے تمام ارمان پورے ہوں۔ میری دعا ہے کہ آپ کی زندگی کا یہ سال خوشیوں، کامیابیوں اور ڈھیر ساری محبتوں سے بھر پور ہو۔</p>
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
        <div class="envelope" data-index="4" onclick="openEnvelope(4)">
            <div class="flap"></div>
            <div class="letter" role="article" aria-labelledby="title4">
                <div class="card-content">
                    <h2 id="title4">Yaadein Jo Reh Gayi</h2>
                    <p>آپ ہمیشہ سب کے لیے اچھا سوچنے والی، ہر ایک کے کام آنے والی، اور دوسروں کی خوشی میں خوش ہونے والی لڑکی ہیں، اور ایسے لوگ واقعی کم ہوتے ہیں۔ 

<div class="quote" style="border-left: 5px solid var(--gold);">"Mujhe abhi tak woh din yaad hai jab hum shed se wapis aa rahe thay aur barish ho rahi thi… aur mere mana karne ke bawajood ap ne pani me jump kiya."
"Aur phir aap ke haath ka banaya hua pulao aur custard — abhi tak uski khushboo yaad aati hai."</div></p>
                    <p>کاش یہ وقت واپس آ جائے، وہ سب معصوم باتیں، وہ سب ہنسی مذاق اور وہ بے فکری کے دن۔ لیکن اب بھی جہاں کہیں بھی آپ ہوں، میری دعا ہے کہ آپ وہاں مکمل خوش اور پرسکون رہیں۔ یہ یادیں ہماری دوستی کی بنیاد ہیں، اور مجھے فخر ہے کہ میں آپ کو جانتا ہوں۔</p>
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
        <div class="envelope" data-index="5" onclick="openEnvelope(5)">
            <div class="flap"></div>
            <div class="letter" role="article" aria-labelledby="title5">
                <div class="card-content">
                    <h2 id="title5">Aap Ki Aankhein</h2>
                    <p>آپ کی آنکھیں—وہ گہرا سیاہ رنگ جو عام نہیں، ایک ایسے راز کی طرح ہے جو صرف خوبصورتی نہیں… گہرائی بھی رکھتا ہے۔ 

<div class="quote" style="border-left: 5px solid var(--accent-2); color: var(--accent-2); font-style: normal;">"Aap ki aankhein woh gehra kaala rang jo na sirf khoobsurat hain balkay puri kainat in ma samai hoi ha."
"Aap ki aankhon me koi ajeeb si khamosh chamak hai jo dekhne wale ko rok leti hai."</div></p>
                    <p style="text-align: center; font-style: italic; font-weight: 600; color: var(--accent-1);">نور ہی نور سے مکھڑے پہ وہ نوری آنکھیں
    
اس کے انجیل سے چہرے پہ زبوری آنکھیں</p>
                    <p>یہ نظم صرف آپ کے لیے لکھی گئی ہے، آپ کی خوبصورتی اس بات کا ثبوت ہے کہ اللہ نے بہت فرصت میں دنیا بنائی ہے۔ ہمیشہ اپنی اس منفرد پہچان کو برقرار رکھیے گا۔</p>
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
        <div class="envelope" data-index="6" onclick="openEnvelope(6)">
            <div class="flap"></div>
            <div class="letter" role="article" aria-labelledby="title6">
                <div class="card-content">
                    <h2 id="title6">Duaen & Motivation</h2>
                    <p>میں دعا کرتا ہوں کہ اللہ تعالیٰ آپ کی زندگی کو آسانیوں سے بھر دے۔</p>
                    <div class="quote">"Main dua karta hoon ke Allah aap ke tamam goals aasaan kar de."
"Aap jahan bhi jaayein, izzat, mohabbat aur achi niyat wale log milain.Aapka dil hamesha halka aur khush rahe.Laraib… aap intelligent aur sincere hain.
“Jahan niyat saaf hoti hai, wahan raasta ban hi jaata hai.”
“Aap kamzor nahi — bas nazuk dil ki hain. Aur nazuk dil wale hi asli strong hote hain.”"</div>
                    <p>آپ کی محنت اور سچائی کو کوئی نہیں روک سکتا۔ بس یقین رکھیں اور آگے بڑھیں۔</p>
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
        <div class="envelope" data-index="7" onclick="openEnvelope(7)">
            <div class="flap"></div>
            <div class="letter" role="article" aria-labelledby="title7">
                <div class="card-content">
                    <h2 id="title7">End Note</h2>
                    <p> اللہ آپ کو خوشیوں، مسکراہٹوں، کامیابیوں اور محبتوں سے نوازے۔ 

<div class="quote" style="border-left: 5px solid var(--accent-1);">Happy Birthday once again, Laraib! Allah kare yeh saal aap ki zindagi ka sab se behtareen saal ho. Aap hamesha muskurayein, chamkain aur khush rahein.</div></p>
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
        <div class="envelope" data-index="8" onclick="openEnvelope(8)">
            <div class="flap"></div>
            <div class="letter" role="article" aria-labelledby="title8">
                <div class="card-content">
                    <h2 id="title8">🎂 Surprise & Celebration</h2>
                    <p>Happy Birthday once again, Laraib! Press "**Next**" to cut the cake 🎉</p>
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
    <div class="card-wrap" style="align-items:center; justify-content: center;">
        <h2 style="color:var(--accent-2); margin-bottom:10px; font-size: 30px; text-shadow: 1px 1px 0 var(--gold);">Let's Cut the Cake!</h2>

        <div id="cake-container">
            <img id="cake" src="assets/cake.png" alt="Birthday cake" />
            </div>
    
        <div style="margin-top:20px; padding-bottom: 20px;">
            <button id="cutBtn" class="btn" onclick="cutCake()">**Cut Cake 🎂**</button>
        </div>
    
        <audio id="finalMusic" src="assets/happy_birthday_song.mp3" preload="auto" aria-hidden="true"></audio>
        <audio id="sliceSound" src="assets/cake_cut.mp3" preload="auto" aria-hidden="true"></audio>
    </div>
</section>

<h1 id="celebrationText">Happy Birthday Laraib!</h1>

<script>
    const totalSections = 9;
    let current = 1; 
    let bgStarted = false;
    let confettiLoopTimer = null; 
    const CELEBRATION_DURATION = 14000; 

    function showSection(i){
        // Ensure all sections are checked, including the intro
        const allSections = document.querySelectorAll('.section');
        allSections.forEach((el, index) => {
            const sectionIndex = (el.id === 'intro') ? 1 : parseInt(el.id.replace('sec', ''));
            
            if (sectionIndex === i) {
                el.classList.add('active');
            } else {
                el.classList.remove('active');
            }
        });
        current = i;
    }

    // Tries to start the background music
    function tryStartBgMusic() {
        const bg = document.getElementById('bgMusic');
        if(bg && !bgStarted){
            bg.play().then(() => {
                bgStarted = true;
            }).catch(err => {
                console.warn('bgMusic play failed (mobile restriction likely):', err);
            });
        }
    }

    // FIX: Open Door logic with the correct class name for the intro element
    function openDoor(){
        const introSection = document.getElementById('intro');
        
        // 1. Add the door-open class to the section itself for CSS
        introSection.classList.add('door-open');
        
        // 2. Proceed to the first envelope (Section 2)
        setTimeout(() => {
            showSection(2); 
        }, 2200); 
    }

    function openEnvelope(idx){
        // *** START MUSIC ON FIRST USER INTERACTION (SECTION 2) ***
        if (idx === 2) {
            tryStartBgMusic();
        }
        
        const env = document.querySelector(`#sec${idx} .envelope`);
        if(!env) return;

        env.classList.add('opened');

        const controls = env.closest('.card-wrap').querySelector('.controls');
        if(controls){
            setTimeout(()=> {
                controls.innerHTML = `<div><button class="btn" onclick="goNext(${idx})">Next →</button></div>`;
            }, 600);
        }
    }

    function skipOpen(idx){
        // *** START MUSIC ON FIRST USER INTERACTION (SECTION 2) ***
        if (idx === 2) {
            tryStartBgMusic();
        }

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

    function cutCake(){
        const cake = document.getElementById('cake');
        const slice = document.getElementById('sliceSound');
        const final = document.getElementById('finalMusic');
        const celebrationText = document.getElementById('celebrationText');
        const btn = document.getElementById('cutBtn');

        if(btn){ btn.disabled = true; btn.innerHTML = 'Celebrating...'; btn.style.opacity = .7; }

        if(slice){ slice.currentTime = 0; slice.play().catch(()=>{}); }
        
        if(final){
            // Stop the BG music and start the final song
            document.getElementById('bgMusic').pause();
            document.getElementById('bgMusic').currentTime = 0;
            
            final.currentTime = 0;
            final.play().catch(e => console.warn('finalMusic play failed', e));
        }

        // Show the celebration text (now just a glowing effect)
        celebrationText.style.opacity = '1';

        // Cake slice animation (no knife involved)
        setTimeout(()=>{
            cake.style.transform = 'scale(.96)';
            setTimeout(()=>{
                cake.src = 'assets/cake-sliced.png';
                cake.style.transform = 'scale(1)';
            }, 100);
        }, 600);


        // Start continuous confetti loop
        startConfettiLoop();

        // End celebration after CELEBRATION_DURATION
        setTimeout(()=>{
            // Clear confetti loop
            clearInterval(confettiLoopTimer); 

            celebrationText.style.opacity = '0';
            if(final){
                final.pause();
                final.currentTime = 0;
            }
            showClosingOverlay();
        }, CELEBRATION_DURATION); 
    }

    function startConfettiLoop() {
        launchConfetti(80);

        confettiLoopTimer = setInterval(() => {
            launchConfetti(20); 
        }, 500);
    }

    function launchConfetti(n){
        const colors = ['#f94144','var(--accent-1)','var(--gold)','#90be6d','#577590','#b983ff','#ffb3c6', '#800080'];
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
            el.style.transition = `transform ${2.5 + Math.random()*1.5}s linear, top ${2.5 + Math.random()*1.5}s linear, opacity 1s ease`;
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
        overlay.style.fontSize = '28px'; /* Slightly larger */
        overlay.style.fontFamily = 'Segoe UI, Roboto, Arial, sans-serif';
        overlay.style.opacity = '0';
        overlay.style.transition = 'opacity .5s';
        overlay.innerHTML = '<h1 style="color: var(--gold); text-shadow: 0 0 10px rgba(255, 215, 0, 0.8);">🎉 Happy Birthday Laraib! 🎉</h1><p style="font-size: 18px; color: #fff;">Wish you all the best.</p>';
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
        // Automatic door opening is still here
        setTimeout(openDoor, 50); 
    })();
</script>
</body>
</html>

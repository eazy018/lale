
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Para ti</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@500;700&family=Cormorant+Garamond:ital,wght@0,400;0,600;1,400&display=swap" rel="stylesheet">
<style>
  :root{
    --bg-pink: #f6d8e3;
    --bg-pink-2: #f0c3d6;
    --paper: #fdf6ef;
    --ink: #5c3244;
    --seal: #b83b56;
    --seal-dark: #8f2c42;
    --gold: #cf9f4e;
    --heart: #e8617e;
  }

  *{ box-sizing: border-box; }

  html, body{
    margin: 0;
    height: 100%;
    overflow: hidden;
  }

  body{
    font-family: 'Cormorant Garamond', serif;
    background: radial-gradient(circle at 50% 30%, var(--bg-pink) 0%, var(--bg-pink-2) 65%, #e8a9c4 100%);
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    min-height: 100vh;
  }

  /* ===== INTRO SCREEN ===== */
  .intro{
    position: fixed;
    inset: 0;
    z-index: 50;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 2.2rem;
    background: radial-gradient(circle at 50% 40%, var(--bg-pink) 0%, var(--bg-pink-2) 60%, #e8a9c4 100%);
    transition: opacity 0.7s ease, visibility 0.7s ease;
  }
  .intro.hidden{
    opacity: 0;
    visibility: hidden;
    pointer-events: none;
  }

  .intro-glow{
    position: absolute;
    left: 50%;
    top: 50%;
    width: 480px;
    height: 480px;
    max-width: 130vw;
    max-height: 130vw;
    transform: translate(-50%, -50%);
    background: radial-gradient(circle, rgba(255,235,240,0.85) 0%, rgba(255,205,222,0.3) 40%, rgba(255,205,222,0) 70%);
    animation: halo-breathe 4.5s ease-in-out infinite;
    z-index: 0;
  }

  .intro-text{
    position: relative;
    z-index: 1;
    font-family: 'Dancing Script', cursive;
    font-weight: 700;
    font-size: clamp(1.8rem, 8vw, 2.6rem);
    color: var(--ink);
    text-align: center;
    line-height: 1.4;
    margin: 0;
    padding: 0 1.5rem;
    text-shadow: 0 2px 12px rgba(255,255,255,0.6);
    animation: intro-in 0.9s cubic-bezier(.2,.9,.25,1.05) both,
               intro-sway 3.6s ease-in-out infinite 0.9s;
  }

  @keyframes intro-sway{
    0%, 100%{ transform: translateY(0) rotate(-0.6deg); }
    50%{ transform: translateY(-8px) rotate(0.6deg); }
  }

  .intro-btn{
    position: relative;
    z-index: 1;
    font-family: 'Cormorant Garamond', serif;
    font-weight: 600;
    font-size: 1.3rem;
    letter-spacing: 0.06em;
    border: none;
    border-radius: 999px;
    padding: 0.9rem 2.4rem;
    cursor: pointer;
    color: #fff6f2;
    background: linear-gradient(160deg, var(--heart), var(--seal-dark));
    box-shadow:
      0 10px 22px -8px rgba(184,59,86,0.6),
      0 0 18px -2px rgba(232,97,126,0.55),
      inset 0 1px 0 rgba(255,255,255,0.35);
    animation: intro-in 0.9s cubic-bezier(.2,.9,.25,1.05) 0.15s both, yes-pulse 1.8s ease-in-out infinite 1.2s;
  }
  .intro-btn:hover{ animation-play-state: paused; transform: translateY(-2px) scale(1.03); }
  .intro-btn:focus-visible{ outline: 3px solid var(--gold); outline-offset: 2px; }

  @keyframes intro-in{
    from{ opacity: 0; transform: translateY(14px); }
    to{ opacity: 1; transform: translateY(0); }
  }

  /* scene stays hidden until intro is dismissed */
  .scene{ opacity: 0; transition: opacity 0.8s ease; }
  .scene.revealed{ opacity: 1; }

  /* ===== FLOATING PHOTO STICKERS ===== */
  .stickers{
    position: fixed;
    inset: 0;
    z-index: 40;
    pointer-events: none;
    transition: opacity 0.6s ease;
  }
  .stickers.hidden{ opacity: 0; }

  .sticker{
    position: absolute;
    width: 82px;
    padding: 6px 6px 8px;
    background: #fffdfb;
    border-radius: 10px;
    box-shadow:
      0 14px 26px -10px rgba(90,30,55,0.4),
      0 0 0 1px rgba(255,255,255,0.6);
    animation: sticker-float 5s ease-in-out infinite;
  }
  .sticker img{
    display: block;
    width: 100%;
    height: auto;
    border-radius: 4px;
  }
  .sticker.no-frame{
    background: none;
    padding: 0;
    box-shadow: none;
    filter: drop-shadow(0 8px 14px rgba(90,30,55,0.35));
  }

  @keyframes sticker-float{
    0%, 100%{ transform: translateY(0) rotate(var(--rot, -4deg)); }
    50%{ transform: translateY(-14px) rotate(calc(var(--rot, -4deg) * -1)); }
  }

  @media (min-width: 640px){
    .sticker{ width: 108px; }
  }

  /* ambient floating hearts in background */
  .ambient{
    position: fixed;
    inset: 0;
    pointer-events: none;
    overflow: hidden;
    z-index: 0;
  }
  .ambient img{
    position: absolute;
    bottom: -10%;
    width: 28px;
    height: auto;
    opacity: 0;
    animation: float-up linear infinite;
    filter: drop-shadow(0 0 6px rgba(232,97,126,0.65)) drop-shadow(0 0 14px rgba(255,255,255,0.35));
  }
  @keyframes float-up{
    0%{ transform: translateY(0) rotate(0deg); opacity: 0; }
    10%{ opacity: 0.8; }
    90%{ opacity: 0.6; }
    100%{ transform: translateY(-110vh) rotate(25deg); opacity: 0; }
  }

  /* ===== ENVELOPE ===== */
  .scene{
    position: relative;
    z-index: 1;
    perspective: 1400px;
  }

  .glow-halo{
    position: absolute;
    left: 50%;
    top: 50%;
    width: 620px;
    height: 620px;
    max-width: 140vw;
    max-height: 140vw;
    transform: translate(-50%, -50%);
    background: radial-gradient(circle, rgba(255,235,240,0.85) 0%, rgba(255,205,222,0.35) 35%, rgba(255,205,222,0) 68%);
    z-index: 0;
    pointer-events: none;
    animation: halo-breathe 4.5s ease-in-out infinite;
  }
  @keyframes halo-breathe{
    0%, 100%{ opacity: 0.7; transform: translate(-50%, -50%) scale(0.96); }
    50%{ opacity: 1; transform: translate(-50%, -50%) scale(1.04); }
  }

  .ground-shadow{
    position: absolute;
    left: 50%;
    bottom: -14%;
    width: min(60vw, 260px);
    height: 26px;
    transform: translateX(-50%);
    background: radial-gradient(ellipse at center, rgba(120,40,65,0.38) 0%, rgba(120,40,65,0) 72%);
    border-radius: 50%;
    z-index: 0;
    animation: shadow-breathe 3.6s ease-in-out infinite;
    transition: opacity 0.5s ease;
  }
  @keyframes shadow-breathe{
    0%, 100%{ transform: translateX(-50%) scale(1); opacity: 0.85; }
    50%{ transform: translateX(-50%) scale(0.82); opacity: 0.55; }
  }
  .scene.open .ground-shadow{ opacity: 0; }

  .envelope{
    width: min(78vw, 340px);
    height: min(52vw, 230px);
    position: relative;
    cursor: pointer;
    transition: transform 0.4s ease;
    animation: envelope-float 3.6s ease-in-out infinite;
  }
  .envelope:hover{ animation-play-state: paused; transform: translateY(-6px) rotate(-1deg) scale(1.02); }

  @keyframes envelope-float{
    0%, 100%{ transform: translateY(0) rotate(-0.6deg); }
    50%{ transform: translateY(-10px) rotate(0.6deg); }
  }

  .envelope-body{
    position: absolute;
    inset: 0;
    background: linear-gradient(160deg, #fbeaf1 0%, #f6d3e2 100%);
    border-radius: 6px;
    box-shadow:
      0 22px 45px -14px rgba(140, 50, 80, 0.5),
      0 6px 16px -6px rgba(140, 50, 80, 0.3),
      0 2px 0 rgba(255,255,255,0.5) inset;
  }

  .envelope-shadow-fold{
    position: absolute;
    inset: 0;
    border-radius: 6px;
    background:
      linear-gradient(to bottom right, transparent 49.6%, rgba(140,60,90,0.12) 50%, transparent 50.4%),
      linear-gradient(to bottom left, transparent 49.6%, rgba(140,60,90,0.12) 50%, transparent 50.4%);
    z-index: 3;
    pointer-events: none;
  }

  .flap{
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 58%;
    background: linear-gradient(160deg, #f6d6e4 0%, #eebedb 100%);
    clip-path: polygon(0 0, 100% 0, 50% 96%);
    transform-origin: top center;
    transition: transform 0.9s cubic-bezier(.6,-0.28,.35,1.4);
    z-index: 4;
    box-shadow: 0 1px 4px rgba(140,50,80,0.15);
  }

  .seal{
    position: absolute;
    left: 50%;
    top: 44%;
    width: 52px;
    height: 52px;
    transform: translate(-50%,-50%);
    border-radius: 50%;
    background: radial-gradient(circle at 35% 30%, var(--seal) 0%, var(--seal-dark) 75%);
    box-shadow: 0 3px 8px rgba(0,0,0,0.35), inset 0 -3px 6px rgba(0,0,0,0.25), inset 0 2px 4px rgba(255,255,255,0.25);
    z-index: 6;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #f5d9b8;
    font-size: 22px;
    transition: transform 0.4s ease, opacity 0.4s ease;
    animation: seal-pulse 2s ease-in-out infinite;
  }
  @keyframes seal-pulse{
    0%, 100%{ box-shadow: 0 3px 8px rgba(0,0,0,0.35), inset 0 -3px 6px rgba(0,0,0,0.25), inset 0 2px 4px rgba(255,255,255,0.25), 0 0 0 0 rgba(184,59,86,0.5); }
    50%{ box-shadow: 0 3px 8px rgba(0,0,0,0.35), inset 0 -3px 6px rgba(0,0,0,0.25), inset 0 2px 4px rgba(255,255,255,0.25), 0 0 0 8px rgba(184,59,86,0); }
  }
  .seal img{
    width: 26px;
    height: 26px;
    object-fit: contain;
    filter: drop-shadow(0 1px 1px rgba(0,0,0,0.4)) drop-shadow(0 0 6px rgba(255,200,180,0.7));
  }

  .letter-peek{
    position: absolute;
    left: 6%;
    right: 6%;
    bottom: 4%;
    height: 82%;
    background: var(--paper);
    border-radius: 4px 4px 2px 2px;
    box-shadow: 0 -2px 10px rgba(140,50,80,0.12);
    z-index: 2;
  }

  .hint{
    position: absolute;
    bottom: -2.6rem;
    left: 0; right: 0;
    text-align: center;
    color: var(--ink);
    font-size: 1.05rem;
    letter-spacing: 0.04em;
    opacity: 0.75;
    animation: pulse 2.4s ease-in-out infinite;
  }
  @keyframes pulse{
    0%,100%{ opacity: 0.5; }
    50%{ opacity: 0.9; }
  }

  /* open states */
  .scene.open .flap{
    transform: rotateX(180deg);
    z-index: 1;
  }
  .scene.open .seal{
    opacity: 0;
    transform: translate(-50%,-50%) scale(0.4) rotate(25deg);
  }
  .scene.open .envelope{
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.5s ease 0.35s;
  }
  .scene.open .hint{ display: none; }

  /* ===== LETTER ===== */
  .letter{
    position: fixed;
    left: 50%;
    top: 50%;
    width: min(84vw, 380px);
    max-height: 82vh;
    overflow-y: auto;
    background:
      radial-gradient(120% 60% at 50% 0%, rgba(255,255,255,0.9) 0%, rgba(255,255,255,0) 55%),
      var(--paper);
    background-image:
      radial-gradient(120% 60% at 50% 0%, rgba(255,255,255,0.9) 0%, rgba(255,255,255,0) 55%),
      linear-gradient(rgba(184,59,86,0.05) 1px, transparent 1px);
    background-size: 100% 100%, 100% 2.1rem;
    background-repeat: no-repeat, repeat;
    border-radius: 4px;
    box-shadow:
      0 40px 70px -18px rgba(90,30,55,0.55),
      0 8px 24px -6px rgba(90,30,55,0.3),
      0 0 60px -10px rgba(255,210,225,0.6),
      0 1px 0 rgba(255,255,255,0.6) inset;
    padding: 2.6rem 2rem 2.2rem;
    text-align: center;
    transform: translate(-50%, -40%) scale(0.8) rotate(-6deg);
    opacity: 0;
    z-index: 5;
    pointer-events: none;
    transition: transform 0.9s cubic-bezier(.34,1.56,.64,1) 0.3s, opacity 0.6s ease 0.3s;
  }

  .scene.open .letter{
    transform: translate(-50%, -50%) scale(1) rotate(0deg);
    opacity: 1;
    pointer-events: auto;
  }

  .letter-mark{
    font-family: 'Dancing Script', cursive;
    color: var(--gold);
    font-size: 1.3rem;
    display: block;
    margin-bottom: 0.6rem;
    letter-spacing: 0.06em;
    opacity: 0;
    transform: translateY(10px);
  }
  .scene.open .letter-mark{
    animation: line-in 0.6s ease forwards 0.75s;
  }

  .message{
    font-family: 'Dancing Script', cursive;
    font-weight: 700;
    font-size: clamp(1.5rem, 6vw, 2rem);
    color: var(--ink);
    line-height: 1.5;
    margin: 0 0 1.9rem;
    opacity: 0;
    transform: translateY(10px);
  }
  .scene.open .message{
    animation: line-in 0.7s ease forwards 0.95s;
  }

  .signature{
    font-family: 'Cormorant Garamond', serif;
    font-style: italic;
    color: rgba(92,50,68,0.55);
    font-size: 1rem;
    margin-bottom: 1.6rem;
    opacity: 0;
    transform: translateY(10px);
  }
  .scene.open .signature{
    animation: line-in 0.6s ease forwards 1.2s;
  }

  @keyframes line-in{
    to{ opacity: 1; transform: translateY(0); }
  }

  .choices{
    display: flex;
    gap: 0.9rem;
    justify-content: center;
    flex-wrap: wrap;
    opacity: 0;
    transform: translateY(10px);
  }
  .scene.open .choices{
    animation: line-in 0.6s ease forwards 1.4s;
  }

  button.choice{
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.15rem;
    letter-spacing: 0.02em;
    border: none;
    border-radius: 999px;
    padding: 0.75rem 1.6rem;
    cursor: pointer;
    transition: transform 0.25s ease, box-shadow 0.25s ease;
  }
  button.choice:focus-visible{
    outline: 3px solid var(--gold);
    outline-offset: 2px;
  }

  .btn-yes{
    background: linear-gradient(160deg, var(--heart), var(--seal-dark));
    color: #fff6f2;
    box-shadow:
      0 10px 22px -8px rgba(184,59,86,0.6),
      0 0 18px -2px rgba(232,97,126,0.55),
      inset 0 1px 0 rgba(255,255,255,0.35);
    animation: yes-pulse 1.8s ease-in-out infinite 2s;
  }
  .btn-yes:hover{ transform: translateY(-2px) scale(1.03); animation-play-state: paused; }

  @keyframes yes-pulse{
    0%, 100%{ transform: scale(1); }
    50%{ transform: scale(1.06); }
  }

  .btn-kiss{
    background: transparent;
    color: var(--ink);
    border: 1.5px solid rgba(92,50,68,0.35) !important;
  }
  .btn-kiss:hover{ transform: translateY(-2px) scale(1.03); background: rgba(184,59,86,0.06); }

  /* ===== FINAL ANSWER STATE ===== */
  .final-msg{
    font-family: 'Dancing Script', cursive;
    font-weight: 700;
    font-size: clamp(1.6rem, 7vw, 2.1rem);
    color: var(--seal-dark);
    display: none;
    margin-top: 0.4rem;
  }
  .letter.answered .final-msg{
    display: block;
    animation: pop-in 0.5s cubic-bezier(.34,1.56,.64,1) forwards;
  }
  @keyframes pop-in{
    0%{ transform: scale(0.5); opacity: 0; }
    100%{ transform: scale(1); opacity: 1; }
  }
  .letter.answered .message,
  .letter.answered .choices,
  .letter.answered .signature{ display: none; }

  /* ===== POEM (appears after "sí") ===== */
  .poem{
    display: none;
    margin-top: 0.2rem;
  }
  .letter.show-poem .final-msg{ display: none; }
  .letter.show-poem .poem{
    display: block;
    animation: pop-in 0.7s cubic-bezier(.34,1.56,.64,1) forwards;
  }
  .poem-mark{
    font-family: 'Dancing Script', cursive;
    color: var(--gold);
    font-size: 1.3rem;
    display: block;
    margin-bottom: 1rem;
    letter-spacing: 0.06em;
  }
  .poem p{
    font-family: 'Cormorant Garamond', serif;
    font-style: italic;
    color: var(--ink);
    font-size: 1.12rem;
    line-height: 1.7;
    margin: 0 0 1.2rem;
  }
  .poem .poem-sign{
    font-style: normal;
    color: var(--seal-dark);
    font-size: 1rem;
    margin-top: 0.4rem;
  }

  .letter.show-poem-kiss .final-msg{ display: none; }
  .letter.show-poem-kiss .poem-kiss{
    display: block;
    animation: pop-in 0.7s cubic-bezier(.34,1.56,.64,1) forwards;
  }
  .poem-kiss{ display: none; margin-top: 0.2rem; }
  .poem-kiss p{
    font-family: 'Cormorant Garamond', serif;
    font-style: italic;
    color: var(--ink);
    font-size: 1.12rem;
    line-height: 1.7;
    margin: 0 0 1.2rem;
  }

  .btn-back{
    font-family: 'Cormorant Garamond', serif;
    font-size: 1.05rem;
    letter-spacing: 0.02em;
    border: 1.5px solid rgba(92,50,68,0.35);
    border-radius: 999px;
    padding: 0.65rem 1.5rem;
    cursor: pointer;
    background: transparent;
    color: var(--ink);
    margin-top: 0.6rem;
    transition: transform 0.25s ease, background 0.25s ease;
  }
  .btn-back:hover{ transform: translateY(-2px) scale(1.03); background: rgba(184,59,86,0.06); }
  .btn-back:focus-visible{ outline: 3px solid var(--gold); outline-offset: 2px; }

  /* ===== POEM CATS (top/bottom, ~1fps cartoon wiggle) ===== */
  .poem-cat{
    position: fixed;
    left: 50%;
    width: 110px;
    z-index: 45;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.6s ease;
  }
  .poem-cat img{
    display: block;
    width: 100%;
    height: auto;
    filter: drop-shadow(0 10px 14px rgba(90,30,55,0.35));
  }
  .poem-cat.show{ opacity: 1; }
  .poem-cat.top{
    top: 6%;
    animation: cat-wiggle-top 8s steps(1) infinite;
  }
  .poem-cat.bottom{
    bottom: 6%;
    animation: cat-wiggle-bottom 8s steps(1) infinite;
  }

  @keyframes cat-wiggle-top{
    0%{ transform: translateX(-50%) rotate(-4deg) translateY(0); }
    12.5%{ transform: translateX(-50%) rotate(3deg) translateY(-4px); }
    25%{ transform: translateX(-50%) rotate(-2deg) translateY(2px); }
    37.5%{ transform: translateX(-50%) rotate(4deg) translateY(-3px); }
    50%{ transform: translateX(-50%) rotate(-3deg) translateY(1px); }
    62.5%{ transform: translateX(-50%) rotate(2deg) translateY(-4px); }
    75%{ transform: translateX(-50%) rotate(-4deg) translateY(0); }
    87.5%{ transform: translateX(-50%) rotate(3deg) translateY(2px); }
    100%{ transform: translateX(-50%) rotate(-4deg) translateY(0); }
  }
  @keyframes cat-wiggle-bottom{
    0%{ transform: translateX(-50%) rotate(4deg) translateY(0); }
    12.5%{ transform: translateX(-50%) rotate(-3deg) translateY(-3px); }
    25%{ transform: translateX(-50%) rotate(2deg) translateY(2px); }
    37.5%{ transform: translateX(-50%) rotate(-4deg) translateY(-4px); }
    50%{ transform: translateX(-50%) rotate(3deg) translateY(1px); }
    62.5%{ transform: translateX(-50%) rotate(-2deg) translateY(-3px); }
    75%{ transform: translateX(-50%) rotate(4deg) translateY(0); }
    87.5%{ transform: translateX(-50%) rotate(-3deg) translateY(2px); }
    100%{ transform: translateX(-50%) rotate(4deg) translateY(0); }
  }

  @media (min-width: 640px){
    .poem-cat{ width: 140px; }
  }

  @media (prefers-reduced-motion: reduce){
    .poem-cat{ animation: none; }
  }

  /* ===== HEART BURST ===== */
  .burst{
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 20;
    overflow: hidden;
  }
  .burst img{
    position: absolute;
    top: 50%;
    left: 50%;
    width: 26px;
    height: auto;
    opacity: 0;
    animation: burst-out 1.6s ease-out forwards;
    filter: drop-shadow(0 0 8px rgba(232,97,126,0.8)) drop-shadow(0 2px 3px rgba(90,20,40,0.35));
  }
  @keyframes burst-out{
    0%{ transform: translate(-50%,-50%) scale(0.3); opacity: 0; }
    12%{ opacity: 1; }
    100%{ transform: translate(calc(-50% + var(--dx)), calc(-50% + var(--dy))) scale(1.1) rotate(var(--rot)); opacity: 0; }
  }

  @media (prefers-reduced-motion: reduce){
    .ambient span, .hint, .sticker, .intro-text{ animation: none; }
    .flap, .letter, .seal{ transition-duration: 0.2s; }
    .burst i{ animation-duration: 0.6s; }
  }
</style>
</head>
<body>

<div class="intro" id="intro">
  <div class="intro-glow"></div>
  <p class="intro-text">Tengo algo para ti,<br>¿quieres verlo?</p>
  <button class="intro-btn" id="introBtn">SIIII</button>
</div>

<div class="stickers" id="stickersIntro">
  <div class="sticker" style="left:5%; top:10%; --rot:-7deg; animation-duration:5.4s;">
    <img src="assets/doodle-carita.jpg" alt="">
  </div>
  <div class="sticker" style="left:68%; top:9%; --rot:6deg; animation-duration:4.6s; animation-delay:0.4s;">
    <img src="assets/doodle-conejo.jpg" alt="">
  </div>
  <div class="sticker" style="left:6%; top:66%; --rot:5deg; animation-duration:5s; animation-delay:0.8s;">
    <img src="assets/doodle-abrazo.jpg" alt="">
  </div>
  <div class="sticker" style="left:67%; top:64%; --rot:-6deg; animation-duration:4.8s; animation-delay:0.2s;">
    <img src="assets/doodle-alien.jpg" alt="">
  </div>
  <div class="sticker no-frame" style="left:40%; top:3%; width:64px; animation-duration:4.2s; animation-delay:0.6s;">
    <img src="assets/snoopy.gif" alt="">
  </div>
</div>

<div class="stickers hidden" id="stickersEnvelope">
  <div class="sticker" style="left:4%; top:14%; --rot:-6deg; animation-duration:5.2s;">
    <img src="assets/doodle-alien.jpg" alt="">
  </div>
  <div class="sticker" style="left:70%; top:12%; --rot:7deg; animation-duration:4.7s; animation-delay:0.5s;">
    <img src="assets/doodle-carita.jpg" alt="">
  </div>
  <div class="sticker" style="left:5%; top:70%; --rot:6deg; animation-duration:5s; animation-delay:0.3s;">
    <img src="assets/doodle-conejo.jpg" alt="">
  </div>
  <div class="sticker" style="left:69%; top:68%; --rot:-5deg; animation-duration:4.5s; animation-delay:0.7s;">
    <img src="assets/doodle-abrazo.jpg" alt="">
  </div>
  <div class="sticker no-frame" style="left:44%; top:6%; width:60px; animation-duration:4.3s; animation-delay:0.9s;">
    <img src="assets/snoopy.gif" alt="">
  </div>
</div>

<audio id="bgMusic" src="assets/musica.mp3" loop preload="auto"></audio>

<div class="ambient" id="ambient"></div>

<div class="scene" id="scene">
  <div class="glow-halo"></div>
  <div class="ground-shadow" id="groundShadow"></div>
  <div class="envelope" id="envelope">
    <div class="envelope-body"></div>
    <div class="letter-peek"></div>
    <div class="flap"></div>
    <div class="seal"><img src="assets/corazon.png" alt=""></div>
    <div class="envelope-shadow-fold"></div>
  </div>
  <p class="hint">toca el sobre</p>

  <div class="letter" id="letter">
    <span class="letter-mark">para ti</span>
    <p class="message">Me gustas, te pienso, te sueño…<br>¿dejamos de esconder<br>nuestro amor?</p>
    <p class="signature">— con todo mi corazón</p>
    <div class="choices">
      <button class="choice btn-yes" id="btnYes">sí</button>
      <button class="choice btn-kiss" id="btnKiss">te doy un beso</button>
    </div>
    <p class="final-msg" id="finalMsg"></p>
    <div class="poem" id="poemBlock">
      <span class="poem-mark">para ti, otra vez</span>
      <p>No existe humo capaz de llenar mis pulmones<br>como lo hace el aire que queda después de un beso tuyo.</p>
      <p>Podría perderme entre nubes,<br>dejar que el mundo se vuelva gris,<br>pero ni el humo más profundo<br>lograría reemplazar<br>la forma en que respira mi corazón<br>cuando te encuentra.</p>
      <p>Me siento vivo<br>cada vez que tus brazos me recuerdan<br>que hay un lugar donde pertenezco.<br>Cada abrazo tuyo<br>es una primavera escondida<br>debajo de mi pecho.</p>
      <p>Tus labios tienen esa extraña costumbre<br>de devolverle color a mis días,<br>como si supieran despertar<br>todo lo que creía dormido.</p>
      <p class="poem-sign">— con todo mi corazón</p>
    </div>
    <div class="poem-kiss" id="poemKissBlock">
      <span class="poem-mark">un poquito más</span>
      <p>Cada vez que me besas,<br>mi corazón despierta<br>como un árbol después del invierno,<br>y vuelve a latir<br>con una fuerza<br>que había olvidado.</p>
      <p style="margin-bottom:0.4rem;">Pero devuélvete, porque hay otra cosita que quiero que leas…</p>
      <button class="btn-back" id="btnBack">volver</button>
    </div>
  </div>
</div>

<div class="burst" id="burst"></div>

<div class="poem-cat top" id="poemCatLeft">
  <img src="assets/gato-izquierda.png" alt="">
</div>
<div class="poem-cat bottom" id="poemCatRight">
  <img src="assets/gato-derecha.png" alt="">
</div>

<script>
  // intro screen: reveals the scene and starts the music on the user's first tap
  const intro = document.getElementById('introBtn');
  const introScreen = document.getElementById('intro');
  const bgMusic = document.getElementById('bgMusic');
  const sceneEl = document.getElementById('scene');
  const stickersIntroEl = document.getElementById('stickersIntro');
  const stickersEnvelopeEl = document.getElementById('stickersEnvelope');

  intro.addEventListener('click', () => {
    introScreen.classList.add('hidden');
    sceneEl.classList.add('revealed');
    stickersIntroEl.classList.add('hidden');
    stickersEnvelopeEl.classList.remove('hidden');
    bgMusic.volume = 0.7;
    bgMusic.play().catch(() => {
      // if playback is still blocked for some reason, it will simply stay silent
    });
  });

  // ambient floating hearts
  const ambient = document.getElementById('ambient');
  for(let i=0;i<14;i++){
    const s = document.createElement('img');
    s.src = 'assets/corazon.png';
    s.alt = '';
    s.style.left = Math.random()*100 + '%';
    const size = 20 + Math.random()*22;
    s.style.width = size + 'px';
    s.style.animationDuration = (9 + Math.random()*10) + 's';
    s.style.animationDelay = (Math.random()*10) + 's';
    ambient.appendChild(s);
  }

  const scene = document.getElementById('scene');
  const envelope = document.getElementById('envelope');
  envelope.addEventListener('click', () => {
    scene.classList.add('open');
    stickersEnvelopeEl.classList.add('hidden');
  });

  const letter = document.getElementById('letter');
  const finalMsg = document.getElementById('finalMsg');
  const btnYes = document.getElementById('btnYes');
  const btnKiss = document.getElementById('btnKiss');
  const burstLayer = document.getElementById('burst');
  const poemCatLeft = document.getElementById('poemCatLeft');
  const poemCatRight = document.getElementById('poemCatRight');

  function heartBurst(count){
    for(let i=0;i<count;i++){
      const h = document.createElement('img');
      h.src = 'assets/corazon.png';
      h.alt = '';
      const angle = Math.random()*Math.PI*2;
      const dist = 120 + Math.random()*260;
      h.style.setProperty('--dx', Math.cos(angle)*dist + 'px');
      h.style.setProperty('--dy', Math.sin(angle)*dist + 'px');
      h.style.setProperty('--rot', (Math.random()*360-180) + 'deg');
      h.style.left = (45 + Math.random()*10) + '%';
      h.style.top = (55 + Math.random()*10) + '%';
      h.style.width = (18 + Math.random()*16) + 'px';
      h.style.animationDelay = (Math.random()*0.25) + 's';
      burstLayer.appendChild(h);
      setTimeout(() => h.remove(), 2000);
    }
  }

  function heartBurstAt(x, y, count){
    for(let i=0;i<count;i++){
      const h = document.createElement('img');
      h.src = 'assets/corazon.png';
      h.alt = '';
      const angle = Math.random()*Math.PI*2;
      const dist = 50 + Math.random()*110;
      h.style.setProperty('--dx', Math.cos(angle)*dist + 'px');
      h.style.setProperty('--dy', Math.sin(angle)*dist + 'px');
      h.style.setProperty('--rot', (Math.random()*360-180) + 'deg');
      h.style.left = x + 'px';
      h.style.top = y + 'px';
      h.style.width = (12 + Math.random()*12) + 'px';
      h.style.animationDelay = (Math.random()*0.15) + 's';
      burstLayer.appendChild(h);
      setTimeout(() => h.remove(), 2000);
    }
  }

  // a little heart burst on every tap/click anywhere on the page
  document.addEventListener('click', (e) => {
    heartBurstAt(e.clientX, e.clientY, 9);
  });

  btnYes.addEventListener('click', () => {
    finalMsg.textContent = 'sabía que dirías que sí';
    letter.classList.add('answered');
    heartBurst(40);
    setTimeout(() => heartBurst(30), 300);
    setTimeout(() => heartBurst(20), 650);
    setTimeout(() => {
      letter.classList.add('show-poem');
      heartBurst(16);
      poemCatLeft.classList.add('show');
      poemCatRight.classList.add('show');
    }, 2600);
  });

  btnKiss.addEventListener('click', () => {
    finalMsg.textContent = 'yo también te doy uno';
    letter.classList.add('answered');
    heartBurst(18);
    setTimeout(() => {
      letter.classList.add('show-poem-kiss');
    }, 1800);
  });

  const btnBack = document.getElementById('btnBack');
  btnBack.addEventListener('click', () => {
    letter.classList.remove('answered', 'show-poem-kiss');
    finalMsg.textContent = '';
  });
</script>

</body>
</html>

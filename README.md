<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Love Letter</title>
<link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@500;700&family=Playfair+Display:ital@1&family=Lato:wght@300;400&display=swap" rel="stylesheet">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html { overflow: hidden; height: 100%; }
body {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: linear-gradient(145deg, #ffe4f0, #ffd0e7, #ffbedd);
  overflow: hidden;
  user-select: none;
  font-family: 'Lato', sans-serif;
}

body::before {
  content: "";
  position: fixed;
  inset: 0;
  background-image: radial-gradient(circle, rgba(255,150,195,0.28) 1.5px, transparent 1.5px);
  background-size: 38px 38px;
  pointer-events: none;
}

/* ═══════════════════════════
   SCREEN 1 — ENVELOPE
═══════════════════════════ */
#envelope-screen {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  z-index: 1;
}
#envelope-screen.hidden { display: none; }

.env-box {
  position: relative;
  width: 360px;
  height: 230px;
  perspective: 900px;
  perspective-origin: 50% 0%;
}

.env-body {
  position: absolute;
  inset: 0;
  background: #f9a8c9;
  border-radius: 8px 8px 14px 14px;
  box-shadow: 0 20px 55px rgba(190,60,120,0.4), 0 6px 18px rgba(190,60,120,0.2);
}
.env-body::before {
  content: "";
  position: absolute;
  inset: 0;
  clip-path: polygon(0 0, 50% 52%, 0 100%);
  background: rgba(200,70,125,0.13);
  border-radius: 8px 0 0 14px;
}
.env-body::after {
  content: "";
  position: absolute;
  inset: 0;
  clip-path: polygon(100% 0, 50% 52%, 100% 100%);
  background: rgba(180,50,105,0.11);
  border-radius: 0 8px 14px 0;
}
.env-v {
  position: absolute;
  inset: 0;
  clip-path: polygon(50% 52%, 100% 100%, 0 100%);
  background: rgba(230,85,140,0.18);
  border-radius: 0 0 14px 14px;
  pointer-events: none;
}

.flap {
  position: absolute;
  inset: 0;
  transform-origin: top center;
  transform: rotateX(0deg);
  transition: transform 0.9s cubic-bezier(0.45, 0, 0.2, 1);
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
  pointer-events: none;
}
.flap-face {
  position: absolute;
  inset: 0;
  clip-path: polygon(0% 0%, 100% 0%, 50% 58%);
  background: #f088bc;
  border-radius: 8px 8px 0 0;
}
.flap-face::after {
  content: "";
  position: absolute;
  inset: 0;
  clip-path: polygon(0% 0%, 100% 0%, 50% 58%);
  background: linear-gradient(160deg, rgba(255,255,255,0.16), rgba(180,45,95,0.2));
}

/* WAX SEAL */
.seal {
  position: absolute;
  top: 105px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 10;
  cursor: pointer;
  transition: opacity 0.4s, transform 0.4s;
}
.seal-inner { transition: transform 0.2s ease; }
.seal:hover .seal-inner { transform: scale(1.08); }

.env-box.opening .flap { transform: rotateX(-178deg); }
.env-box.opening .seal {
  opacity: 0;
  transform: translateX(-50%) scale(0.1);
  pointer-events: none;
}

.env-hint {
  margin-top: 26px;
  font-size: 12px;
  letter-spacing: 0.09em;
  color: #c06080;
  font-weight: 300;
  opacity: 0.8;
}

/* ═══════════════════════════
   SCREEN 2 — LETTER
═══════════════════════════ */
#letter-screen {
  display: none;
  flex-direction: column;
  align-items: center;
  width: 100%;
  height: 100vh;
  padding: 20px 0 20px;
  z-index: 1;
  overflow: hidden;
  animation: letterAppear 0.55s cubic-bezier(0.34, 1.4, 0.64, 1) both;
}
#letter-screen.visible { display: flex; }

@keyframes letterAppear {
  from { opacity: 0; transform: translateY(30px) scale(0.96); }
  to   { opacity: 1; transform: translateY(0) scale(1); }
}

.letter-scroll {
  width: 340px;
  max-height: calc(100vh - 100px);
  overflow-y: auto;
  overflow-x: hidden;
  border-radius: 10px;
  scrollbar-width: thin;
  scrollbar-color: #f9a8c9 transparent;
}
.letter-scroll::-webkit-scrollbar { width: 5px; }
.letter-scroll::-webkit-scrollbar-thumb { background: #f9a8c9; border-radius: 10px; }
.letter-scroll::-webkit-scrollbar-track { background: transparent; }

.letter-paper {
  position: relative;
  width: 100%;
  background: #fefaef;
  border-radius: 10px;
  box-shadow: 0 16px 50px rgba(0,0,0,0.22);
  padding: 36px 28px 40px;
  background-image: repeating-linear-gradient(
    transparent, transparent 31px,
    #e8d4b5 31px, #e8d4b5 32px
  );
  background-position: 0 60px;
}

.tape {
  position: absolute;
  width: 66px;
  height: 21px;
  background: rgba(255, 176, 210, 0.78);
  border-radius: 3px;
  box-shadow: inset 0 1px 0 rgba(255,255,255,0.55);
}
.tape-tr { top: -9px;  right: 26px; transform: rotate(4deg); }
.tape-bl { bottom: -9px; left: 18px; transform: rotate(-3deg); }

.deco-tr { position: absolute; top: -24px; right: -2px; pointer-events: none; }
.deco-bl { position: absolute; bottom: -20px; left: -2px; pointer-events: none; }

.ltr-date {
  display: block;
  font-family: 'Lato', sans-serif;
  font-size: 10px;
  letter-spacing: 0.1em;
  color: #c090a8;
  font-weight: 300;
  margin-bottom: 12px;
}
.ltr-salut {
  display: block;
  font-family: 'Dancing Script', cursive;
  font-size: 22px;
  color: #7a3858;
  margin-bottom: 8px;
  line-height: 1.3;
}
.ltr-body {
  display: block;
  font-family: 'Playfair Display', serif;
  font-style: italic;
  color: #5c2a3a;
  font-size: 13px;
  line-height: 32px;
  white-space: pre-wrap;
}
.ltr-proposal {
  display: block;
  font-family: 'Dancing Script', cursive;
  font-size: 22px;
  color: #c2185b;
  font-weight: 700;
  margin-top: 10px;
  line-height: 1.5;
}
.ltr-sign {
  display: block;
  font-family: 'Dancing Script', cursive;
  font-size: 16px;
  color: #c090a8;
  margin-top: 4px;
}

.scroll-hint {
  font-size: 10px;
  color: #c090a8;
  letter-spacing: 0.07em;
  font-weight: 300;
  margin-top: 6px;
  opacity: 0.7;
  transition: opacity 0.4s;
  animation: bob 1.6s infinite;
}
@keyframes bob {
  0%,100% { transform: translateY(0); }
  50%      { transform: translateY(3px); }
}

.btn-back {
  margin-top: 14px;
  padding: 11px 32px;
  border: 1.5px solid #f06292;
  border-radius: 30px;
  background: transparent;
  color: #c2185b;
  font-family: 'Lato', sans-serif;
  font-size: 12px;
  letter-spacing: 0.1em;
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
  flex-shrink: 0;
}
.btn-back:hover { background: #f06292; color: white; }

.falling {
  position: fixed;
  top: -50px;
  pointer-events: none;
  z-index: 999;
  animation: fallDown linear forwards;
}
@keyframes fallDown {
  0%   { transform: translateY(0) rotate(0deg); opacity: 1; }
  85%  { opacity: 1; }
  100% { transform: translateY(110vh) rotate(480deg); opacity: 0; }
}

/* ═══════════════════════════════════════════════
   SCREEN 3 — RING BOX  (true CSS 3D, 3-step)
═══════════════════════════════════════════════ */
#ring-screen {
  display: none;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  gap: 0;
  z-index: 1;
  position: relative;
  width: 100%;
  height: 100vh;
  animation: letterAppear 0.6s cubic-bezier(0.34,1.4,0.64,1) both;
  background: radial-gradient(ellipse at 50% 45%, #1a0510 0%, #07010d 100%);
  overflow: hidden;
}
#ring-screen.visible { display: flex; }

#ring-screen::before {
  content:""; position:absolute; inset:0; pointer-events:none;
  background:
    radial-gradient(ellipse at 10% 20%, rgba(160,20,50,0.18) 0%, transparent 50%),
    radial-gradient(ellipse at 90% 80%, rgba(120,10,40,0.15) 0%, transparent 50%);
}

@keyframes fvFloat {
  0%,100% { transform:translateY(0px); }
  50%     { transform:translateY(-10px); }
}
@keyframes twinkle {
  0%,100%{ opacity:0.06; transform:scale(0.6); }
  50%    { opacity:0.9;  transform:scale(1.4); }
}
.sparkle-dot {
  position:absolute; border-radius:50%; background:white;
  animation:twinkle ease-in-out infinite; pointer-events:none;
}

.ring-screen-title {
  font-family:'Dancing Script',cursive; font-size:21px;
  color:#f0b8c8; letter-spacing:0.05em;
  text-shadow:0 0 16px rgba(240,100,140,0.4);
  margin-bottom:22px; position:relative; z-index:2;
  /* Hidden until box is opened */
  visibility: hidden;
  opacity: 0;
  transition: opacity 0.6s ease, visibility 0s 0.6s;
}
.ring-screen-title.visible {
  visibility: visible;
  opacity: 1;
  transition: opacity 0.6s ease;
}

/* ════════════════════════════════════════
   TRUE CSS 3D BOX
   ┌─────────────────────────────────────┐
   │  .box-scene                         │
   │    perspective: 600px               │
   │    perspective-origin: 50% -10%     │
   │                                     │
   │  .ring-box-wrap                     │
   │    transform-style: preserve-3d     │
   │                                     │
   │  BASE = 5 real face planes          │
   │    .b-front  translateZ(+d/2)       │
   │    .b-back   translateZ(-d/2)       │
   │    .b-left   rotateY(-90°) tz       │
   │    .b-right  rotateY(+90°) tz       │
   │    .b-bottom rotateX(-90°) tz       │
   │    (top open — velvet visible)      │
   │                                     │
   │  LID = 4 face planes                │
   │    pivot at back top edge           │
   │    rotateX(0°→-115°) on open        │
   │    .l-top  .l-front  .l-left .l-rt  │
   └─────────────────────────────────────┘
════════════════════════════════════════ */

/* ════════════════════════════════════════════
   RING BOX — 3-state reveal
   State 0 (default):  box closed, ring hidden inside
   State 1 (lid-open): lid swings back, ring visible sitting in box
   State 2 (ring-lifted): ring rises out of box on tap
════════════════════════════════════════════ */

.box-scene {
  perspective: 600px;
  perspective-origin: 50% -10%;
  position: relative;
  z-index: 2;
  filter: drop-shadow(0 40px 22px rgba(0,0,0,0.65));
}



.ring-box-wrap {
  position: relative;
  width: 180px;
  height: 110px;
  transform-style: preserve-3d;
  cursor: pointer;
  transform: translateZ(0px);
}

.b-face, .l-face {
  position: absolute;
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
}

.b-front {
  width: 180px; height: 110px;
  top: 0; left: 0;
  transform: translateZ(25px);
  background: #2a1010;
  box-shadow: inset 0 0 0 1px rgba(255,255,255,0.04);
}
.b-front::before {
  content:''; position:absolute; inset:0;
  background: repeating-linear-gradient(
    135deg, transparent, transparent 2px,
    rgba(255,255,255,0.012) 2px, rgba(255,255,255,0.012) 3px
  );
}
.b-front::after {
  content:''; position:absolute;
  top:0; left:0; right:0; height:2px;
  background: rgba(255,255,255,0.07);
}
.b-back {
  width: 180px; height: 110px;
  top: 0; left: 0;
  transform: translateZ(-25px) rotateY(180deg);
  background: #180808;
}
.b-left {
  width: 50px; height: 110px;
  top: 0; left: 0;
  transform: rotateY(-90deg) translateZ(-25px);
  background: #1a0808;
  box-shadow: inset -3px 0 8px rgba(0,0,0,0.4);
}
.b-right {
  width: 50px; height: 110px;
  top: 0; left: 130px;
  transform: rotateY(90deg) translateZ(25px);
  background: #130606;
  box-shadow: inset 3px 0 8px rgba(0,0,0,0.5);
}
.b-bottom {
  width: 180px; height: 50px;
  top: 60px; left: 0;
  transform: rotateX(-90deg) translateZ(-25px);
  background: #100404;
}

.b-interior {
  position: absolute;
  width: 164px; height: 38px;
  top: 8px; left: 8px;
  transform: translateZ(22px) rotateX(-90deg) translateY(-88px);
  background: #1c1a24;
  border-radius: 4px;
  box-shadow: inset 0 0 20px rgba(0,0,0,0.6);
  background-image:
    radial-gradient(ellipse at 50% 20%, rgba(80,75,110,0.4) 0%, transparent 60%),
    repeating-linear-gradient(45deg, transparent, transparent 2px,
      rgba(255,255,255,0.01) 2px, rgba(255,255,255,0.01) 3px);
  transition: box-shadow 0.7s 0.5s ease;
}

.ring-slot {
  position: absolute;
  bottom: 6px; left: 50%; transform: translateX(-50%);
  width: 70px; height: 12px; border-radius: 50%;
  background: #0f0d18;
  box-shadow: inset 0 3px 7px rgba(0,0,0,0.8);
}

.box-lid {
  position: absolute;
  top: -90px; left: 0;
  width: 180px; height: 90px;
  transform-style: preserve-3d;
  transform-origin: center bottom;
  transform: translateZ(-25px) rotateX(0deg);
  transition: transform 1.1s cubic-bezier(0.42, 0, 0.18, 1);
  z-index: 10;
}

.l-top {
  width: 180px; height: 50px;
  top: 0; left: 0;
  transform: rotateX(90deg) translateZ(50px);
  background: #301010;
  box-shadow: inset 0 0 0 1px rgba(255,255,255,0.04);
}
.l-top::before {
  content:''; position:absolute; inset:0;
  background: linear-gradient(135deg,
    rgba(255,255,255,0.07) 0%,
    rgba(255,255,255,0.02) 40%,
    transparent 70%);
}
.l-front {
  width: 180px; height: 90px;
  top: 0; left: 0;
  transform: translateZ(25px);
  background: #351212;
  box-shadow:
    inset 0 1px 0 rgba(255,255,255,0.06),
    inset 0 -1px 0 rgba(0,0,0,0.3);
}
.l-front::before {
  content:''; position:absolute; inset:0;
  background: repeating-linear-gradient(
    135deg, transparent, transparent 2px,
    rgba(255,255,255,0.01) 2px, rgba(255,255,255,0.01) 3px
  );
}
.l-front::after {
  content:''; position:absolute;
  bottom:0; left:0; right:0; height:3px;
  background: linear-gradient(90deg, #200808, #6a2020, #200808);
}
.l-left {
  width: 50px; height: 90px;
  top: 0; left: 0;
  transform: rotateY(-90deg) translateZ(-25px);
  background: #280e0e;
}
.l-right {
  width: 50px; height: 90px;
  top: 0; left: 130px;
  transform: rotateY(90deg) translateZ(25px);
  background: #1e0a0a;
}
.l-inner {
  width: 164px; height: 74px;
  top: 8px; left: 8px;
  transform: translateZ(24px);
  background: #1e1c28;
  border-radius: 4px 4px 0 0;
  background-image: repeating-linear-gradient(
    45deg, transparent, transparent 2px,
    rgba(255,255,255,0.01) 2px, rgba(255,255,255,0.01) 3px
  );
  pointer-events: none;
}

.lid-hinge {
  position: absolute;
  bottom: -5px; left: 50%;
  transform: translateX(-50%) translateZ(-24px);
  width: 50px; height: 10px;
  background: linear-gradient(90deg,
    #150505, #3a1010, #622020, #883030, #622020, #3a1010, #150505);
  border-radius: 5px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.7);
  z-index: 15;
}

.ring-box-wrap.lid-open .box-lid {
  transform: translateZ(-25px) rotateX(-110deg);
}
.ring-box-wrap.lid-open .b-interior {
  box-shadow:
    inset 0 0 20px rgba(0,0,0,0.5),
    0 0 40px 10px rgba(255,245,220,0.07);
}
/* Box fades and shrinks away after opening */
.ring-box-wrap {
  transition: opacity 0.8s ease, transform 0.8s ease;
}
.ring-box-wrap.box-hidden {
  opacity: 0;
  transform: scale(0.85) translateY(10px);
  pointer-events: none;
}

.ring-display {
  position: absolute;
  left: 50%;
  top: 60px;
  transform: translateX(-50%) translateY(60px) scale(0.9);
  z-index: 20;
  cursor: default;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.7s 0.5s ease, transform 0.9s 0.4s cubic-bezier(0.34, 1.15, 0.64, 1);
}
/* Box opened → ring rises into view */
.ring-box-wrap.lid-open ~ .ring-display {
  opacity: 1;
  transform: translateX(-50%) translateY(-40px) scale(1);
  cursor: pointer;
  pointer-events: auto;
  transition: opacity 0.7s 0.5s ease, transform 0.9s 0.4s cubic-bezier(0.34, 1.15, 0.64, 1);
}
/* Ring zoomed in */
.ring-box-wrap.lid-open ~ .ring-display.ring-zoomed {
  transform: translateX(-50%) translateY(-70px) scale(2.2);
  cursor: zoom-out;
  transition: transform 0.5s cubic-bezier(0.34, 1.1, 0.64, 1);
}

/* ── Hint text ── */
.ring-box-hint {
  font-size:11px; letter-spacing:0.1em; color:#f5c0cc;
  font-weight:300; font-family:'Lato',sans-serif; opacity:0.65;
  margin-top:12px; position:relative; z-index:2; transition:opacity 0.3s;
}

/* ── Proposal + buttons ── */
.proposal-text {
  font-family:'Dancing Script',cursive; font-size:28px;
  color:#fbd5de; font-weight:700; text-align:center;
  text-shadow:0 0 28px rgba(255,140,170,0.7),0 2px 8px rgba(0,0,0,0.6);
  opacity:0; transform:translateY(8px);
  transition:opacity 0.7s, transform 0.7s;
  margin-top:8px; position:relative; z-index:2;
}
.proposal-text.show { opacity:1; transform:translateY(0); }

.answer-btns {
  display:flex; gap:14px; margin-top:10px;
  opacity:0; transform:translateY(8px);
  transition:opacity 0.6s, transform 0.6s; pointer-events:none;
  position:relative; z-index:2;
}
.answer-btns.show { opacity:1; transform:translateY(0); pointer-events:auto; }

.btn-yes,.btn-no {
  padding:11px 32px; border-radius:30px; font-family:'Lato',sans-serif;
  font-size:12.5px; letter-spacing:0.12em; cursor:pointer;
  font-weight:400; transition:transform 0.16s, box-shadow 0.16s;
}
.btn-yes {
  background:linear-gradient(135deg,#f06292,#c2185b); color:white; border:none;
  box-shadow:0 5px 22px rgba(194,24,91,0.55);
}
.btn-yes:hover { transform:scale(1.06); box-shadow:0 8px 28px rgba(194,24,91,0.7); }
.btn-no {
  background:transparent; color:#f5c0cc; border:1.5px solid rgba(245,192,204,0.45);
}
.btn-no:hover { background:rgba(245,192,204,0.08); }

.btn-ring-back {
  font-size:10.5px; letter-spacing:0.09em; color:rgba(245,192,204,0.4);
  font-family:'Lato',sans-serif; font-weight:300; background:none; border:none;
  cursor:pointer; transition:color 0.2s; margin-top:6px; position:relative; z-index:2;
}
.btn-ring-back:hover { color:rgba(245,192,204,0.85); }

/* ════════════════════════════════
   MINI LETTER — peeks below box,
   expands to fullscreen overlay
════════════════════════════════ */
.mini-letter-peek {
  position:relative; z-index:3;
  margin-top:10px;
  display:flex; flex-direction:column; align-items:center;
  cursor:pointer;
  visibility: hidden;
  opacity: 0;
  pointer-events: none;
  transform: translateY(10px);
  transition: opacity 0.6s ease, transform 0.7s cubic-bezier(0.34,1.2,0.64,1);
}
.mini-letter-peek.visible {
  visibility: visible;
  opacity: 1;
  pointer-events: auto;
  transform: translateY(0);
}
.mini-peek-tab {
  width:140px; height:30px;
  background:linear-gradient(180deg,#fef6e4,#fef0d0);
  border-radius:0 0 12px 12px;
  box-shadow:0 6px 18px rgba(0,0,0,0.45), inset 0 -1px 0 rgba(200,160,80,0.3);
  display:flex; align-items:center; justify-content:center;
  padding: 0 12px;
  gap: 6px;
  transition:transform 0.2s, box-shadow 0.2s;
  background-image:linear-gradient(180deg,#fef6e4,#fef0d0),
    repeating-linear-gradient(transparent,transparent 9px,#e8d4a8 9px,#e8d4a8 10px);
}
.mini-peek-tab:hover { transform:translateY(3px); box-shadow:0 8px 22px rgba(0,0,0,0.5); }
.mini-peek-label {
  font-family:'Dancing Script',cursive; font-size:12px;
  color:#a07040; letter-spacing:0.04em;
}
.mini-peek-env {
  width:18px; height:13px; position:relative; flex-shrink:0;
}
.mini-peek-env::before {
  content:""; position:absolute; inset:0;
  background:#f9a8c9; border-radius:2px;
}
.mini-peek-env::after {
  content:""; position:absolute; top:0; left:0; right:0;
  height:7px; clip-path:polygon(0 0,100% 0,50% 60%);
  background:#f088bc;
}

/* FULL OVERLAY when open */
.mini-letter-overlay {
  position:fixed; inset:0; z-index:50;
  display:flex; align-items:center; justify-content:center;
  background:rgba(10,2,10,0.82);
  opacity:0; pointer-events:none;
  transition:opacity 0.4s;
  backdrop-filter:blur(4px);
}
.mini-letter-overlay.open { opacity:1; pointer-events:auto; }

.mini-letter-modal {
  position:relative;
  width:min(340px,88vw);
  background:#fef8e8;
  border-radius:12px;
  box-shadow:0 30px 80px rgba(0,0,0,0.7), 0 0 0 1px rgba(200,160,80,0.2);
  padding:32px 28px 28px;
  transform:translateY(24px) scale(0.95);
  transition:transform 0.4s cubic-bezier(0.34,1.4,0.64,1);
  /* lined paper */
  background-image:repeating-linear-gradient(
    transparent,transparent 27px,#e0c890 27px,#e0c890 28px);
  background-position:0 48px;
  overflow:hidden;
}
.mini-letter-overlay.open .mini-letter-modal {
  transform:translateY(0) scale(1);
}
/* decorative tape strip */
.modal-tape {
  position:absolute; top:-6px; left:50%; transform:translateX(-50%) rotate(-1deg);
  width:70px; height:18px;
  background:rgba(255,180,210,0.75);
  border-radius:3px;
  box-shadow:inset 0 1px 0 rgba(255,255,255,0.6);
}
/* pink corner rose */
.modal-rose {
  position:absolute; top:12px; right:14px; font-size:20px; opacity:0.55;
}
.modal-salut {
  display:block; font-family:'Dancing Script',cursive;
  font-size:18px; color:#7a3858; margin-bottom:10px; margin-top:6px;
}
.modal-textarea {
  width:100%; font-family:'Playfair Display',serif; font-style:italic;
  color:#4a2030; font-size:13px; line-height:28px;
  background:transparent; border:none; outline:none;
  resize:none; min-height:150px;
}
.modal-close-hint {
  font-size:10px; color:#c090a8; font-family:'Lato',sans-serif;
  font-weight:300; letter-spacing:0.09em; text-align:center;
  margin-top:10px; opacity:0.7; display:block;
  cursor:pointer;
}
.modal-close-hint:hover { opacity:1; }

/* ═══════════════════════════
   SCREEN 4a — YES
   SCREEN 4b — NO
═══════════════════════════ */
#yes-screen, #no-screen {
  display: none;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 18px;
  z-index: 1;
  animation: letterAppear 0.5s ease both;
  padding: 20px;
  text-align: center;
}
#yes-screen.visible { display: flex; }
#no-screen.visible  { display: flex; }

.response-msg {
  font-family: 'Dancing Script', cursive;
  font-size: 32px;
  color: #c2185b;
  font-weight: 700;
  text-align: center;
  line-height: 1.3;
}
.response-sub {
  font-family: 'Playfair Display', serif;
  font-style: italic;
  font-size: 14px;
  color: #7a3858;
  text-align: center;
  max-width: 280px;
  line-height: 1.8;
}
.btn-restart {
  margin-top: 10px;
  padding: 11px 30px;
  border: 1.5px solid #f06292;
  border-radius: 30px;
  background: transparent;
  color: #c2185b;
  font-family: 'Lato', sans-serif;
  font-size: 12px;
  letter-spacing: 0.1em;
  cursor: pointer;
  transition: background 0.2s, color 0.2s;
}
.btn-restart:hover { background: #f06292; color: white; }

/* ── Celebration confetti pieces (SVG-based) ── */
.confetti-piece {
  position: fixed;
  top: -40px;
  pointer-events: none;
  z-index: 999;
  animation: confettiFall linear forwards;
}
@keyframes confettiFall {
  0%   { transform: translateY(0) rotate(0deg) scale(1); opacity: 1; }
  80%  { opacity: 1; }
  100% { transform: translateY(110vh) rotate(720deg) scale(0.6); opacity: 0; }
}

/* ── Rain drops ── */
.raindrop {
  position: fixed;
  top: -30px;
  pointer-events: none;
  z-index: 999;
  animation: rainFall linear forwards;
}
@keyframes rainFall {
  0%   { transform: translateY(0) scaleY(1); opacity: 0.8; }
  100% { transform: translateY(110vh) scaleY(1.2); opacity: 0; }
}
</style>
</head>
<body>

<!-- ═══ SCREEN 1: ENVELOPE ═══ -->
<div id="envelope-screen">
  <div class="env-box" id="envBox">

    <div class="env-body"><div class="env-v"></div></div>
    <div class="flap"><div class="flap-face"></div></div>

    <!-- WAX SEAL -->
    <div class="seal" id="seal">
      <div class="seal-inner">
        <svg width="76" height="76" viewBox="0 0 80 80" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <radialGradient id="wax" cx="36%" cy="30%" r="62%">
              <stop offset="0%"   stop-color="#e06078"/>
              <stop offset="40%"  stop-color="#b82048"/>
              <stop offset="100%" stop-color="#720820"/>
            </radialGradient>
            <radialGradient id="sheen" cx="34%" cy="26%" r="52%">
              <stop offset="0%"   stop-color="rgba(255,255,255,0.42)"/>
              <stop offset="100%" stop-color="rgba(255,255,255,0)"/>
            </radialGradient>
            <filter id="waxShadow">
              <feDropShadow dx="0" dy="3" stdDeviation="4" flood-color="rgba(100,5,20,0.6)"/>
            </filter>
          </defs>
          <!-- wax drips -->
          <ellipse cx="40" cy="7"  rx="7.5" ry="5"   fill="#901530"/>
          <ellipse cx="64" cy="16" rx="5"   ry="7.5" fill="#901530"/>
          <ellipse cx="74" cy="40" rx="5"   ry="6.5" fill="#881028"/>
          <ellipse cx="65" cy="65" rx="6.5" ry="5"   fill="#901530"/>
          <ellipse cx="40" cy="74" rx="7.5" ry="5"   fill="#881028"/>
          <ellipse cx="16" cy="65" rx="6.5" ry="5"   fill="#901530"/>
          <ellipse cx="6"  cy="40" rx="5"   ry="6.5" fill="#881028"/>
          <ellipse cx="16" cy="16" rx="5"   ry="7.5" fill="#901530"/>
          <ellipse cx="53" cy="5"  rx="4"   ry="3.5" fill="#a01838"/>
          <ellipse cx="27" cy="6"  rx="4"   ry="3.5" fill="#a01838"/>
          <ellipse cx="73" cy="27" rx="3.5" ry="4"   fill="#a01838"/>
          <ellipse cx="74" cy="55" rx="3.5" ry="4"   fill="#a01838"/>
          <ellipse cx="6"  cy="55" rx="3.5" ry="4"   fill="#a01838"/>
          <ellipse cx="7"  cy="27" rx="3.5" ry="4"   fill="#a01838"/>
          <!-- main disc -->
          <circle cx="40" cy="40" r="30" fill="url(#wax)" filter="url(#waxShadow)"/>
          <!-- embossed rings -->
          <circle cx="40" cy="40" r="26.5" fill="none" stroke="rgba(255,255,255,0.2)"  stroke-width="1.5"/>
          <circle cx="40" cy="40" r="23.5" fill="none" stroke="rgba(0,0,0,0.18)" stroke-width="0.8"/>
          <!-- embossed heart -->
          <g transform="translate(40,41) scale(0.95)">
            <path d="M0 13C0 13 -12 6 -12 -1C-12 -6 -8.5 -9 -5 -9C-2.5 -9 0 -6.5 0 -4.5C0 -6.5 2.5 -9 5 -9C8.5 -9 12 -6 12 -1C12 6 0 13 0 13Z"
                  fill="rgba(0,0,0,0.25)" transform="translate(0.5,1.5)"/>
            <path d="M0 13C0 13 -12 6 -12 -1C-12 -6 -8.5 -9 -5 -9C-2.5 -9 0 -6.5 0 -4.5C0 -6.5 2.5 -9 5 -9C8.5 -9 12 -6 12 -1C12 6 0 13 0 13Z"
                  fill="#e06078"/>
            <path d="M0 13C0 13 -12 6 -12 -1C-12 -6 -8.5 -9 -5 -9C-2.5 -9 0 -6.5 0 -4.5C0 -6.5 2.5 -9 5 -9C8.5 -9 12 -6 12 -1C12 6 0 13 0 13Z"
                  fill="url(#sheen)"/>
          </g>
          <!-- surface sheen -->
          <ellipse cx="31" cy="27" rx="13" ry="7.5" fill="rgba(255,255,255,0.13)" transform="rotate(-28 31 27)"/>
        </svg>
      </div>
    </div>

  </div>
  <p class="env-hint">Click the wax seal to open ♡</p>
</div>

<!-- ═══ SCREEN 2: LETTER ═══ -->
<div id="letter-screen">

  <div class="letter-scroll" id="letterScroll">
    <div class="letter-paper">
      <div class="tape tape-tr"></div>
      <div class="tape tape-bl"></div>

      <!-- top-right flower -->
      <svg class="deco-tr" width="48" height="48" viewBox="0 0 48 48">
        <g transform="translate(24,24)">
          <ellipse cx="0" cy="-11" rx="5.5" ry="10" fill="#ffb3d4"/>
          <ellipse cx="0" cy="-11" rx="5.5" ry="10" fill="#ffc8e0" transform="rotate(72)"/>
          <ellipse cx="0" cy="-11" rx="5.5" ry="10" fill="#ffb3d4" transform="rotate(144)"/>
          <ellipse cx="0" cy="-11" rx="5.5" ry="10" fill="#ffc8e0" transform="rotate(216)"/>
          <ellipse cx="0" cy="-11" rx="5.5" ry="10" fill="#ffb3d4" transform="rotate(288)"/>
          <circle cx="0" cy="0" r="6.5" fill="#ff75ad"/>
          <circle cx="0" cy="0" r="3"   fill="#ffd6ea"/>
        </g>
      </svg>

      <!-- bottom-left flower -->
      <svg class="deco-bl" width="36" height="36" viewBox="0 0 36 36">
        <g transform="translate(18,18)">
          <ellipse cx="0" cy="-8" rx="4" ry="7.5" fill="#ffc8e0"/>
          <ellipse cx="0" cy="-8" rx="4" ry="7.5" fill="#ffb3d4" transform="rotate(72)"/>
          <ellipse cx="0" cy="-8" rx="4" ry="7.5" fill="#ffc8e0" transform="rotate(144)"/>
          <ellipse cx="0" cy="-8" rx="4" ry="7.5" fill="#ffb3d4" transform="rotate(216)"/>
          <ellipse cx="0" cy="-8" rx="4" ry="7.5" fill="#ffc8e0" transform="rotate(288)"/>
          <circle cx="0" cy="0" r="5"   fill="#ff75ad"/>
          <circle cx="0" cy="0" r="2.5" fill="#ffd6ea"/>
        </g>
      </svg>

      <span class="ltr-date">February 14, 2026</span>
      <span class="ltr-salut">My Dearest Love,</span>
      <span class="ltr-body">
I don't know where to begin,
because every time I try to put
into words, kung gaano kita kamahal,
It always feels like it's not enough.

But I'll try anyway — because
you deserve every single attempt.

We’ve been through so much together already —
years of knowing each other,
distance, moments of doubt, moments of joy,
the times when we felt fragile, and the times we weren't sure how to reach each other,
across cities, across screens, across everything in between.
There were times when things felt easy, and also times when everything felt complicated.        

And yet… andito tayo.
Pinipili parin ang isa't isa. Still trying. Still staying. Still loving.
And somehow, that makes all the chaos feel worth it. 
Kase at the end of the day, we never let go.        
                
And after all these years, after all the ups and downs,
I realized something simple and clear —
I don’t want a life without you.
Not just emotionally, but realistically. When I think about my future, naa jud ka.        
Every laugh, every quiet moment, every random questions, every chaotic little things we do
turns the ordinary into something unforgettable.

The ring I gave you isn’t just metal or stone.
It’s a piece of me, a piece of my heart and soul,
a token of every thought, every hope, every assurance I carry for you,
a reflection of how serious I am about us — not just in feelings, but in intention.        
a reminder that no matter the distance, no matter the obstacles,
I am yours, and I will continue to be yours, with the same certainty I feel when I say your name.
Not just when things are smooth, but even when we're figuring things out.       

I’ve watched you be brave in ways you didn’t notice,
gentle in ways no one deserved,
real in ways I didn’t even know was possible.
And every single time, I’ve fallen deeper.
And that feeling didn't come from perfection — it came from seeing you as a whole person.        
And even on the hard days, my heart keeps finding its way back to you.
Kahit may misunderstandings, kahit may space, and kahit minsan nag pu-pull back ka, ikaw pa rin talaga ang hinahanp ko.         
You are my favourite part of every day.
The first thought when I wake up,
the last one before I sleep.
The only person I want to share everything with —
the good, the bad, the silly, the messy, the quiet, the loud.
Everything is just better because you're in it.        

I don’t want to imagine a life that doesn’t have you in it.
Not for a second, not now, not ever.
Because loving you stopped being a question a long time ago.
        
So here, in this letter, sealed
and folded and held — I want to
say something I've been
carrying in my chest for a long,
long time.
Something I hope will reach your heart even from afar.
Na kahit malayo ako, sana malinaw sayo kung gaano ako kasigurado.        

I want you to know that even across the distance,
even through all the challenges and the messy moments,
my heart is yours.
and it's not something na I just suddenly realize or something, it's something I thought about a million times,
and still landed in the same answer.        
I am loving you now,
and I will keep loving you every day,
in every way I can,
for every moment we’re given —
this lifetime and the next.     
                
I love you.

Fully. Completely. Without
condition or reservation.

And I want to keep loving you
for every second we're given. 
And I will, babi, as long as I exist.
Not because I have to — but because, even after everything we've been through, 
you're still the one I choose.
      </span>
      <span class="ltr-proposal">Will you marry me? ♡</span>
      <span class="ltr-sign">— Yours, always &amp; forever.</span>
    </div>
  </div>

  <p class="scroll-hint" id="scrollHint">↓ scroll to read more</p>
  <div style="display:flex;gap:12px;flex-wrap:wrap;justify-content:center;">
    <button class="btn-back" id="btnBack">↩ Put back in envelope</button>
    <button class="btn-back" id="btnRevealRing" style="border-color:#c2185b;color:#c2185b;">✦ There's something else...</button>
  </div>
</div>

<!-- ═══ SCREEN 3: RING BOX (3-step reveal) ═══ -->
<div id="ring-screen">

  <p class="ring-screen-title">One more thing, Babi…</p>

  <div class="box-scene">
    <div class="ring-box-wrap" id="ringBox">

      <!-- ── LID ── -->
      <div class="box-lid" id="boxLid">
        <div class="l-face l-front"></div>
        <div class="l-face l-top"></div>
        <div class="l-face l-left"></div>
        <div class="l-face l-right"></div>
        <div class="l-face l-inner"></div>
        <div class="lid-hinge"></div>
      </div>

      <!-- ── BASE faces ── -->
      <div class="b-face b-front"></div>
      <div class="b-face b-back"></div>
      <div class="b-face b-left"></div>
      <div class="b-face b-right"></div>
      <div class="b-face b-bottom"></div>

      <!-- ── INTERIOR + ring ── -->
      <div class="b-interior">
        <div class="ring-slot"></div>
      </div>


    </div><!-- /ring-box-wrap -->


      <div class="ring-display" id="ringDisplay">
<!-- Mikana ring — clean accurate replica -->
            <svg id="ringInBox" width="90" height="95" viewBox="0 0 200 210"
                 xmlns="http://www.w3.org/2000/svg"
                 style="cursor:pointer;filter:drop-shadow(0 4px 16px rgba(255,200,40,0.52))">
              <defs>
                <linearGradient id="sAuV" x1="0%" y1="0%" x2="0%" y2="100%">
                  <stop offset="0%"   stop-color="#fff8c0"/>
                  <stop offset="35%"  stop-color="#ffd700"/>
                  <stop offset="70%"  stop-color="#c89000"/>
                  <stop offset="100%" stop-color="#a87000"/>
                </linearGradient>
                <linearGradient id="sAuH" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%"   stop-color="#fff5a0"/>
                  <stop offset="45%"  stop-color="#ffd700"/>
                  <stop offset="100%" stop-color="#c09000"/>
                </linearGradient>
                <radialGradient id="sDia" cx="30%" cy="22%" r="68%">
                  <stop offset="0%"   stop-color="#ffffff"/>
                  <stop offset="18%"  stop-color="#f0f8ff"/>
                  <stop offset="48%"  stop-color="#b8d4ff"/>
                  <stop offset="80%"  stop-color="#7098d8"/>
                  <stop offset="100%" stop-color="#4060b0"/>
                </radialGradient>
                <radialGradient id="sAcc" cx="35%" cy="28%" r="65%">
                  <stop offset="0%"  stop-color="#ffffff"/>
                  <stop offset="50%" stop-color="#d0e4ff"/>
                  <stop offset="100%" stop-color="#6888c8"/>
                </radialGradient>
                <filter id="sDg" x="-40%" y="-40%" width="180%" height="180%">
                  <feGaussianBlur in="SourceAlpha" stdDeviation="2.5" result="b"/>
                  <feColorMatrix in="b" type="matrix"
                    values="0 0 0 0 .3 0 0 0 0 .55 0 0 0 0 1 0 0 0 .88 0" result="g"/>
                  <feMerge><feMergeNode in="g"/><feMergeNode in="SourceGraphic"/></feMerge>
                </filter>
                <clipPath id="sBc">
                  <rect x="0" y="93" width="200" height="120"/>
                </clipPath>
              </defs>

              <!-- BAND: perfect circle, clipped behind stone -->
              <circle cx="100" cy="160" r="63" fill="none"
                      stroke="url(#sAuV)" stroke-width="9" clip-path="url(#sBc)"/>
              <circle cx="100" cy="160" r="63" fill="none"
                      stroke="rgba(80,50,0,0.28)" stroke-width="3.5" clip-path="url(#sBc)"/>
              <circle cx="100" cy="160" r="63" fill="none"
                      stroke="rgba(255,250,185,0.7)" stroke-width="2.5"
                      stroke-dasharray="55 345" stroke-dashoffset="-108" clip-path="url(#sBc)"/>

              <!-- SHOULDER RAMPS — band widens toward center stone -->
              <path d="M 44 148 C 40 138,42 122,52 112 C 58 106,66 102,76 100
                       L 78 108 C 70 110,63 114,58 119 C 50 127,49 140,52 150 Z"
                    fill="url(#sAuV)"/>
              <path d="M 156 148 C 160 138,158 122,148 112 C 142 106,134 102,124 100
                       L 122 108 C 130 110,137 114,142 119 C 150 127,151 140,148 150 Z"
                    fill="url(#sAuV)"/>

              <!-- ACCENT STONES LEFT — L1 biggest, closest to center -->
              <circle cx="68" cy="113" r="7"   fill="url(#sAcc)" filter="url(#sDg)"/>
              <circle cx="68" cy="113" r="7"   fill="none" stroke="url(#sAuH)" stroke-width="1"/>
              <circle cx="66" cy="111" r="2.4" fill="rgba(255,255,255,0.95)"/>
              <circle cx="68" cy="106.2" r="1.6" fill="url(#sAuH)"/>
              <circle cx="68" cy="119.8" r="1.6" fill="url(#sAuH)"/>
              <circle cx="61.2" cy="113"  r="1.6" fill="url(#sAuH)"/>
              <circle cx="74.8" cy="113"  r="1.6" fill="url(#sAuH)"/>

              <circle cx="55" cy="129" r="5.5" fill="url(#sAcc)" filter="url(#sDg)"/>
              <circle cx="55" cy="129" r="5.5" fill="none" stroke="url(#sAuH)" stroke-width="0.9"/>
              <circle cx="53.2" cy="127.2" r="1.85" fill="rgba(255,255,255,0.93)"/>
              <circle cx="55" cy="123.7" r="1.3" fill="url(#sAuH)"/>
              <circle cx="55" cy="134.3" r="1.3" fill="url(#sAuH)"/>
              <circle cx="49.7" cy="129"  r="1.3" fill="url(#sAuH)"/>
              <circle cx="60.3" cy="129"  r="1.3" fill="url(#sAuH)"/>

              <circle cx="46" cy="145" r="4"   fill="url(#sAcc)" filter="url(#sDg)"/>
              <circle cx="46" cy="145" r="4"   fill="none" stroke="url(#sAuH)" stroke-width="0.8"/>
              <circle cx="44.5" cy="143.5" r="1.3" fill="rgba(255,255,255,0.92)"/>
              <circle cx="46" cy="141.1" r="1.1" fill="url(#sAuH)"/>
              <circle cx="46" cy="148.9" r="1.1" fill="url(#sAuH)"/>
              <circle cx="42.1" cy="145"  r="1.1" fill="url(#sAuH)"/>
              <circle cx="49.9" cy="145"  r="1.1" fill="url(#sAuH)"/>

              <!-- ACCENT STONES RIGHT — mirror -->
              <circle cx="132" cy="113" r="7"   fill="url(#sAcc)" filter="url(#sDg)"/>
              <circle cx="132" cy="113" r="7"   fill="none" stroke="url(#sAuH)" stroke-width="1"/>
              <circle cx="130" cy="111" r="2.4" fill="rgba(255,255,255,0.95)"/>
              <circle cx="132" cy="106.2" r="1.6" fill="url(#sAuH)"/>
              <circle cx="132" cy="119.8" r="1.6" fill="url(#sAuH)"/>
              <circle cx="125.2" cy="113"  r="1.6" fill="url(#sAuH)"/>
              <circle cx="138.8" cy="113"  r="1.6" fill="url(#sAuH)"/>

              <circle cx="145" cy="129" r="5.5" fill="url(#sAcc)" filter="url(#sDg)"/>
              <circle cx="145" cy="129" r="5.5" fill="none" stroke="url(#sAuH)" stroke-width="0.9"/>
              <circle cx="143.2" cy="127.2" r="1.85" fill="rgba(255,255,255,0.93)"/>
              <circle cx="145" cy="123.7" r="1.3" fill="url(#sAuH)"/>
              <circle cx="145" cy="134.3" r="1.3" fill="url(#sAuH)"/>
              <circle cx="139.7" cy="129"  r="1.3" fill="url(#sAuH)"/>
              <circle cx="150.3" cy="129"  r="1.3" fill="url(#sAuH)"/>

              <circle cx="154" cy="145" r="4"   fill="url(#sAcc)" filter="url(#sDg)"/>
              <circle cx="154" cy="145" r="4"   fill="none" stroke="url(#sAuH)" stroke-width="0.8"/>
              <circle cx="152.5" cy="143.5" r="1.3" fill="rgba(255,255,255,0.92)"/>
              <circle cx="154" cy="141.1" r="1.1" fill="url(#sAuH)"/>
              <circle cx="154" cy="148.9" r="1.1" fill="url(#sAuH)"/>
              <circle cx="150.1" cy="145"  r="1.1" fill="url(#sAuH)"/>
              <circle cx="157.9" cy="145"  r="1.1" fill="url(#sAuH)"/>

              <!-- SETTING BASE — solid gold bridge from band top up to stone base -->
              <!-- Fills the gap between band top (y≈97) and stone bottom (y≈80) -->
              <path d="M 82 100 C 84 95, 88 88, 86 82
                       C 90 78, 95 76, 100 76
                       C 105 76, 110 78, 114 82
                       C 112 88, 116 95, 118 100
                       C 112 97, 106 95, 100 95
                       C 94 95, 88 97, 82 100 Z"
                    fill="url(#sAuH)"/>
              <!-- Setting base inner shadow for depth -->
              <path d="M 86 99 C 88 94, 91 89, 90 84 C 93 81, 96 79, 100 79
                       C 104 79, 107 81, 110 84 C 109 89, 112 94, 114 99
                       C 109 96, 105 94, 100 94 C 95 94, 91 96, 86 99 Z"
                    fill="rgba(160,100,0,0.22)"/>

              <!-- 4 PRONG BALLS — sit on top of setting base, gripping stone -->
              <circle cx="86"  cy="82" r="5.2" fill="url(#sAuH)"/>
              <circle cx="114" cy="82" r="5.2" fill="url(#sAuH)"/>
              <circle cx="91"  cy="75" r="4.2" fill="url(#sAuH)" opacity="0.85"/>
              <circle cx="109" cy="75" r="4.2" fill="url(#sAuH)" opacity="0.85"/>
              <circle cx="84"  cy="80" r="1.6" fill="rgba(255,252,190,0.88)"/>
              <circle cx="112" cy="80" r="1.6" fill="rgba(255,252,190,0.88)"/>

              <!-- CENTER DIAMOND r=28 -->
              <circle cx="100" cy="52" r="28" fill="url(#sDia)" filter="url(#sDg)"/>
              <circle cx="100" cy="52" r="28" fill="none" stroke="rgba(150,190,245,0.22)" stroke-width="0.8"/>
              <polygon points="100,27 114,33 118,52 114,71 100,77 86,71 82,52 86,33"
                       fill="rgba(255,255,255,0.12)" stroke="rgba(142,180,255,0.52)" stroke-width="0.75"/>
              <line x1="100" y1="27" x2="100" y2="77" stroke="rgba(150,192,255,0.17)" stroke-width="0.55"/>
              <line x1="82"  y1="52" x2="118" y2="52" stroke="rgba(150,192,255,0.17)" stroke-width="0.55"/>
              <line x1="86"  y1="33" x2="114" y2="71" stroke="rgba(150,192,255,0.13)" stroke-width="0.5"/>
              <line x1="114" y1="33" x2="86"  y2="71" stroke="rgba(150,192,255,0.13)" stroke-width="0.5"/>
              <line x1="100" y1="27" x2="118" y2="52" stroke="rgba(142,180,255,0.2)"  stroke-width="0.5"/>
              <line x1="100" y1="27" x2="82"  y2="52" stroke="rgba(142,180,255,0.2)"  stroke-width="0.5"/>
              <ellipse cx="86" cy="33" rx="12" ry="7" fill="rgba(255,255,255,0.94)" transform="rotate(-32 86 33)"/>
              <circle  cx="83" cy="30" r="5.5" fill="white"/>
              <circle  cx="80" cy="27" r="2.8" fill="rgba(255,255,255,0.8)"/>
              <circle  cx="114" cy="70" r="3.5" fill="rgba(255,255,255,0.6)"/>
              <ellipse cx="113" cy="34" rx="8"   ry="3.2" fill="rgba(255,178,212,0.44)" transform="rotate(-20 113 34)"/>
              <ellipse cx="88"  cy="70" rx="6.5" ry="2.7" fill="rgba(178,212,255,0.42)" transform="rotate(-14 88 70)"/>
              <circle  cx="107" cy="36" r="2.2" fill="rgba(255,255,255,0.7)"/>
            </svg>
                    </div><!-- /ring-display -->

  </div><!-- /box-scene -->

  <!-- NOTE — appears after box open -->
  <div class="mini-letter-peek" id="miniPeek">
    <div class="mini-peek-tab">
      <span class="mini-peek-env"></span>
      <span class="mini-peek-label">one last thing, Babi ♡</span>
    </div>
  </div>

  <p class="ring-box-hint" id="ringBoxHint">Tap the box to open</p>
  <p class="proposal-text" id="proposalTxt">Will you marry me?</p>
  <div class="answer-btns" id="answerBtns">
    <button class="btn-yes" id="btnYes">Yes, always</button>
    <button class="btn-no"  id="btnNo">Not yet…</button>
  </div>
  <button class="btn-ring-back" id="btnRingBack">← back to letter</button>

</div>


<!-- ═══ RING FULLSCREEN OVERLAY ═══ -->
<div id="ringFullview" style="
  position:fixed;inset:0;z-index:80;
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  background:radial-gradient(ellipse at 50% 50%,#1a0825 0%,#05010d 100%);
  opacity:0;pointer-events:none;transition:opacity 0.45s;
">
  <p style="font-family:'Dancing Script',cursive;font-size:20px;color:rgba(245,192,210,0.75);
             letter-spacing:0.14em;margin-bottom:28px;text-align:center;">✦ &nbsp; your ring &nbsp; ✦</p>

  <!-- MIKANA RING — large fullscreen, accurate replica -->
  <svg id="ringFvSvg" width="300" height="310" viewBox="0 0 300 310"
       xmlns="http://www.w3.org/2000/svg"
       style="animation:fvFloat 4s ease-in-out infinite;
              filter:drop-shadow(0 0 36px rgba(255,210,40,0.58)) drop-shadow(0 0 70px rgba(135,168,255,0.28))">
    <defs>
      <linearGradient id="fvAuV" x1="0%" y1="0%" x2="0%" y2="100%">
        <stop offset="0%"   stop-color="#fff8c0"/>
        <stop offset="32%"  stop-color="#ffd700"/>
        <stop offset="68%"  stop-color="#c48a00"/>
        <stop offset="100%" stop-color="#9e6800"/>
      </linearGradient>
      <linearGradient id="fvAuH" x1="0%" y1="0%" x2="100%" y2="100%">
        <stop offset="0%"   stop-color="#fff5a0"/>
        <stop offset="45%"  stop-color="#ffd700"/>
        <stop offset="100%" stop-color="#c09000"/>
      </linearGradient>
      <radialGradient id="fvDia" cx="30%" cy="22%" r="68%">
        <stop offset="0%"   stop-color="#ffffff"/>
        <stop offset="15%"  stop-color="#f2faff"/>
        <stop offset="45%"  stop-color="#b5d2ff"/>
        <stop offset="78%"  stop-color="#6890d5"/>
        <stop offset="100%" stop-color="#3d5eae"/>
      </radialGradient>
      <radialGradient id="fvAcc" cx="35%" cy="28%" r="65%">
        <stop offset="0%"  stop-color="#ffffff"/>
        <stop offset="50%" stop-color="#cce0ff"/>
        <stop offset="100%" stop-color="#6080c5"/>
      </radialGradient>
      <filter id="fvDg" x="-38%" y="-38%" width="176%" height="176%">
        <feGaussianBlur in="SourceAlpha" stdDeviation="5" result="b"/>
        <feColorMatrix in="b" type="matrix"
          values="0 0 0 0 .3 0 0 0 0 .55 0 0 0 0 1 0 0 0 .9 0" result="g"/>
        <feMerge><feMergeNode in="g"/><feMergeNode in="SourceGraphic"/></feMerge>
      </filter>
      <filter id="fvADg" x="-50%" y="-50%" width="200%" height="200%">
        <feGaussianBlur stdDeviation="2.2" result="b"/>
        <feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge>
      </filter>
      <!-- Clip band: only show from y=138 down (hidden behind stone+setting) -->
      <clipPath id="fvBc">
        <rect x="0" y="138" width="300" height="180"/>
      </clipPath>
    </defs>

    <!--
      GEOMETRY (scaled 1.5x from small version):
      Band circle: cx=150, cy=240, r=95  → top at y=145
      Stone center: cx=150, cy=78,  r=44
      Shoulders sweep from band top up to stone base
    -->

    <!-- ══ BAND — perfect circle ══ -->
    <circle cx="150" cy="240" r="95" fill="none"
            stroke="url(#fvAuV)" stroke-width="16" clip-path="url(#fvBc)"/>
    <circle cx="150" cy="240" r="95" fill="none"
            stroke="rgba(80,50,0,0.28)" stroke-width="6" clip-path="url(#fvBc)"/>
    <circle cx="150" cy="240" r="95" fill="none"
            stroke="rgba(255,250,185,0.7)" stroke-width="4.5"
            stroke-dasharray="85 510" stroke-dashoffset="-162" clip-path="url(#fvBc)"/>

    <!-- ══ SHOULDER RAMPS ══ -->
    <!-- Left: band widens from bottom-left up to center stone setting -->
    <path d="M 62 222 C 57 205,60 182,74 166 C 83 155,96 149,110 147
             L 113 160 C 101 162,91 167,84 177 C 73 190,70 208,74 222 Z"
          fill="url(#fvAuV)"/>
    <!-- Right mirror -->
    <path d="M 238 222 C 243 205,240 182,226 166 C 217 155,204 149,190 147
             L 187 160 C 199 162,209 167,216 177 C 227 190,230 208,226 222 Z"
          fill="url(#fvAuV)"/>
    <!-- Shoulder gloss highlight -->
    <path d="M 65 218 C 61 203,64 182,77 167 C 85 157,97 152,110 150
             L 111 155 C 99 157,88 162,80 172 C 69 184,66 203,70 218 Z"
          fill="rgba(255,252,195,0.38)"/>
    <path d="M 235 218 C 239 203,236 182,223 167 C 215 157,203 152,190 150
             L 189 155 C 201 157,212 162,220 172 C 231 184,234 203,230 218 Z"
          fill="rgba(255,252,195,0.38)"/>

    <!-- ══ ACCENT STONES LEFT ══ -->
    <!-- L1 r=11, closest to center -->
    <circle cx="102" cy="170" r="11"  fill="url(#fvAcc)" filter="url(#fvADg)"/>
    <circle cx="102" cy="170" r="11"  fill="none" stroke="url(#fvAuH)" stroke-width="1.5"/>
    <circle cx="99"  cy="167" r="3.8" fill="rgba(255,255,255,0.95)"/>
    <circle cx="102" cy="159.3" r="2.4" fill="url(#fvAuH)"/>
    <circle cx="102" cy="180.7" r="2.4" fill="url(#fvAuH)"/>
    <circle cx="91.3" cy="170"  r="2.4" fill="url(#fvAuH)"/>
    <circle cx="112.7" cy="170" r="2.4" fill="url(#fvAuH)"/>

    <!-- L2 r=8.5 -->
    <circle cx="80"  cy="194" r="8.5"  fill="url(#fvAcc)" filter="url(#fvADg)"/>
    <circle cx="80"  cy="194" r="8.5"  fill="none" stroke="url(#fvAuH)" stroke-width="1.3"/>
    <circle cx="77.5" cy="191.5" r="2.9" fill="rgba(255,255,255,0.93)"/>
    <circle cx="80"  cy="185.7" r="2"  fill="url(#fvAuH)"/>
    <circle cx="80"  cy="202.3" r="2"  fill="url(#fvAuH)"/>
    <circle cx="71.7" cy="194"  r="2"  fill="url(#fvAuH)"/>
    <circle cx="88.3" cy="194"  r="2"  fill="url(#fvAuH)"/>

    <!-- L3 r=6.5 -->
    <circle cx="63"  cy="217" r="6.5"  fill="url(#fvAcc)" filter="url(#fvADg)"/>
    <circle cx="63"  cy="217" r="6.5"  fill="none" stroke="url(#fvAuH)" stroke-width="1.1"/>
    <circle cx="61"  cy="215" r="2.2"  fill="rgba(255,255,255,0.92)"/>
    <circle cx="63"  cy="210.7" r="1.7" fill="url(#fvAuH)"/>
    <circle cx="63"  cy="223.3" r="1.7" fill="url(#fvAuH)"/>
    <circle cx="56.7" cy="217"  r="1.7" fill="url(#fvAuH)"/>
    <circle cx="69.3" cy="217"  r="1.7" fill="url(#fvAuH)"/>

    <!-- ACCENT STONES RIGHT — mirror -->
    <!-- R1 -->
    <circle cx="198" cy="170" r="11"  fill="url(#fvAcc)" filter="url(#fvADg)"/>
    <circle cx="198" cy="170" r="11"  fill="none" stroke="url(#fvAuH)" stroke-width="1.5"/>
    <circle cx="195" cy="167" r="3.8" fill="rgba(255,255,255,0.95)"/>
    <circle cx="198" cy="159.3" r="2.4" fill="url(#fvAuH)"/>
    <circle cx="198" cy="180.7" r="2.4" fill="url(#fvAuH)"/>
    <circle cx="187.3" cy="170"  r="2.4" fill="url(#fvAuH)"/>
    <circle cx="208.7" cy="170"  r="2.4" fill="url(#fvAuH)"/>

    <!-- R2 -->
    <circle cx="220" cy="194" r="8.5"  fill="url(#fvAcc)" filter="url(#fvADg)"/>
    <circle cx="220" cy="194" r="8.5"  fill="none" stroke="url(#fvAuH)" stroke-width="1.3"/>
    <circle cx="217.5" cy="191.5" r="2.9" fill="rgba(255,255,255,0.93)"/>
    <circle cx="220" cy="185.7" r="2"  fill="url(#fvAuH)"/>
    <circle cx="220" cy="202.3" r="2"  fill="url(#fvAuH)"/>
    <circle cx="211.7" cy="194"  r="2"  fill="url(#fvAuH)"/>
    <circle cx="228.3" cy="194"  r="2"  fill="url(#fvAuH)"/>

    <!-- R3 -->
    <circle cx="237" cy="217" r="6.5"  fill="url(#fvAcc)" filter="url(#fvADg)"/>
    <circle cx="237" cy="217" r="6.5"  fill="none" stroke="url(#fvAuH)" stroke-width="1.1"/>
    <circle cx="235" cy="215" r="2.2"  fill="rgba(255,255,255,0.92)"/>
    <circle cx="237" cy="210.7" r="1.7" fill="url(#fvAuH)"/>
    <circle cx="237" cy="223.3" r="1.7" fill="url(#fvAuH)"/>
    <circle cx="230.7" cy="217"  r="1.7" fill="url(#fvAuH)"/>
    <circle cx="243.3" cy="217"  r="1.7" fill="url(#fvAuH)"/>

    <!-- SETTING BASE — solid gold bridge from band top (y≈148) up to stone base (y≈122) -->
    <path d="M 122 150 C 126 141, 130 130, 128 122
             C 135 116, 142 112, 150 112
             C 158 112, 165 116, 172 122
             C 170 130, 174 141, 178 150
             C 169 145, 160 142, 150 142
             C 140 142, 131 145, 122 150 Z"
          fill="url(#fvAuH)"/>
    <!-- Inner shadow -->
    <path d="M 128 149 C 131 141, 134 132, 133 124 C 139 119, 144 116, 150 116
             C 156 116, 161 119, 167 124 C 166 132, 169 141, 172 149
             C 164 144, 157 142, 150 142 C 143 142, 136 144, 128 149 Z"
          fill="rgba(150,90,0,0.2)"/>

    <!-- ══ 4 PRONG BALLS ══ -->
    <circle cx="128" cy="124" r="8"   fill="url(#fvAuH)"/>
    <circle cx="172" cy="124" r="8"   fill="url(#fvAuH)"/>
    <circle cx="136" cy="112" r="6.5" fill="url(#fvAuH)" opacity="0.85"/>
    <circle cx="164" cy="112" r="6.5" fill="url(#fvAuH)" opacity="0.85"/>
    <circle cx="125" cy="121" r="2.5" fill="rgba(255,252,190,0.88)"/>
    <circle cx="169" cy="121" r="2.5" fill="rgba(255,252,190,0.88)"/>

    <!-- ══ CENTER DIAMOND r=44 ══ -->
    <circle cx="150" cy="78" r="44" fill="url(#fvDia)" filter="url(#fvDg)"/>
    <circle cx="150" cy="78" r="44" fill="none" stroke="rgba(150,190,245,0.2)" stroke-width="1.2"/>
    <!-- Table octagon -->
    <polygon points="150,38 171,48 178,78 171,108 150,118 129,108 122,78 129,48"
             fill="rgba(255,255,255,0.12)" stroke="rgba(140,178,255,0.5)" stroke-width="1.1"/>
    <!-- Facets -->
    <line x1="150" y1="38"  x2="150" y2="118" stroke="rgba(148,190,255,0.17)" stroke-width="0.9"/>
    <line x1="122" y1="78"  x2="178" y2="78"  stroke="rgba(148,190,255,0.17)" stroke-width="0.9"/>
    <line x1="129" y1="48"  x2="171" y2="108" stroke="rgba(148,190,255,0.13)" stroke-width="0.8"/>
    <line x1="171" y1="48"  x2="129" y2="108" stroke="rgba(148,190,255,0.13)" stroke-width="0.8"/>
    <line x1="150" y1="38"  x2="178" y2="78"  stroke="rgba(138,176,255,0.2)"  stroke-width="0.8"/>
    <line x1="150" y1="38"  x2="122" y2="78"  stroke="rgba(138,176,255,0.2)"  stroke-width="0.8"/>
    <line x1="171" y1="108" x2="150" y2="118" stroke="rgba(138,176,255,0.14)" stroke-width="0.75"/>
    <line x1="129" y1="108" x2="150" y2="118" stroke="rgba(138,176,255,0.14)" stroke-width="0.75"/>
    <!-- Primary sparkle — top-left flash -->
    <ellipse cx="129" cy="49" rx="19" ry="11"
             fill="rgba(255,255,255,0.94)" transform="rotate(-32 129 49)"/>
    <circle  cx="124" cy="44" r="8.5" fill="white"/>
    <circle  cx="119" cy="40" r="4.2" fill="rgba(255,255,255,0.8)"/>
    <!-- Secondary sparkle bottom-right -->
    <circle  cx="171" cy="107" r="5.5" fill="rgba(255,255,255,0.6)"/>
    <circle  cx="175" cy="111" r="2.5" fill="rgba(255,255,255,0.38)"/>
    <!-- Colour fire -->
    <ellipse cx="170" cy="50" rx="13"  ry="5" fill="rgba(255,175,210,0.45)" transform="rotate(-20 170 50)"/>
    <ellipse cx="131" cy="107" rx="10" ry="4" fill="rgba(175,210,255,0.43)" transform="rotate(-14 131 107)"/>
    <ellipse cx="167" cy="97"  rx="7"  ry="3" fill="rgba(188,255,208,0.34)" transform="rotate(18 167 97)"/>
    <circle  cx="158" cy="54" r="3.5" fill="rgba(255,255,255,0.7)"/>
    <circle  cx="143" cy="100" r="2.2" fill="rgba(255,255,255,0.45)"/>
  </svg>

  <p id="ringFvClose" style="font-family:'Lato',sans-serif;font-size:11px;letter-spacing:0.14em;
     color:rgba(245,192,210,0.45);margin-top:28px;cursor:pointer;transition:color 0.2s;"
     onmouseover="this.style.color='rgba(245,192,210,0.9)'"
     onmouseout="this.style.color='rgba(245,192,210,0.45)'">
    ✦ tap anywhere to close ✦
  </p>
</div>

<!-- MINI LETTER FULLSCREEN OVERLAY -->
<div class="mini-letter-overlay" id="miniLetterOverlay">
  <div class="mini-letter-modal" id="miniLetterModal">
    <div class="modal-tape"></div>
    <svg class="modal-rose" width="22" height="22" viewBox="0 0 24 24">
      <g transform="translate(12,12)">
        <ellipse cx="0" cy="-5.5" rx="3" ry="5" fill="#ffb3d4"/>
        <ellipse cx="0" cy="-5.5" rx="3" ry="5" fill="#ffc8e0" transform="rotate(72)"/>
        <ellipse cx="0" cy="-5.5" rx="3" ry="5" fill="#ffb3d4" transform="rotate(144)"/>
        <ellipse cx="0" cy="-5.5" rx="3" ry="5" fill="#ffc8e0" transform="rotate(216)"/>
        <ellipse cx="0" cy="-5.5" rx="3" ry="5" fill="#ffb3d4" transform="rotate(288)"/>
        <circle cx="0" cy="0" r="3.5" fill="#ff75ad"/>
      </g>
    </svg>
    <span class="modal-salut">My babi,</span>
    <textarea class="modal-textarea" id="miniLetterText" spellcheck="false"
      rows="6"> 
    I want you to know that loving you and choosing you stopped being a question for me a long time ago.
It became a quiet certainty — the kind that stays steady even when everything else feels uncertain.
You are not just someone I love, you are the one who I want to build, grow, and come home to.
This ring isn’t about pressure or perfection — it’s about my heart being sure of you.
And it has been sure for a while now.     

— From Bab.
</textarea>
    <span class="modal-close-hint" id="modalCloseBtn">Tap here to fold it back ↩</span>
  </div>
</div>

<!-- ═══ SCREEN 4a: YES ═══ -->
<div id="yes-screen">
  <p class="response-msg">Yes!!! ♡</p>
  <p class="response-sub">
    You just made me the happiest person alive.
I assure you that I would keep choosing you, not just in big moments like this, but also in the quiet and ordinary days.
This is not just a “yes” to the engagement— it’s a yes to choosing us, building, growing, and facing everything together.
I love you. And I want you to know that, I'm all in.    
    </p>
  <button class="btn-restart" id="btnRestartYes">♡ Back to the beginning</button>
</div>

<!-- ═══ SCREEN 4b: NO ═══ -->
<div id="no-screen">
  <p class="response-msg" style="color:#6080b0;">That's okay...</p>
  <p class="response-sub" style="color:#506090;"> 
My love for you doesn’t disappear just because the timing wasn’t right.
I don’t want a forced yes — I want a ready one.
and when the day comes that you’re sure, I’ll still be here choosing you.
    
    </p>
  <button class="btn-restart" id="btnRestartNo" style="border-color:#8090c0;color:#6080b0;">← Start over</button>
</div>

<script>
const ns = "http://www.w3.org/2000/svg";

/* ── Screen refs ── */
const envScreen   = document.getElementById('envelope-screen');
const letterScr   = document.getElementById('letter-screen');
const ringScr     = document.getElementById('ring-screen');
const yesScr      = document.getElementById('yes-screen');
const noScr       = document.getElementById('no-screen');

/* ── Element refs ── */
const envBox         = document.getElementById('envBox');
const seal           = document.getElementById('seal');
const btnBack        = document.getElementById('btnBack');
const btnRevealRing  = document.getElementById('btnRevealRing');
const scroll         = document.getElementById('letterScroll');
const scrollHint     = document.getElementById('scrollHint');
const ringBox        = document.getElementById('ringBox');
const ringBoxHint    = document.getElementById('ringBoxHint');
const proposalTxt    = document.getElementById('proposalTxt');
const answerBtns     = document.getElementById('answerBtns');
const btnYes         = document.getElementById('btnYes');
const btnNo          = document.getElementById('btnNo');
const btnRingBack    = document.getElementById('btnRingBack');
const btnRestartYes  = document.getElementById('btnRestartYes');
const btnRestartNo   = document.getElementById('btnRestartNo');

function show(el) { el.classList.add('visible'); }

/* ── Scroll hint ── */
scroll.addEventListener('scroll', () => {
  if (scroll.scrollTop > 20) scrollHint.style.opacity = '0';
}, { passive: true });

/* ── Envelope open ── */
seal.addEventListener('click', () => {
  envBox.classList.add('opening');
  setTimeout(() => {
    envScreen.classList.add('hidden');
    show(letterScr);
    scroll.scrollTop = 0;
    scrollHint.style.opacity = '0.7';
    spawnParticles();
  }, 1000);
});

/* ── Back to envelope ── */
btnBack.addEventListener('click', () => {
  letterScr.classList.remove('visible');
  envBox.classList.remove('opening');
  envScreen.classList.remove('hidden');
});

/* ── Reveal ring screen ── */
btnRevealRing.addEventListener('click', () => {
  letterScr.classList.remove('visible');
  // Reset all ring states
  ringBox.classList.remove('lid-open', 'ring-lifted', 'box-hidden');
  ringDisplay.classList.remove('ring-zoomed');
  ringZoomed = false;
  document.getElementById('miniPeek').classList.remove('visible');
  document.querySelector('.ring-screen-title').classList.remove('visible');
  proposalTxt.classList.remove('show');
  answerBtns.classList.remove('show');
  ringBoxHint.textContent = 'Tap the box to open';
  ringBoxHint.style.opacity = '0.65';
  document.getElementById('miniLetterOverlay').classList.remove('open');
  show(ringScr);
  createSparkles();
});

/* ══════════════════════════════════════════
   3-STEP RING REVEAL LOGIC
   Step 1: closed box (default)
   Step 2: tap box  → lid-open class → lid rotates back, ring fades in
   Step 3: tap ring → ring-lifted class → ring rises + scales up,
           proposal text appears
══════════════════════════════════════════ */
let proposalTimer = null;
let ringZoomed = false;
const ringDisplay = document.getElementById('ringDisplay');

/* TAP THE BOX — opens lid, then fades box away, ring appears */
/* TAP THE BOX — Step 1 → 2 */
ringBox.addEventListener('click', (e) => {
  if (ringBox.classList.contains('ring-lifted')) return;
  if (ringBox.classList.contains('lid-open')) return;
  ringBox.classList.add('lid-open');
  // After lid animation, fade box out
  setTimeout(() => {
    ringBox.classList.add('box-hidden');
    document.getElementById('miniPeek').classList.add('visible');
    ringBoxHint.textContent = 'Tap the ring ✦';
    ringBoxHint.style.opacity = '0.5';
    document.querySelector('.ring-screen-title').classList.add('visible');
  }, 900);
});

/* TAP THE RING — zoom in/out toggle, then show proposal when zoomed */
ringDisplay.addEventListener('click', (e) => {
  if (!ringBox.classList.contains('lid-open')) return;
  e.stopPropagation();

  if (!ringZoomed) {
    // Zoom in
    ringDisplay.classList.add('ring-zoomed');
    ringBoxHint.textContent = '';
    ringZoomed = true;
    // Show proposal after zoom settles
    proposalTimer = setTimeout(() => {
      proposalTxt.classList.add('show');
      setTimeout(() => answerBtns.classList.add('show'), 600);
    }, 400);
  } else {
    // Zoom out
    ringDisplay.classList.remove('ring-zoomed');
    ringZoomed = false;
  }
});

/* ── Mini letter overlay ── */
const miniPeek   = document.getElementById('miniPeek');
const overlay    = document.getElementById('miniLetterOverlay');
const closeBtn   = document.getElementById('modalCloseBtn');

miniPeek.addEventListener('click', () => { overlay.classList.add('open'); });
closeBtn.addEventListener('click', () => { overlay.classList.remove('open'); });
overlay.addEventListener('click', (e) => { if (e.target === overlay) overlay.classList.remove('open'); });

/* ── Back to letter ── */
btnRingBack.addEventListener('click', () => {
  ringScr.classList.remove('visible');
  if (proposalTimer) clearTimeout(proposalTimer);
  show(letterScr);
});

/* ── YES ── */
btnYes.addEventListener('click', () => {
  ringScr.classList.remove('visible');
  show(yesScr);
  spawnCelebration();
});

/* ── NO ── */
btnNo.addEventListener('click', () => {
  ringScr.classList.remove('visible');
  show(noScr);
  spawnRain();
});

/* ── Restart ── */
[btnRestartYes, btnRestartNo].forEach(btn => {
  btn.addEventListener('click', () => {
    yesScr.classList.remove('visible');
    noScr.classList.remove('visible');
    envBox.classList.remove('opening');
    envScreen.classList.remove('hidden');
    envScreen.style.display = '';
  });
});

/* ══════════════════════════
   RING SCREEN SPARKLES
══════════════════════════ */
function createSparkles() {
  const scr = document.getElementById('ring-screen');
  scr.querySelectorAll('.sparkle-dot').forEach(s => s.remove());
  for (let i = 0; i < 30; i++) {
    const dot = document.createElement('div');
    dot.className = 'sparkle-dot';
    const sz = 1.2 + Math.random() * 2.8;
    dot.style.cssText = `width:${sz}px;height:${sz}px;left:${Math.random()*100}%;top:${Math.random()*100}%;animation-duration:${1.4+Math.random()*3.2}s;animation-delay:${Math.random()*3}s;`;
    scr.appendChild(dot);
  }
}

/* ══════════════════════════
   PARTICLE MAKERS
══════════════════════════ */
function makeHeart(size, color) {
  const svg = document.createElementNS(ns,"svg");
  svg.setAttribute("width",size); svg.setAttribute("height",size*0.92);
  svg.setAttribute("viewBox","0 0 20 18");
  const p = document.createElementNS(ns,"path");
  const cols = color ? [color] : ["#ff80b5","#f48fb1","#f06292","#ffb3d4","#ff5599","#ffcce6"];
  p.setAttribute("d","M10 16C10 16 1 10 1 5C1 2.5 3 1 5.5 1C7.2 1 8.8 2 10 3.5C11.2 2 12.8 1 14.5 1C17 1 19 2.5 19 5C19 10 10 16 10 16Z");
  p.setAttribute("fill", cols[Math.floor(Math.random()*cols.length)]);
  svg.appendChild(p); return svg;
}

function makeFlower(size) {
  const svg = document.createElementNS(ns,"svg");
  svg.setAttribute("width",size); svg.setAttribute("height",size);
  svg.setAttribute("viewBox","0 0 40 40");
  const g = document.createElementNS(ns,"g");
  g.setAttribute("transform","translate(20,20)");
  const cols = ["#ffb3d4","#ffc2dc","#ff99cc","#f48fb1","#ffcce6"];
  const c = cols[Math.floor(Math.random()*cols.length)];
  for(let i=0;i<5;i++){
    const e=document.createElementNS(ns,"ellipse");
    e.setAttribute("cx","0"); e.setAttribute("cy","-9");
    e.setAttribute("rx","4"); e.setAttribute("ry","8");
    e.setAttribute("fill",c);
    e.setAttribute("transform",`rotate(${i*72})`);
    g.appendChild(e);
  }
  const dot=document.createElementNS(ns,"circle");
  dot.setAttribute("cx","0"); dot.setAttribute("cy","0");
  dot.setAttribute("r","5"); dot.setAttribute("fill","#ff70a8");
  g.appendChild(dot); svg.appendChild(g); return svg;
}

function makeConfettiRect(size, color) {
  const svg = document.createElementNS(ns,"svg");
  svg.setAttribute("width",size); svg.setAttribute("height",size*0.5);
  svg.setAttribute("viewBox","0 0 20 10");
  const r = document.createElementNS(ns,"rect");
  r.setAttribute("x","0"); r.setAttribute("y","0");
  r.setAttribute("width","20"); r.setAttribute("height","10");
  r.setAttribute("rx","2"); r.setAttribute("fill",color);
  svg.appendChild(r); return svg;
}

function makeStar(size, color) {
  const svg = document.createElementNS(ns,"svg");
  svg.setAttribute("width",size); svg.setAttribute("height",size);
  svg.setAttribute("viewBox","0 0 24 24");
  const p = document.createElementNS(ns,"polygon");
  p.setAttribute("points","12,2 15,9 22,9 16,14 18,21 12,17 6,21 8,14 2,9 9,9");
  p.setAttribute("fill",color);
  svg.appendChild(p); return svg;
}

function makeRaindrop(h) {
  const svg = document.createElementNS(ns,"svg");
  svg.setAttribute("width","3"); svg.setAttribute("height",h);
  svg.setAttribute("viewBox",`0 0 3 ${h}`);
  const r = document.createElementNS(ns,"rect");
  r.setAttribute("x","0"); r.setAttribute("y","0");
  r.setAttribute("width","2.5"); r.setAttribute("height",h);
  r.setAttribute("rx","1.25");
  r.setAttribute("fill","rgba(120,145,220,0.55)");
  svg.appendChild(r); return svg;
}

/* ── Envelope open: hearts & flowers ── */
function spawnParticles(){
  let n=0;
  const iv=setInterval(()=>{
    if(n++>=38){clearInterval(iv);return;}
    const el=document.createElement('div');
    el.className='falling';
    const size=11+Math.floor(Math.random()*20);
    el.appendChild(Math.random()>0.45?makeHeart(size):makeFlower(size));
    el.style.left=(Math.random()*100)+'vw';
    const dur=3.5+Math.random()*4;
    el.style.animationDuration=dur+'s';
    el.style.animationDelay=(Math.random()*0.8)+'s';
    document.body.appendChild(el);
    setTimeout(()=>el.remove(),(dur+1.5)*1000);
  },160);
}

/* ── YES: confetti ── */
function spawnCelebration(){
  const cols = ["#f06292","#ffd700","#c2185b","#ff80ab","#fff176","#80deea","#ce93d8","#a5d6a7","#ff8a65","#ffcc02"];
  let n=0;
  const iv=setInterval(()=>{
    if(n++>=70){clearInterval(iv);return;}
    const el=document.createElement('div');
    el.className='confetti-piece';
    el.style.left=(Math.random()*100)+'vw';
    const dur=2.8+Math.random()*3.5;
    el.style.animationDuration=dur+'s';
    el.style.animationDelay=(Math.random()*1.4)+'s';
    const color=cols[Math.floor(Math.random()*cols.length)];
    const size=9+Math.floor(Math.random()*18);
    const t=Math.random();
    if(t<0.38)      el.appendChild(makeHeart(size,color));
    else if(t<0.66) el.appendChild(makeConfettiRect(size,color));
    else            el.appendChild(makeStar(size,color));
    document.body.appendChild(el);
    setTimeout(()=>el.remove(),(dur+1.5)*1000);
  },80);
}

/* ── NO: rain ── */
function spawnRain(){
  let n=0;
  const iv=setInterval(()=>{
    if(n++>=90){clearInterval(iv);return;}
    const el=document.createElement('div');
    el.className='raindrop';
    el.style.left=(Math.random()*102)+'vw';
    const h=16+Math.floor(Math.random()*22);
    el.appendChild(makeRaindrop(h));
    const dur=0.7+Math.random()*0.75;
    el.style.animationDuration=dur+'s';
    el.style.animationDelay=(Math.random()*2)+'s';
    document.body.appendChild(el);
    setTimeout(()=>el.remove(),(dur+2.5)*1000);
  },50);
}
</script>
</body>
</html>

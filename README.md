# Valentines
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>❤️ Valentine?</title>
  <style>
    :root{--bg1:#ffd6e7;--bg2:#ffeef6;--hot:#ff2d6f;--hot2:#ff5a8f;--text:#b3003b;}
    *{box-sizing:border-box}
    body{margin:0;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;height:100vh;display:flex;align-items:center;justify-content:center;background:radial-gradient(1000px 700px at 50% 20%,var(--bg2),var(--bg1));overflow:hidden;user-select:none;-webkit-tap-highlight-color:transparent}
    .hearts{position:fixed;top:10px;left:0;right:0;text-align:center;font-size:18px;opacity:.9;pointer-events:none;letter-spacing:6px;transform:rotate(-4deg);filter:drop-shadow(0 2px 6px rgba(0,0,0,.1))}
    .card{width:min(92vw,420px);padding:28px 22px 22px;border-radius:24px;background:rgba(255,255,255,.55);backdrop-filter:blur(10px);box-shadow:0 18px 50px rgba(0,0,0,.12);border:1px solid rgba(255,255,255,.65);position:relative}
    h1{margin:0 0 8px;color:var(--text);font-size:34px;line-height:1.05;text-align:center;font-weight:800}
    .sub{text-align:center;margin:0 0 18px;color:#cc0048;font-weight:600;opacity:.85}
    .stage{position:relative;height:190px;border-radius:18px;background:linear-gradient(180deg,rgba(255,255,255,.65),rgba(255,255,255,.35));border:1px solid rgba(255,255,255,.7);overflow:hidden;padding:14px}
    .btn{border:none;font-weight:800;font-size:18px;padding:14px 20px;border-radius:16px;cursor:pointer;display:inline-flex;align-items:center;justify-content:center;gap:8px;min-width:140px;touch-action:manipulation;transition:transform .08s ease}
    .btn:active{transform:scale(.98)}
    .yes{background:linear-gradient(180deg,var(--hot2),var(--hot));color:#fff;box-shadow:0 10px 24px rgba(255,45,111,.35)}
    .no{position:absolute;left:50%;top:118px;transform:translateX(-50%);background:rgba(255,255,255,.8);color:#7a0030;border:2px solid rgba(255,45,111,.35);box-shadow:0 10px 18px rgba(0,0,0,.08)}
    .yesWrap{display:flex;justify-content:center;margin-top:10px}
    .tiny{font-size:13px;text-align:center;color:#9b1b4e;opacity:.65;margin-top:10px}
    .success{display:none;text-align:center;padding:18px 10px 6px}
    .success h2{margin:0;color:var(--text);font-size:34px;font-weight:900;letter-spacing:.5px}
    .success p{margin:10px 0 0;color:#b3003b;font-weight:700;opacity:.9;font-size:18px}
    .bigHeart{width:120px;height:110px;margin:8px auto 10px;position:relative;transform:translateY(6px);filter:drop-shadow(0 12px 22px rgba(255,45,111,.25))}
    .bigHeart:before,.bigHeart:after{content:"";position:absolute;width:60px;height:96px;background:var(--hot);border-radius:60px 60px 0 0;left:30px;top:10px;transform:rotate(-45deg);transform-origin:0 100%}
    .bigHeart:after{left:30px;transform:rotate(45deg);transform-origin:100% 100%}
    canvas#confetti{position:fixed;inset:0;pointer-events:none;display:none}
  </style>
</head>
<body>
  <div class="hearts">💗 💗 💗 💗 💗 💗 💗 💗</div>
  <canvas id="confetti"></canvas>

  <div class="card">
    <div id="ask">
      <h1>Jason, will you<br/>be my Valentine? 💕</h1>
      <p class="sub">Be honest… but choose wisely 😇</p>

      <div class="stage" id="stage">
        <div class="yesWrap">
          <button class="btn yes" id="yesBtn">Yes! 💞</button>
        </div>
        <button class="btn no" id="noBtn" aria-label="No">No</button>
      </div>

      <div class="tiny">Tip: try tapping “No” 😈</div>
    </div>

    <div id="win" class="success">
      <div class="bigHeart"></div>
      <h2>YAY! 🎉</h2>
      <p>Best decision ever!<br/><span style="font-weight:900;">I LOVE YOU 😘</span></p>
    </div>
  </div>

<script>
  const stage = document.getElementById("stage");
  const noBtn  = document.getElementById("noBtn");
  const yesBtn = document.getElementById("yesBtn");
  const ask    = document.getElementById("ask");
  const win    = document.getElementById("win");

  function clamp(v,min,max){ return Math.max(min, Math.min(max, v)); }

  function moveNoButtonAway(fromX, fromY){
    const rect = stage.getBoundingClientRect();
    const btn  = noBtn.getBoundingClientRect();

    const bx = (btn.left - rect.left) + btn.width / 2;
    const by = (btn.top  - rect.top ) + btn.height / 2;

    const dx = bx - fromX, dy = by - fromY;
    const mag = Math.max(1, Math.hypot(dx,dy));
    const ux = dx / mag, uy = dy / mag;

    const kick = 70 + Math.random()*80;
    let nx = bx + ux*kick + (Math.random()*80 - 40);
    let ny = by + uy*kick + (Math.random()*60 - 30);

    const pad=12;
    const minX = pad + btn.width/2, maxX = rect.width - pad - btn.width/2;
    const minY = pad + btn.height/2, maxY = rect.height - pad - btn.height/2;

    nx = clamp(nx, minX, maxX);
    ny = clamp(ny, minY, maxY);

    noBtn.style.left = (nx - btn.width/2) + "px";
    noBtn.style.top  = (ny - btn.height/2) + "px";
    noBtn.style.transform = "none";
  }

  noBtn.addEventListener("pointerenter", (e)=>{
    const rect = stage.getBoundingClientRect();
    moveNoButtonAway(e.clientX-rect.left, e.clientY-rect.top);
  });

  noBtn.addEventListener("pointerdown", (e)=>{
    e.preventDefault();
    const rect = stage.getBoundingClientRect();
    moveNoButtonAway(e.clientX-rect.left, e.clientY-rect.top);
  });

  stage.addEventListener("pointermove", (e)=>{
    const rect = stage.getBoundingClientRect();
    const x = e.clientX - rect.left, y = e.clientY - rect.top;
    const b = noBtn.getBoundingClientRect();
    const bx = (b.left-rect.left)+b.width/2, by=(b.top-rect.top)+b.height/2;
    if (Math.hypot(bx-x,by-y) < 85) moveNoButtonAway(x,y);
  });

  // confetti
  const canvas = document.getElementById("confetti");
  const ctx = canvas.getContext("2d");
  function resize(){
    canvas.width = innerWidth * devicePixelRatio;
    canvas.height = innerHeight * devicePixelRatio;
    ctx.setTransform(devicePixelRatio,0,0,devicePixelRatio,0,0);
  }
  addEventListener("resize", resize);

  function makeConfetti(n){
    const colors=["#ff2d6f","#ff5a8f","#ff8fb3","#ffffff","#ff3b7d"];
    return Array.from({length:n},()=>({
      x:Math.random()*innerWidth,
      y:Math.random()*-innerHeight,
      vx:(Math.random()*2-1)*1.2,
      vy:1+Math.random()*2.8,
      vr:(Math.random()*2-1)*0.08,
      rot:Math.random()*Math.PI,
      w:4+Math.random()*10,
      h:4+Math.random()*12,
      c:colors[(Math.random()*colors.length)|0]
    }));
  }

  function startConfetti(ms){
    resize();
    canvas.style.display="block";
    let pieces = makeConfetti(180);
    const endAt = performance.now()+ms;

    (function tick(now){
      ctx.clearRect(0,0,innerWidth,innerHeight);
      for(const p of pieces){
        p.vy += 0.06;
        p.x += p.vx; p.y += p.vy; p.rot += p.vr;
        if(p.y > innerHeight+30){ p.y=-20; p.x=Math.random()*innerWidth; p.vy=1+Math.random()*2.5; }
        ctx.save();
        ctx.translate(p.x,p.y);
        ctx.rotate(p.rot);
        ctx.globalAlpha=0.9;
        ctx.fillStyle=p.c;
        ctx.fillRect(-p.w/2,-p.h/2,p.w,p.h);
        ctx.restore();
      }
      if(now < endAt) requestAnimationFrame(tick);
      else canvas.style.display="none";
    })(performance.now());
  }

  yesBtn.addEventListener("click", ()=>{
    ask.style.display="none";
    win.style.display="block";
    startConfetti(1600);
  });
</script>
</body>
</html>

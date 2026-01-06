<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>Ahmed Mahmoud Live Photo</title>

<style>
body{
  margin:0;
  background:#000;
  display:flex;
  justify-content:center;
  align-items:center;
  height:100vh;
  font-family:Arial;
}
.studio{
  width:360px;
  background:#111;
  padding:15px;
  border-radius:20px;
  box-shadow:0 0 30px #0ff;
  text-align:center;
}
h2{
  color:#0ff;
  font-size:18px;
}
.frame{
  border:3px solid #0ff;
  border-radius:15px;
  overflow:hidden;
}
video,img{
  width:100%;
}
button{
  margin-top:10px;
  padding:10px 20px;
  font-size:16px;
  border:none;
  border-radius:25px;
  background:#0ff;
  cursor:pointer;
}
.preview{
  margin-top:10px;
  display:none;
}
.actions{
  display:flex;
  justify-content:center;
  gap:10px;
  margin-top:6px;
}
.actions button{
  background:#222;
  color:#0ff;
  border:1px solid #0ff;
  padding:5px 10px;
  border-radius:10px;
}
</style>
</head>

<body>

<div class="studio">
  <h2>أحمد محمود (لايف فوتو)</h2>

  <div class="frame">
    <video id="cam" autoplay playsinline></video>
  </div>

  <button id="shoot">📸 تصوير</button>

  <div class="preview">
    <p style="color:#aaa;font-size:13px;">اضغط مطوّل على الصورة</p>
    <img id="photo">
    <div class="actions">
      <button onclick="saveImage()">⬇️ حفظ</button>
    </div>
    <video id="motion" muted loop style="display:none"></video>
  </div>
</div>

<canvas id="canvas" style="display:none"></canvas>

<script>
const cam = document.getElementById("cam");
const canvas = document.getElementById("canvas");
const shoot = document.getElementById("shoot");
const photo = document.getElementById("photo");
const motion = document.getElementById("motion");
const preview = document.querySelector(".preview");

let recorder, chunks=[];

// تشغيل الكاميرا
navigator.mediaDevices.getUserMedia({
  video:{width:{ideal:1920},height:{ideal:1080}},
  audio:false
}).then(stream=>{
  cam.srcObject = stream;
  recorder = new MediaRecorder(stream);
  recorder.ondataavailable = e => chunks.push(e.data);
  recorder.onstop = ()=>{
    const blob = new Blob(chunks,{type:"video/webm"});
    motion.src = URL.createObjectURL(blob);
  };
});

// تصوير
shoot.onclick = ()=>{
  chunks=[];
  recorder.start();
  setTimeout(capturePhoto,700);
  setTimeout(()=>recorder.stop(),1400);
};

// التقاط الصورة + علامة مائية
function capturePhoto(){
  canvas.width = cam.videoWidth;
  canvas.height = cam.videoHeight;
  const ctx = canvas.getContext("2d");

  ctx.drawImage(cam,0,0);

  ctx.font="28px Arial";
  ctx.fillStyle="rgba(255,255,255,0.4)";
  ctx.textAlign="right";
  ctx.fillText("Ahmed Mahmoud",canvas.width-20,canvas.height-20);

  photo.src = canvas.toDataURL("image/jpeg",1);
  preview.style.display="block";
}

// Live effect
photo.onmousedown = ()=>motion.play();
photo.onmouseup = ()=>motion.pause();
photo.ontouchstart = ()=>motion.play();
photo.ontouchend = ()=>motion.pause();

// حفظ الصورة
function saveImage(){
  const a = document.createElement("a");
  a.href = photo.src;
  a.download = "Ahmed-Mahmoud-LivePhoto.jpg";
  a.click();
}
</script>

</body>
</html>
<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>Ahmed Mahmoud Camera</title>

<style>
body{
  margin:0;
  background:#000;
  display:flex;
  justify-content:center;
  align-items:center;
  height:100vh;
  font-family:Arial;
}
.app{
  width:360px;
  background:#111;
  padding:15px;
  border-radius:20px;
  box-shadow:0 0 30px #000;
  text-align:center;
}
h2{
  color:#f5deb3;
  font-size:18px;
  margin-bottom:10px;
}
.frame{
  border:5px solid #222;
  border-radius:15px;
  overflow:hidden;
  filter:
    contrast(1.1)
    saturate(0.9)
    sepia(0.25)
    brightness(0.95);
}
video,img{
  width:100%;
}
button{
  margin-top:10px;
  padding:10px 20px;
  font-size:16px;
  border:none;
  border-radius:25px;
  background:#f5deb3;
  cursor:pointer;
}
</style>
</head>

<body>

<div class="app">
  <h2>Ahmed Mahmoud Film Cam</h2>

  <div class="frame">
    <video id="cam" autoplay playsinline></video>
  </div>

  <button onclick="shoot()">📸 SNAP</button>

  <img id="photo" style="display:none;margin-top:10px;">
</div>

<canvas id="canvas" style="display:none"></canvas>

<script>
const cam = document.getElementById("cam");
const canvas = document.getElementById("canvas");
const photo = document.getElementById("photo");

navigator.mediaDevices.getUserMedia({
  video:{width:{ideal:1920},height:{ideal:1080}},
  audio:false
}).then(stream=>cam.srcObject=stream);

function shoot(){
  canvas.width = cam.videoWidth;
  canvas.height = cam.videoHeight;
  const ctx = canvas.getContext("2d");

  ctx.filter = "contrast(1.1) saturate(0.9) sepia(0.25) brightness(0.95)";
  ctx.drawImage(cam,0,0);

  // Grain
  for(let i=0;i<8000;i++){
    const x=Math.random()*canvas.width;
    const y=Math.random()*canvas.height;
    ctx.fillStyle="rgba(0,0,0,0.05)";
    ctx.fillRect(x,y,1,1);
  }

  // تاريخ
  ctx.font="28px monospace";
  ctx.fillStyle="rgba(255,240,200,0.8)";
  ctx.fillText(new Date().toLocaleDateString(),20,canvas.height-20);

  photo.src = canvas.toDataURL("image/jpeg",0.9);
  photo.style.display="block";
}
</script>

</body>
</html>

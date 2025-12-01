<!-- ============================= -->
<!--   GEO BOT WEBSITE (UPDATED)  -->
<!--   LOGIN SISWA B + LOGIN GURU B -->
<!--   FULL REWRITE FOR STABILITY  -->
<!-- ============================= -->
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>GeoBot - Website Geografi</title>
  <style>
    body{font-family:Arial;background:#eaf0e6;margin:0;}
    header{background:#3a6f41;color:white;padding:14px;text-align:center;font-size:22px;font-weight:bold;}
    #loginScreen, #loginGuruScreen{
      position:fixed; inset:0; background:#e8f0e5;
      display:flex; justify-content:center; align-items:center;
      z-index:9999;
    }
    .loginBox{
      background:white; width:300px; padding:22px;
      border-radius:12px; box-shadow:0 2px 6px rgba(0,0,0,0.3);
      text-align:center;
    }
    input, button{width:100%; padding:10px; margin-top:10px; border-radius:6px; border:1px solid #ccc;}
    button{background:#3a6f41; color:white; cursor:pointer; border:none;}
  </style>
</head>
<body>

<header>GeoBot - Website Geografi</header>

<!-- ==================== LOGIN SISWA ==================== -->
<div id="loginStudent" class="loginScreen">
  <div class="loginBox">
    <h2>Login Siswa</h2>
    <input id="studentUser" placeholder="Username siswa">
    <input id="studentPass" type="password" placeholder="Password siswa">
    <button onclick="window.doStudentLogin()">Masuk</button>
    <button style="background:#6b6b6b" onclick="window.openGuruLogin()">Login Guru</button>
  </div>
</div>

<!-- ==================== LOGIN GURU ==================== -->
<div id="loginTeacher" class="loginScreen" style="display:none;">
  <div class="loginBox">
    <h2>Login Guru</h2>
    <input id="teacherUser" placeholder="Username guru">
    <input id="teacherPass" type="password" placeholder="Password guru">
    <button onclick="window.doTeacherLogin()">Masuk</button>
    <button style="background:#6b6b6b" onclick="window.closeGuruLogin()">Kembali</button>
  </div>
</div>

<!-- ==================== AREA CHATBOT ==================== -->
<div id="app" style="display:none; padding:20px;">
  <h2>Selamat Datang, <span id="activeUser"></span></h2>
  <p></p>
</div>

<script>
// ========================= DATABASE =========================
let savedGuru = JSON.parse(localStorage.getItem("geobot_guru")) || { user:"guru", pass:"1234" };
let savedStudents = JSON.parse(localStorage.getItem("geobot_students")) || {
  "dina05": "1234",
  "budi01": "1111"
};

let activeUser = null;
let activeRole = null;

function saveDB(){
  localStorage.setItem("geobot_guru", JSON.stringify(savedGuru));
  localStorage.setItem("geobot_students", JSON.stringify(savedStudents));
}

// ========================= LOGIN SISWA =========================
window.doStudentLogin = function(){
  let u = document.getElementById("studentUser").value;
  let p = document.getElementById("studentPass").value;

  if(savedStudents[u] && savedStudents[u] === p){
    activeUser = u;
    activeRole = "siswa";
    masukKeApp();
  } else {
    alert("Username atau password tidak sesuai!");
  }
}

// ========================= LOGIN GURU =========================
window.openGuruLogin = function(){
  document.getElementById("loginStudent").style.display = "none";
  document.getElementById("loginTeacher").style.display = "flex";
}
window.closeGuruLogin = function(){
  document.getElementById("loginTeacher").style.display = "none";
  document.getElementById("loginStudent").style.display = "flex";
}

window.doTeacherLogin = function(){
  let u = document.getElementById("teacherUser").value;
  let p = document.getElementById("teacherPass").value;

  if(u === savedGuru.user && p === savedGuru.pass){
    activeUser = u;
    activeRole = "guru";
    masukKeApp();
  } else {
    alert("Login guru salah!");
  }
}

// ========================= MASUK KE APLIKASI =========================
function masukKeApp(){
  document.getElementById("loginStudent").style.display = "none";
  document.getElementById("loginTeacher").style.display = "none";
  document.getElementById("app").style.display = "block";

  document.getElementById("activeUser").textContent = `${activeUser} (${activeRole})`;
}

</script>

</body>
</html>

<!DOCTYPE html>
<html dir="rtl" lang="he">
	<head>
		<!--index.md-->
		<meta charset="utf-8">		
		<style>* {font-size: 70px; text-align: center;}</style>
	</head>
	<body>	
		<p id="msg">מטרת משחק זה היא לאמן אותך לחשב מהר יותר.<br>עליך לפתור 10 תרגילים במהירות מסויימת,<br>אם מצליחים התרגילים נעשים קשים קצת יותר,<br>אם נכשלים המהירות נעשית קלה קצת יותר (מלבד כל שלב עשירי, שהם שלבים מאתגרים יותר, כי בהם המהירות לא משתנה),<br>כך שהמהירות הנדרשת היא תמיד בסביבות קצה היכולת שלך.<br>(המלצה: קל יותר להשתמש בריבוע המספרים מאשר בשורת המספרים).</p>
		<div id="start screen">
			<p>שלב <span id="lvl">1</span></p>
			<p><button id="start" type="button" onclick="start()">התחל</button></p>
			<p>
				<button type="button" onclick="save()">שמור</button>
				<button type="button" onclick="load()">טען</button>
				<button type="button" onclick="del()">מחק</button>
			</p>
		</div>
		<div id="game screen" style="display:none" dir="ltr"><p><span id="question"></span><input id="answer" type="number" style="text-align:left" value="" oninput="typing(this.value)"></p></div>	
		<p id="spd" style="display:none">1000</p>
		<script>
var lvl, st, td, qn, lb, b, s, o, result;

function start(){
	lvl=Number(document.getElementById("lvl").innerHTML); //level
	st=Date.now(); //start time
	td=0; //total digits
	qn=1; //question number
	document.getElementById("game screen").style.display="block";	
	document.getElementById("answer").focus();
	document.getElementById("start screen").style.display="none";
	newQuestion()
}
	
function newQuestion(){
	lb = (lvl - 9 > 1) ? lvl - 9 : 1; //lower bound
	b = Math.floor((lvl - lb + 1) * Math.random() + lb); //big number
	s = Math.floor((b / 2) * Math.random() + 1); //small number
	o = Math.floor(4 * Math.random() + 1); //operator
	switch (o){
		case 1:
			document.getElementById("question").innerHTML = s + "+" + (b - s) + "=";
			result = b;
			break;
		case 2:
			document.getElementById("question").innerHTML = b + "-" + s + "=";
			result = b - s;
			break;
		case 3:
			document.getElementById("question").innerHTML = Math.floor(b / s) + "×" + s + "=";
			result = Math.floor(b / s) * s;
			break;
		case 4:
			document.getElementById("question").innerHTML = Math.floor(b / s) * s + <!--"÷"-->"/" + s + "=";
			result = Math.floor(b / s);
	}
	td+=result.toString().length;
	document.getElementById("msg").innerHTML ="תרגיל&nbsp;" + qn;
}

function typing(answer){
  question=document.getElementById("question").innerHTML;
  if (result.toString().includes(answer) == false) {
    document.getElementById("answer").value  = "";
    if (question.indexOf("=") == question.length-1) {
      document.getElementById("question").innerHTML +=  result;
      td -= result.toString().length;
    }
  } else if (answer == result) {
    document.getElementById("answer").value  = "";
    if (question.indexOf("=") == question.length-1) {
      qn++;
    }
  if (qn<11) {
    newQuestion()
  } else {
    end()
  }
  }
}

function end(){
  document.getElementById("game screen").style.display="none";  
  at = Math.round((Date.now() - st) / td);  
  spd = Number(document.getElementById("spd").innerHTML);
  document.getElementById("msg").innerHTML = ((at <= spd) ? "עלית שלב (בזכות " : "נשארת שלב (בגלל ") + "פער של " + Math.abs((at-spd)/1000) + " שניות)";
  if (at <= spd) {
    document.getElementById("lvl").innerHTML++;	
	document.getElementById("start screen").style.color=(lvl % 9==0)?"red":"black";
  }
  if (lvl % 10 > 0) {
    document.getElementById("spd").innerHTML = Math.round((spd + at) / 2);	 
  }
  document.getElementById("start screen").style.display="block";
  document.getElementById("start").focus();
}  
function save(){
  if (typeof(Storage) !== "undefined") {
    localStorage.lvl = document.getElementById("lvl").innerHTML;
    localStorage.spd = document.getElementById("spd").innerHTML;
    document.getElementById("msg").innerHTML="המשחק נשמר";
  } else {
    document.getElementById("msg").innerHTML="אין אפשרות לשמור";
}
 }
function load(){
  document.getElementById("lvl").innerHTML = localStorage.lvl;
  document.getElementById("spd").innerHTML = localStorage.spd;
  document.getElementById("msg").innerHTML="המשחק נטען";
}
 
function del(){
  localStorage.removeItem("lvl");
  localStorage.removeItem("spd");
  document.getElementById("msg").innerHTML="השמירה נמחקה";
}
</script>
	</body>
</html>
# Snake-Game
<!DOCTYPE html>
<html>
	<head>
		<title>Anamika</title>
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<style>
			*{
	box-sizing: border-box;
}
body {
	background-color: #fff;
}
	  #board{
  		display: grid;
  		border:1px solid red;
  		width: 75vh;
  		height: 75vh;
  		margin:auto;
  		background-color: #ccc;
	  }
	  .snakeBody{
	  	background-color: blue;
  		border:1px solid white;
	  }
	  #botton_cont{
  		position:absolute;
	   	display: block;
	  	border:2px solid cyan;
	  	text-align: center;
	  	border-radius:50%;
	  	height: 25vh;
	  	width: 25vh;
	  background-image: radial-gradient(rgb(50,50,50),brown);
	  	background-color: brown;
  		bottom:0%;
  		left:calc((100vw - 25vh)/2);
  		box-shadow:3px 3px 3px black;
  		visibility:hidden;
	  }
	  #botton_cont>*{
	  	margin:auto;
	  	border:1px solid white;
	  	background-color: silver;
	  	color: white;
	  	width:30%;
	  	height: 32%;
	  	border-radius:30%;
	  	border-style: outset;
	  	font-size: bolder;
	  	text-align: center;
	  }

	  #upBtn, #downBtn{
	  	display: block;
	  	width:40%;
	  }
	  #pauseBtn{
		  border-radius:50%;
	  }
	  #food{
	  	border-color: red;
	  	background-color: green;
  		margin:0;
  		paddding:0;
  		border-radius:50%;
  		animation: headEffect 500ms infinite;
	  }
	  #leader{
  		animation: headEffect 500ms infinite;
	  }
	  @keyframes headEffect {
  		from{
  		  background-color:blue;
  		  border:2px solid white;
  		}
  		to{
  		  background-color:white;
  		  border:2px solid red;
  		}
	  }
	  section{
  	  position:absolute;
  	  top:15vh;
  	  left:12.5vw;
		border:2px solid white;
		text-align: center;
		display: block;
		margin: auto;
		color: white;
	  box-shadow: 3px 2px 5px grey;
	  background-color:red;
	  padding: 0 3vw;
	  width:75vw;
	}
	section>h2{
	  border:1px solid black;
	  background-color:black;
	}
	section>div>button{
		border:1px solid white;
		width: 40%;
		box-shadow: 2px 2px 3px black;
		margin-bottom: 2vh;
		height: 5vh;
		border-radius: 2vw 1vh;
		font-size: bolder;
		background-color: pink;
	}
	#mRight,#mLeft{
	  display:inline-block;
	}
	#mRight{
	  float:right;
	}
	#mLeft{
	  float:left;
	}
	#gmInfo{
	  position:absolute;
	  width:100vw;
	}
	.dir{
	  width:3vmin;
	}
		</style>
</head>
	<body onkeyup="keyHandler();">
		<h1 style = "text-align:center;">Made by Anamika Kashyap<br> Press a direction and start playing</h1>
	<div id='board'>
	</div>
	<section id="prompt">
		<h2>Anamika's "Xx_SNAKE_xX"</h2>
		<h3 id='notf'>Tutorial</h3>
		<p id='msg'>The game is Made by Anamika and is very easy; <br>
		1) Eat the dropped apples, <br>
		2) Do not bite yourself, <br>
		3) Stay away from the walls.
	</p>
	<h4 id='comment'>Good Luck</h4>
		<div>
			<button onclick="PlayHandler()">Play</button>
		</div>

	</section>
  <div id='gmInfo'>
  <h1 id='mLeft'>Points:<br><center id='pt'>0</center></h1>
  <h1 id='mRight'>Lives:<br><center>1</center></h1>
  </div>
	<div id="botton_cont">
		<button id="upBtn" onclick="ButtonHandler([0,-1])">
	  <img style='transform:rotate(-270deg)' class='dir' src="https://dl.dropboxusercontent.com/s/a0shbsupbzhw6ro/Red_Arrow_PNG.png?dl=0" alt="">
	</button>
		<button id= "leftBtn" onclick="ButtonHandler([-1,0])">
	  <img class='dir' src="https://dl.dropboxusercontent.com/s/a0shbsupbzhw6ro/Red_Arrow_PNG.png?dl=0" alt="">
	</button>
	<button id="pauseBtn" onclick="PauseHandler();">II</button>
		<button id="rightBtn" onclick="ButtonHandler([1,0])">
	  <img style='transform:rotate(180deg)' class='dir' src="https://dl.dropboxusercontent.com/s/a0shbsupbzhw6ro/Red_Arrow_PNG.png?dl=0" alt="">
	</button>
		<button id="downBtn" onclick="ButtonHandler([0,1])">
	  <img style='transform:rotate(-90deg)' class='dir' src="https://dl.dropboxusercontent.com/s/a0shbsupbzhw6ro/Red_Arrow_PNG.png?dl=0" alt="">
	</button>
	</div>
	<script>
			var snake,snakeTimer,gridNum,in_game;
	  window.onload = function(){
	  	Make_Grid();
	  	snake = new Game();
	  	snake.Display();
	  }
	  function Grid_Auto_Gen(num){
	  //create the num colomn for the grid
  		let strTemp = ''
  		for(let i=0;i<num;i++){
  		  strTemp+='auto ';
  		}
  		return strTemp;
	  }
	  function Make_Grid(){
	  //size the game area base on window height and width
  		var w =document.body.clientWidth;
  		let board = document.getElementById('board');
  		if (board.getBoundingClientRect().width > (w*0.95)){
  		  board.style.width = board.style.height = `${w*0.95}px`;
  		  let controlla = document.getElementById('botton_cont');
  		  controlla.style.left = `${(w - controlla.clientWidth)/2}px`;
  		};
  		gridNum  = Math.floor(board.clientWidth/15);//scaling the grid based on the numbers of pixels
  		board.style.gridTemplateColumns = Grid_Auto_Gen(gridNum);
	  	for(let i=0;i<Math.pow(gridNum, 2);i++){
	  		let blk = document.createElement("div");
  		  blk.style.width = blk.style.height = `${board.clientWidth/gridNum}px`;
  			board.appendChild(blk);
	  	}
	  }
	  class Game{
	  //the single class that handler all game logic
	  	constructor(){
		in_game = true;
		this.currPt = 0;
		this.pointEl = document.getElementById('pt');
		this.pointEl.textContent = '0';
  	  	this.food = null;
  	  	this.board = document.getElementById("board");
	  		this.vel = [1,0];
		// this.snakeArr -> refrence the grid numbers that are part of the snakebody;
	  		this.snakeArr = [this.board.children[0],this.board.children[1],this.board.children[2],this.board.children[3]];// (3) the initial snake size
	  }
		Display(){
		//colors the grid of the board that are part of the snake using the snakeArr;
		//color the body of the snake
		  for(let _blk of this.board.children){//loop through the grid of the game area
		  	if (!this.snakeArr.includes(_blk)){//if the grid is not part of the snake (from the snakeArr)
	  	  	_blk.removeAttribute("class");//uncolor it
		  	}
		  }
		  for(let _blk of this.snakeArr){ //color the body of the snake
		  	if(_blk.getAttribute("class") != "snakeBody"){
				_blk.setAttribute("class","snakeBody");
	  	  }
		  }
	  	}
	  	UpdateLeader(){
		//manage the snakehead(leader) the last refrence in the snakeArr
  	  	let leaderCurrIndex = Array.prototype.indexOf.call(this.board.children, this.snakeArr[this.snakeArr.length-1]);//current index of snake head in the game area
		let hitWall = false; // has the snake head hit the wall, default false
  	  	if (this.vel[0]==0 && this.vel[1]==1){//moving downward
				if ((leaderCurrIndex + gridNum)>this.board.children.length){//check if the head is about to collide with wall, set hitwall to true
			hitWall = true;
				}else{//if not about to collide with the wall, move the snake head appropriately
		  	   this.snakeArr[this.snakeArr.length-1]=this.board.children[leaderCurrIndex + (gridNum)];
		   }
  	  	}else if(this.vel[0]==1 && this.vel[1]==0){//moving forward
			  if (((leaderCurrIndex + 1)%(gridNum))==0){
			hitWall = true;
			  }else{
		  		this.snakeArr[this.snakeArr.length-1]=this.board.children[leaderCurrIndex + 1];
		  }
  	  	}else if (this.vel[0]==0 && this.vel[1]==-1){//moving upward
			  if ((leaderCurrIndex - gridNum)<0){
	  			hitWall = true;
			  }else{
				  this.snakeArr[this.snakeArr.length-1]=this.board.children[leaderCurrIndex - (gridNum)];
			}
  	  	}else if (this.vel[0]==-1 && this.vel[1]==0){ //moving backward
				if ((leaderCurrIndex%gridNum)==0){
				  hitWall = true;
				}else{
		  	   this.snakeArr[this.snakeArr.length-1]=this.board.children[leaderCurrIndex -1];
		   }
  	  	}
		if(hitWall){//if the wall is hit call pauseHandler with the followinf message to display
		  document.getElementById('notf').innerHTML = `Game Over`;
		  document.getElementById('msg').innerHTML = `After eaten ${this.currPt} fruits you bit the wall `;
		  document.getElementById('comment').innerHTML = `Press Play to Replay?`;
		  in_game = null; //set in game to false;
  			  PauseHandler(true);
  			  return;
		}
		//check if the head of the snake about to collide with it body
			for(let i=0; i<this.snakeArr.length-1;i++){//loop through the body of the snake
			  if (this.snakeArr[i] == this.snakeArr[this.snakeArr.length-1]){//if the body part and head in same grid, then game over, call pauseHandler with the followinf message to display
			document.getElementById('notf').innerHTML = `Game Over`;
			document.getElementById('msg').innerHTML = `After eaten ${this.currPt} fruits you bit yourself `;
			document.getElementById('comment').innerHTML = `Press Play to Replay?`;
			in_game = null;
				  PauseHandler(true);
				  return;
			  }
			}
		//check if the head of the snake about to collide with it the food
  	  	if (this.snakeArr[this.snakeArr.length-1] == this.food){//if food eaten
				this.snakeArr.push(this.food);
				this.food.removeAttribute("id");//remove the style of the food from the grid
		  this.currPt += 1; //update player Pt
		  this.pointEl.textContent = `${this.currPt}`;
				this.food=null;//set food to null
		  }
			this.snakeArr[this.snakeArr.length-1].setAttribute('id','leader');//style and color the new snake head
	  	}
	  	UpdateBody(){
		//move the body of the snake / no regard for the snake head(handled in the update leader method)
	  		for(let i=0; i<this.snakeArr.length-1;i++){
				this.snakeArr[i] = this.snakeArr[i+1];//move the elements of the snakeArr to the element in it front (index)
			  this.snakeArr[i].removeAttribute("id");//remove head style if neccesary(see line 240)
		  }
  	  }
	  	GenFood(){
		//radomly generate a food in random place
		if (this.food===null){
		  while(true){//Dangerous!!!
			let randNum = Math.floor(Math.random()*this.board.children.length);
			this.food = this.board.children[randNum];
			if (!this.snakeArr.includes(this.food)){
			  this.food.setAttribute("id","food");//style the food grid
			  return true;
			}
		  }
		}
	  	}
	  }
	  function ButtonHandler(vec){
	  if (document.getElementById("prompt").style.display!="none"){
		return;
	  }
  	  if(snakeTimer==null){//jump start the game and wake the snake
  		  snakeTimer = setInterval(function(){
  	  	snake.UpdateBody();
  	  	snake.UpdateLeader();
  	  	snake.Display();
  	  	snake.GenFood();},200);
  	  }
	  	try{//ignore all errors and bugs(Lazy!)
  		  if ((snake.vel[0]!= vec[0])&&(snake.vel[1]!= vec[1])){
				snake.vel = vec;
				return;
  		  }
	  	}catch(err){
	  		return false;
	  	}
	  }
	  function PlayHandler(){
  		document.getElementById("prompt").style.display="none";
	  document.getElementById("botton_cont").style.visibility="visible";
	  if(in_game==null){//starting a new game
		for(let _blk of document.getElementsByClassName('snakeBody')){
		  _blk.removeAttribute('class');
		}
		if (document.getElementById('leader')){
		  document.getElementById('leader').removeAttribute('id');
		}
		if (document.getElementById('food')){
		  document.getElementById('food').removeAttribute('id');
		}
		snake = new Game();
		snake.Display();
	  }
	  }
	  function PauseHandler(msg){//show the menu and stop snake and game timer
	  document.getElementById("prompt").style.display="block";
	  document.getElementById("botton_cont").style.visibility="hidden";
	  if (msg==null){
		document.getElementById('notf').innerHTML = `Game Paused`;
		document.getElementById('msg').innerHTML = `You've eaten ${snake.currPt} fruit so far and you have 1 life left`;
		document.getElementById('comment').innerHTML = `Press Play to Resume?`;
	  }
	  clearInterval(snakeTimer);
	  snakeTimer =null;
	  }
	function keyHandler(){
	  switch (event.key) {
		case "ArrowUp":
		  ButtonHandler([0,-1]);
		  break;
		case "ArrowDown":
		  ButtonHandler([0,1]);
		  break;
		case "ArrowRight":
		  ButtonHandler([1,0]);
		  break;
		case "ArrowLeft":
		  ButtonHandler([-1,0]);
		  break;
	  }
	}
	</script>
  </body>

</html>

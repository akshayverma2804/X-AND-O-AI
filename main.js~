//**************************//

function Vector(x, y) {
    this.x = x;
    this.y = y;
}

Vector.prototype.equal = function(othervector) {
    return (this.x == othervector.x)&&(this.y == othervector.y)
};

function Actor(pos, type) {
    this.pos = pos;
//    if(type)
//        this.type = type;
    this.type = type;
    this.status = null;
}

function newGame() {
    var grid = [];
    for(var i = 0; i < 3; i++) {
        var gridLine = [];
        for(var j = 0; j < 3; j++)
            gridLine.push(new Actor(new Vector(i, j)));
        grid.push(gridLine);
    }
    return grid;
}

var flag = localStorage.getItem('mode');
var grid = newGame();
///**********************//

function elt(name, className) {
    var elt = document.createElement(name);
    if(className)
        elt.className = className;
    return elt;
}

function posToString(posCo) {
    if(!posCo )
        return 'one';
    else if(posCo == 1)
        return 'two';
    return 'three'
}

function gridToVector(target) {
    var x, y;
    var rowParent = target.parentNode;

    function switchName(elt) {
        var i;
        switch (elt.className) {
            case 'one': i = 0;
                break;
            case 'two': i = 1;
                break;
            case 'three': i = 2;
        }
        return i;
    }

    x = switchName(target.parentNode);
    y = switchName(target);

    return new Vector(x, y);
}

function drawGame() {
    var table = elt('table', 'background');

    var count = 0;
    grid.forEach(function(row){
        var rowName = posToString(row[count].pos.x);
        var rowElt = table.appendChild(elt('tr', rowName));

        row.forEach(function(actor) {
            var colName = posToString(actor.pos.y);
            rowElt.appendChild(elt('td', colName));
        });

        count++;
    });

    return table;
};

document.getElementById('game').appendChild(drawGame());

//***************//

function end(cc) {
    var whoWin = playerWin();
    if(whoWin == 'X') {
        restart('You Win!');
        return true;
    }
    else if(whoWin == 'O') {
        restart('Game over!');
        return true;
    }
    else if(cc.length == 9 ) {
        restart("It's a tie!");
        return true;
    }
    return false;
}

function playerWin() {
    var ch = 0;

    //check row
    for(var i = 0; i < 3; i++) {
        var gType = grid[i][0].type;
        var m = 0;
        if(gType) {
            for(var j = 0; j < 3; j++)
                if(gType == grid[i][j].type)
                    m++;
        }
        if(m == 3) {
            ch = gType;
            break;
        }
    }

    //check col
    if(!ch)
        for(var i = 0; i < 3; i++) {
            var gType = grid[0][i].type;
            if (gType) {
                var j = 0;
                while (gType == grid[j][i].type) {
                    j++;
                    if (j == 3) {
                        ch = gType;
                        break;
                    }
                }
            }
        }

    //check diagonal
    if(!ch) {
        var gType = grid[1][1].type;
        if((grid[0][0].type == gType && grid[2][2].type == gType)
            || (grid[0][2].type == gType && grid[2][0].type == gType))
            ch = gType;

    }

    return ch;
}

function restart(heading) {
    var divEnd = document.getElementsByTagName('div')[1];

    divEnd.className = 'end';

    var h = elt('h3');
    h.textContent = heading;

    var res = elt('a');

    res.setAttribute('href', 'home.html');
    res.textContent = 'Restart';
    
    divEnd.appendChild(h);
    divEnd.appendChild(res);				
}

function xPlay() {
    var table = document.getElementById('game');

    table.addEventListener('click', function(event) {
        var cell = event.target;
        var xPos = gridToVector(cell);

        cell.className += ' clicked';
        var xIcon = elt('i', 'fa fa-times');
        cell.appendChild(xIcon);

        //grid[xPos.x][xPos.y] = findActorWithPos(xPos);
//        grid[xPos.x][xPos.y].type = 'X';
        grid[xPos.x][xPos.y] = new Actor(xPos, 'X');
        grid[xPos.x][xPos.y].status = 'clicked';

        setTimeout(oPlay, 500);
    })

}

function O() {
    var x = Math.floor(Math.random() * 3);
    var y = Math.floor(Math.random() * 3);
    return new Actor(new Vector(x, y), 'O');
}

var nodesvis;


////////////////////////////////////////////////
/////////@akshayverma
////////////////////////////////////////////////
var firstMsg = true;

function oPlay() {
    var clickedCell = [];
    for(var i = 0; i < 3; i++) {
        for(var j = 0; j < 3; j++) {
            if(grid[i][j].status == 'clicked')
                clickedCell.push(grid[i][j]);
        }
    }

    //first check
    if(end(clickedCell))
        return;
 
    var nextO;
    if (flag=='H'){
	//alert("Hard");
	nextO = findNextt();	
    }
    if (flag=='E'){
	//alert("Easy");
        nextO = findNext();
    }
    if(!nextO) {
        nextO = new O();
        for(var k = 0; ; k++) {
            if((nextO.pos).equal(clickedCell[k].pos)) {
                nextO = new O();
                k = -1;
            }
            else if (k == clickedCell.length - 1)
                break;
        }
    }

    grid[nextO.pos.x][nextO.pos.y] = nextO;
    grid[nextO.pos.x][nextO.pos.y].status = 'clicked';

    var cssPos = ' .' + posToString(nextO.pos.x) + ' .' + posToString(nextO.pos.y);
    var cell = document.querySelector(cssPos);

    cell.className += ' comClick';
    var oIcon = elt('i', 'fa fa-circle-o');
    cell.appendChild(oIcon);

    //second check
    if(end(clickedCell))
        return;

}

function findNext() {
    var newO;

    grid.forEach(function(row) {
        var fr = row.filter(function(actor) {
            return (actor.type == 'X');
        });
        if(fr.length == 2) {
            row.forEach(function(actor) {
                if(actor.status != 'clicked')
                    newO = actor;
                //return new Actor(actor.pos, 'O');
            })
        }
    });

    for(var i = 0; i < 3; i++) {
        var actor;
        var m = 0, n = 0;
        for(var j = 0; j < 3; j++) {
            if(grid[j][i].status != 'clicked') {
                actor = grid[j][i];
                m++;
            }
            if(grid[j][i].type == 'X')
                n++;
        }
        if(m == 1 && n == 2)
            newO = actor;
    }

    var firstDia = [grid[0][0], grid[1][1], grid[2][2]];
    var secondDia = [grid[0][2], grid[1][1], grid[2][0]];
    var dia = [firstDia, secondDia];

    dia.forEach(function(d) {
        var fr = d.filter(function(actor) {
            return (actor.type == 'X');
        });
        if(fr.length == 2) {
            d.forEach(function(actor) {
                if(actor.status != 'clicked')
                    newO = actor;
            })
        }
    });

    var Msg = "Computer Easy:- I don't know whats going on, I'll just make random move and see if I can win!";
    document.getElementById('comment').innerHTML = Msg;	     
	
    if(newO) newO.type = 'O';
    return newO;
}

////////////////////////////////
//////////@author = akshayverma
function winning(a,type){
    //check hor rows
    for (var i=0; i<3; i++){
        var res = true;
        for (var j=0; j<3; j++){
            if (a[i][j]!=type){
                res = false; break;
            }
        }
        if (res==true) {return true;}
    }
    //check ver cols
    for (var i=0; i<3; i++){
        var res = true;
        for (var j=0; j<3; j++){
            if (a[j][i]!=type){
                res = false; break;
            }
        }
        if (res==true) {return true;}
    }
    //check two dias
    var left = true;
    var right = true;
    for (var i=0; i<3; i++){
        if (a[i][i]!=type){left = false;}
        if (a[i][2-i]!=type){right = false;}
    }
    if (left==true){return true;}
    if (right==true){return true;}
    return false;
}

function draw(a,type){
    for (var i=0; i<3; i++){
        for (var j=0; j<3; j++){
            if (a[i][j]==0){return false;}
        }
    }
    var flag = winning(a,type);
    if (flag==true){return false;}
    return true;
}

function losing(a,type){
    var nextype = type;
    nextype = nextype*(-1);
    return winning(a,nextype);
}
//////////////////////////////////////////////
function findNextt() {
    dgrid = [];
    dgrid.push([0,0,0]);
    dgrid.push([0,0,0]);
    dgrid.push([0,0,0]);
    nodesvis = 0;
    
    for (var i=0; i<3; i++){
      for (var j=0; j<3; j++){
	if (grid[i][j].type=='X'){
	  dgrid[i][j] = 1;
	}
	if (grid[i][j].type=='O'){
	  dgrid[i][j] = -1;
	}
      }
    }
    var ret = findMove(dgrid,-1);
    var stat = ret.x;
    var Msg = "";	
    if (stat==1){
        Msg = "Computer Hard:- You had an equal position, but you are losing now!";
    }	
    if (stat==0){
        Msg = "Computer Hard:- Position is equal right now, You can draw if you play optimally!"; 
	}
    if (stat==-1){
        Msg = "Computer Hard:- I can't believe, I am losing to a human!";
	}
    document.getElementById('comment').innerHTML = Msg;	
    var move = ret.y;
    var x = Math.floor(move/3);
    var y = move%3;
    return new Actor(new Vector(x, y), 'O');
}

function findMove(a,tomove){
    nodesvis++;
    if (winning(a,-1)){
        return new Vector(1,-1);
    }
    if (losing(a,-1)){
        return new Vector(-1,-1);
    }
    if (draw(a,-1)){
        return new Vector(0,-1);
    }

    var best = -1;
    var nbest = -1;
    var gstat = -1;
    if (tomove==1){
        gstat = 1;
    }
    for (var i=0; i<9; i++){
	//console.log("Nodesvisited, i " + nodesvis +" "+ i);	
	var na = a.map(function(arr) {
  		 return arr.slice();
		});
	var xi = Math.floor(i/3);
	var yj = i%3;
	if (na[xi][yj]!=0){continue;}
        na[xi][yj] = tomove;
        
	//console.log("minimax called");
	var nexttomove = tomove*(-1);
        var getstatus = findMove(na, nexttomove);
        var status = getstatus.x;
	//console.log("returnr s with stat = "+status);
	
        if (status==1){
            if (tomove==-1){
                best = i;
                gstat = 1;
            }
            continue;
        }
        if (status==0){
            if (tomove==-1){
                if (nbest==-1){
                    nbest = i;
                }
                if (gstat<0){
                    gstat = 0;
                }
            }
            if (tomove==1){
                if (gstat>0){
                    gstat = 0;
                }
            }
            continue;
        }
        if (status==-1){
            if (tomove==1){
                gstat = -1;
            }
            continue;
        }
    }
    
    if (gstat==1){
        return new Vector(1,best);
    }
    if (gstat==0){
        if (best!=-1){
            nbest = best;
        }
        return new Vector(0,nbest);
    }
    return new Vector(-1,-1);
}

///////@authclose
/////////////////////////////////

function runGame() {
    xPlay();
    //tomove = 1;	
    //bothPlay();
}

runGame();
//////////////////////////////////////////////////////////////////////////////

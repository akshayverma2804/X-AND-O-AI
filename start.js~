start();

function elt(name, className) {
    var elt = document.createElement(name);
    if(className)
        elt.className = className;
    return elt;
}

function start(){
    var divEnd = document.getElementsByTagName('div')[1];

    divEnd.className = 'end';

    var h = elt('h3');
    h.textContent = "Hello! ";

    var res = elt('a');

    res.textContent = 'Play Computer easy';
    res.setAttribute('href', 'index.html'); 
    res.setAttribute('display', 'block');		
    res.addEventListener('click', function(event) {
	localStorage.setItem('mode',"E");
    })	

    var res1 = elt('a');

    res1.textContent = 'Play Computer hard';
    res1.setAttribute('href', 'index.html'); 
    res1.setAttribute('display', 'block');		
    res1.addEventListener('click', function(event) {
	localStorage.setItem('mode',"H");
    })	

	
 
    divEnd.appendChild(h);
    divEnd.appendChild(res);
    for (var i=0; i<3; i++){
         var brk1 = elt('br');
	divEnd.appendChild(brk1);
    }		
    divEnd.appendChild(res1);		
    return true;		
}

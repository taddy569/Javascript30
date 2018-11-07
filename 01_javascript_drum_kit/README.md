Day 1: JavaScript Drum Kit

- We have HTML and CSS file (index.html - style.css), also with sounds folder
	HTML

		+ data-key attribute: data-* is a HTML Global Attributes (like id or class, ...), used to store custome data private to the page. At this case, data-key attribute of div tag and audio tag is store key code corresponds of button
		+ kbd tag is a phrase tag, defines keyboard input
		+ audio: if you want audio display in page, just add controls attribute in the same place with src attribute
	         <audio controls src="source-url"></audio>.
	CSS
		+ playing class is defined with some rules in css file, but it isn't used at the first time when browser load the page, it will be added to class attribute of correct element base on keyDown event (for showing user that what button they've press and playing now,  after the sound is play, playing class is removed from element


Step by step guide

- Step 1: assign handler function to onload property of window object, it named init
	window.addEventListener('load', init);
or	window.onload = init;

- Step 2: in init function, there are two main steps we need to do
	+ tell the browser know when the correct key is pressed and how to handler this event  with playSound function
		window.addEventListener('keydown', playSound);
	+ tell the browser know when the transition event of div element is end with removeTransition handle function. 
		We have 9 div element corresponds with 9 sounds in sounds folder, we need all of this have this handle
		const divs = Array.from(document.querySelectorAll('.key')); // select elements which have class="key"
			notice: querySelectorAll is different with querySelector (return first element that matches a specified CSS selector)
		divs.forEach(function(div) {
			div.addEventListener('transitionend', removeTransition);
		}) // for each div element of divs array, we add the transitionend event with removeTransition handler function

- Step 3: declare helper functions (we have 2 functions: playSound and removeTransition)
	+ function playSound(event) { // event parameter: let the function know what the event object is called when event happend
		const audio = document.querySelector(`audio[data-key="${event.keyCode}"]`);
		if(!audio) {
			return;
		}
		const div = document.querySelector(`div[data-key="${event.keyCode}"]`);
		div.classList.add('playing');
		audio.currentTime = 0;
		audio.play();
	}

	+ function removeTransition(event) {
		if(event.propertyName !== "transform") {
			return;
		}
		event.target.classList.remove('playing');
	}

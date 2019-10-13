// ==UserScript==
// @name         Thue-Morse
// @namespace    https://online-go.com/*
// @version      r1
// @description  Indicate Thue Morse Value
// @author       Vsotvep
// @match        https://online-go.com/*
// @grant        none
// ==/UserScript==

// (function() {
//     'use strict';

    var THUE_MORSE_GAME_LIST = ["19784219", "19784231"]; // put the game numbers in this list (see the url for the number)

var i, j, k=[], m, n=[1,0], c=1;
for (i=0; i<9; i++) {
	m = [];
	for (j=0; j<n.length; j++) { m[j] = 1-n[j]; }
	n = n.concat(m);
}
for (i=0; i<n.length; i++) {
	if (n[i]%2 == c%2) { k[c++] = (n[i] == 1 ? "B" : "W" ); }
	else {
		k[c++] = "PASS";
		k[c++] = (n[i] == 1 ? "B" : "W" );
	}
}

setInterval(function() {
	var thueMorse;
	if (THUE_MORSE_GAME_LIST.includes(location.href.match(/\d+/)[0])) {
		thueMorse = document.getElementById("thueMorse");
		if (thueMorse == null) {
			var chatContainer = document.querySelector('.chat-container');
			thueMorse = document.createElement("div");
			thueMorse.id = "thueMorse";
			thueMorse.style.height = "48px";
			chatContainer.parentNode.insertBefore(thueMorse, chatContainer);
		}
		var moveNumber = parseInt(document.querySelector('.move-number').innerHTML.match(/\d+/)[0]);

		thueMorse.innerHTML = (moveNumber == 0 ? "-" : k[moveNumber]) + " << " + k[moveNumber+1] + " >> (";

		for (i=2; i<0x22; i++) {
            if (i != 0x21) {
                thueMorse.innerHTML = thueMorse.innerHTML + ( k[moveNumber + i] != "PASS" ? k[moveNumber + i] + " " : "" );
            } else {
                thueMorse.innerHTML = thueMorse.innerHTML + ( k[moveNumber + i] != "PASS" ? k[moveNumber + i] : "" );
            }
        }

        thueMorse.innerHTML = thueMorse.innerHTML + ")";

	} else {
		 thueMorse = document.getElementById("thueMorse");
		 if (thueMorse != null) { thueMorse.parentNode.removeChild(thueMorse); }
	}
},200);

// })();

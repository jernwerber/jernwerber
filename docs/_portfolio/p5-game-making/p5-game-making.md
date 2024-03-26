---
title: "Learning JavaScript and p5js through Game-making: Tutorial sketch"
permalink: /portfolio/p5-game-making
layout: page
excerpt_separator: <!--more-->
# p5: true
---
Game-making is a great way to introduce someone to programming because a lot of easy-to-understand game concepts can be used to provide a structure for learning. Adding each feature naturally decomposes a larger problem into manageable chunks, and with some creative planning, features can be arranged to provide a progression through basic coding concepts.
<!--more-->

This portfolio entry showcases a demo I created for use with a Javascript- and p5-based learning experience that introduced foundational concepts against a backdrop of game-making. The demo had to be simple enough such that learners could identify the individual components, but complex enough to require the application of multiple coding concepts. 

_Click to activate sketch_ 👇
<div id="sketch-container" style="display:grid;grid-template-columns:auto auto;padding:10px" markdown=1>
![Screenshot from a sample game written in JavaScript and p5](/portfolio/p5-game-making/p5-game-making-cover.png){: #game-cover style="display:block;max-width:100%;width:400px;filter:blur(3px);"}
</div>
<script src="https://cdn.jsdelivr.net/gh/jernwerber/js-sketches@main/p5-game-making/spicy-visitors.js"></script>
<script>
    (function() { 
        let sc = document.createElement("script");
        sc.setAttribute('src','https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.5.0/p5.js');
        sc.onload = () => {
            // let start = document.createElement("script");
            // start.innerHTML = `new p5(s, "sketch-container")`;
            // start.setAttribute('defer','defer');
            // document.body.appendChild(start);
            document.getElementById("game-cover").addEventListener('click', () => {
                document.getElementById("game-cover").parentElement.remove();
                new p5(s, "sketch-container");
            })
            let sk = document.getElementById("sketch-container");
            function interceptKeys(e) {
                if([32, 37, 38, 39, 40].indexOf(e.keyCode) > -1) {
                    e.preventDefault();
                    console.log(`keycode ${e.keyCode} intercepted`)
                    }}
            sk.addEventListener("focus", () => {
                window.addEventListener("keydown", interceptKeys, false);
                sk.addEventListener("blur", ()=> {
                    window.removeEventListener("keydown", interceptKeys, false)
                }, false);
            }, false);
            }                           
        document.body.appendChild(sc);       
    })();
</script>

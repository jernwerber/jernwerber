---
title: "Game-making tutorial sketch with p5js"
permalink: /portfolio/p5-game-making
layout: page
# p5: true
---

<div id="sketch-container" style="display:grid;grid-template-columns:auto auto;padding:10px" markdown=1>
![Screenshot from a sample game written in JavaScript and p5](/portfolio/p5-game-making/p5-game-making-cover.png){: #game-cover width=400 style="display:block;max-width:100%;"}
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
                document.getElementById("game-cover").remove();
                new p5(s, "sketch-container");
            })
        }
        document.body.appendChild(s);       
    })();
</script>

---
title: "Game-making tutorial sketch with p5js"
permalink: /portfolio/p5-game-making
layout: page
# p5: true
---

<div id="sketch-container" style="padding:10px">
</div>
<script src="https://cdn.jsdelivr.net/gh/jernwerber/js-sketches@latest/p5-game-making/spicy-visitors.js"></script>
<script>
    (function() { 
        let s = document.createElement("script");
        s.setAttribute('src','https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.5.0/p5.js');
        s.onload = () => {
            let start = document.createElement("script");
            start.innerHTML = `new p5(s, "sketch-container")`;
            start.setAttribute('defer','defer');
            document.body.appendChild(start);
        }
        document.body.appendChild(s);       
    })();
</script>

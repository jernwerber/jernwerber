---
title: "Game-making tutorial sketch with p5js"
permalink: /portfolio/p5-game-making
layout: page
# p5: true
---

<div id="sketch-container" style="padding:10px">
</div>
<script src="https://cdn.jsdelivr.net/gh/jernwerber/js-sketches@latest/p5-game-making/spicy-visitors.js"></script>
<script defer>
    (function() { 
        let s = document.createElement("script");
        s.setAttribute('src','https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.5.0/p5.js');
        document.body.appendChild(s);
    })();
    new p5(s, "sketch-container");
</script>

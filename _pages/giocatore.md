---
title: "Giocatore"
permalink: /giocatore/
author_profile: true
---

<div id="player-page">
  <h2 id="player-name"></h2>

  <p><strong>Nome:</strong> <span id="player-nome"></span></p>
  <p><strong>Braccio:</strong> <span id="player-braccio"></span></p>
  <p><strong>Classe:</strong> <span id="player-anno"></span></p>
  <p><strong>Genere:</strong> <span id="player-genere"></span></p>

  <h3>Descrizione</h3>
  <p id="player-descrizione"></p>
</div>

<script>
  const players = {{ site.data.players | jsonify }};

  const params = new URLSearchParams(window.location.search);
  const id = params.get("id");

  const player = players.find(p => p.slug === id);

  if (player) {
    document.getElementById("player-name").textContent = player.nome;
    document.getElementById("player-nome").textContent = player.nome;
    document.getElementById("player-braccio").textContent = player.braccio;
    document.getElementById("player-anno").textContent = player.anno;
    document.getElementById("player-genere").textContent = player.genere;
    document.getElementById("player-descrizione").textContent = player.descrizione;
  } else {
    document.getElementById("player-page").innerHTML = "<p>Giocatore non trovato.</p>";
  }
</script>

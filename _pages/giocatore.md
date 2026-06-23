---
title: "Giocatore"
permalink: /giocatore/
author_profile: true
---

<div id="player-page">

<div style="float:right; margin-left:20px; margin-bottom:20px;">
<img id="player-foto"
     src=""
     alt=""
     style="width:200px; border-radius:10px; display:none;">
</div>

<h2 id="player-name"></h2>

<p style="margin:2px 0;"><strong>Nome:</strong> <span id="player-nome"></span></p>
<p style="margin:2px 0;"><strong>Braccio:</strong> <span id="player-braccio"></span></p>
<p style="margin:2px 0;"><strong>Classe:</strong> <span id="player-anno"></span></p>
<p style="margin:2px 0;"><strong>Genere:</strong> <span id="player-genere"></span></p>

<h3>Descrizione:</h3>
<p id="player-descrizione"></p>

<h3>Statistiche complessive</h3>

<table>
<tr><td><strong>Edizioni giocate</strong></td><td id="stat-edizioni"></td></tr>
<tr><td><strong>Partite giocate</strong></td><td id="stat-partite"></td></tr>
<tr><td><strong>Vittorie</strong></td><td id="stat-vittorie"></td></tr>
<tr><td><strong>Pareggi</strong></td><td id="stat-pareggi"></td></tr>
<tr><td><strong>Sconfitte</strong></td><td id="stat-sconfitte"></td></tr>
<tr><td><strong>Percentuale vittorie</strong></td><td id="stat-percentuale-vittorie"></td></tr>
<tr><td><strong>Game vinti</strong></td><td id="stat-game-vinti"></td></tr>
<tr><td><strong>Game giocati</strong></td><td id="stat-game-giocati"></td></tr>
<tr><td><strong>Percentuale game vinti</strong></td><td id="stat-percentuale-game-vinti"></td></tr>
<tr><td><strong>Differenza game</strong></td><td id="stat-differenza-game"></td></tr>
</table>

<h3>Partite disputate</h3>
<div id="storico-partite"></div>

</div>

<script>
const players = {{ site.data.players | jsonify }};
const editions = {{ site.data.editions | jsonify }};

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

  if (player.foto) {
    const img = document.getElementById("player-foto");
    img.src = "/images/players_images/" + player.foto;
    img.alt = player.nome;
    img.style.display = "block";
  }

  let stats = {
    edizioni: new Set(),
    partite: 0,
    vittorie: 0,
    pareggi: 0,
    sconfitte: 0,
    gameVinti: 0,
    gamePersi: 0
  };

  let storicoHtml = "";

  editions.forEach(edition => {
    let partiteEdizione = [];

    Object.entries(edition.partite).forEach(([fase, partiteFase]) => {
      partiteFase.forEach(match => {
        const isPlayer1 = match.giocatore_1 === id;
        const isPlayer2 = match.giocatore_2 === id;

        if (isPlayer1 || isPlayer2) {
          stats.edizioni.add(edition.slug);
          stats.partite++;

          const opponentSlug = isPlayer1 ? match.giocatore_2 : match.giocatore_1;
          const opponent = players.find(p => p.slug === opponentSlug);

          const gameFatti = isPlayer1 ? match.game_1 : match.game_2;
          const gameSubiti = isPlayer1 ? match.game_2 : match.game_1;

          stats.gameVinti += gameFatti;
          stats.gamePersi += gameSubiti;

          let risultato = "";

          if (gameFatti > gameSubiti) {
            stats.vittorie++;
            risultato = "Vittoria";
          } else if (gameFatti < gameSubiti) {
            stats.sconfitte++;
            risultato = "Sconfitta";
          } else {
            stats.pareggi++;
            risultato = "Pareggio";
          }

          partiteEdizione.push(`
<tr>
<td>${fase}</td>
<td>${opponent ? opponent.nome : opponentSlug}</td>
<td>${gameFatti}-${gameSubiti}</td>
<td>${risultato}</td>
</tr>
          `);
        }
      });
    });

    if (partiteEdizione.length > 0) {
      storicoHtml += `
<h4>${edition.nome}</h4>
<table>
<thead>
<tr>
<th>Fase</th>
<th>Avversario</th>
<th>Risultato</th>
<th>Esito</th>
</tr>
</thead>
<tbody>
${partiteEdizione.join("")}
</tbody>
</table>
      `;
    }
  });

  document.getElementById("stat-edizioni").textContent = stats.edizioni.size;
  document.getElementById("stat-partite").textContent = stats.partite;
  document.getElementById("stat-vittorie").textContent = stats.vittorie;
  document.getElementById("stat-pareggi").textContent = stats.pareggi;
  document.getElementById("stat-sconfitte").textContent = stats.sconfitte;
     const percentualeVittorie = stats.partite > 0
       ? ((stats.vittorie / stats.partite) * 100).toFixed(1)
       : "0.0";
     
     document.getElementById("stat-percentuale-vittorie").textContent =
       percentualeVittorie + "%";
  document.getElementById("stat-game-vinti").textContent = stats.gameVinti;
  document.getElementById("stat-game-giocati").textContent = stats.gameVinti + stats.gamePersi;
     const gameGiocati = stats.gameVinti + stats.gamePersi;
     
     const percentualeGameVinti = gameGiocati > 0
       ? ((stats.gameVinti / gameGiocati) * 100).toFixed(1)
       : "0.0";
     
     document.getElementById("stat-percentuale-game-vinti").textContent =
       percentualeGameVinti + "%";
  document.getElementById("stat-differenza-game").textContent = stats.gameVinti - stats.gamePersi;

  document.getElementById("storico-partite").innerHTML =
    storicoHtml || "<p>Nessuna partita registrata.</p>";

} else {
  document.getElementById("player-page").innerHTML =
    "<p>Giocatore non trovato.</p>";
}
</script>

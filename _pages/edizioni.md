---
title: "Edizioni"
permalink: /edizioni/
author_profile: true
---

<h2>Edizioni del Filippi Slam</h2>

<div id="edizioni-list"></div>

<script>
  const players = {{ site.data.players | jsonify }};
  const editions = {{ site.data.editions | jsonify }};

  let html = "";

  editions.forEach(edition => {
    html += `
      <h3>${edition.nome}</h3>
      <p><strong>Data:</strong> ${edition.data}</p>
      <p><strong>Luogo:</strong> ${edition.luogo}</p>

      <table>
        <thead>
          <tr>
            <th>Giocatore 1</th>
            <th>Giocatore 2</th>
            <th>Risultato</th>
          </tr>
        </thead>
        <tbody>
    `;

    edition.partite.forEach(match => {
      const p1 = players.find(p => p.slug === match.giocatore_1);
      const p2 = players.find(p => p.slug === match.giocatore_2);

      html += `
        <tr>
          <td><a href="/giocatore/?id=${match.giocatore_1}">${p1 ? p1.nome : match.giocatore_1}</a></td>
          <td><a href="/giocatore/?id=${match.giocatore_2}">${p2 ? p2.nome : match.giocatore_2}</a></td>
          <td>${match.game_1}-${match.game_2}</td>
        </tr>
      `;
    });

    html += `
        </tbody>
      </table>
    `;
  });

  document.getElementById("edizioni-list").innerHTML = html;
</script>

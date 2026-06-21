---
title: "Edizione"
permalink: /edizione/
author_profile: true
---

<div id="edition-page"></div>

<script>
  const players = {{ site.data.players | jsonify }};
  const editions = {{ site.data.editions | jsonify }};

  const params = new URLSearchParams(window.location.search);
  const id = params.get("id");

  const edition = editions.find(e => e.slug === id);

  function getPlayerName(slug) {
    const player = players.find(p => p.slug === slug);
    return player ? player.nome : slug;
  }

  if (edition) {
    let html = `
      <h2>${edition.nome}</h2>

      <p><strong>Data:</strong> ${edition.data}</p>
      <p><strong>Luogo:</strong> ${edition.luogo}</p>
      <p><strong>Numero partecipanti:</strong> ${edition.partecipanti ? edition.partecipanti.length : 0}</p>

      <h3>Partecipanti</h3>
      <ul>
    `;

    edition.partecipanti.forEach(slug => {
      html += `
        <li>
          <a href="/giocatore/?id=${slug}">
            ${getPlayerName(slug)}
          </a>
        </li>
      `;
    });

    html += `
      </ul>

      <h3>Partite</h3>

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
      html += `
        <tr>
          <td>
            <a href="/giocatore/?id=${match.giocatore_1}">
              ${getPlayerName(match.giocatore_1)}
            </a>
          </td>
          <td>
            <a href="/giocatore/?id=${match.giocatore_2}">
              ${getPlayerName(match.giocatore_2)}
            </a>
          </td>
          <td>${match.game_1}-${match.game_2}</td>
        </tr>
      `;
    });

    html += `
        </tbody>
      </table>
    `;

    document.getElementById("edition-page").innerHTML = html;

  } else {
    document.getElementById("edition-page").innerHTML =
      "<p>Edizione non trovata.</p>";
  }
</script>

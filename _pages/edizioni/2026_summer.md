---
title: "Filippi Slam Summer Edition 2025"
permalink: /edizioni/2026_summer/
author_profile: true
---

{% assign edition = site.data.editions | where: "slug", "2026_summer" | first %}

<h2>{{ edition.nome }}</h2>

<p style="margin: 2px 0;"><strong>Data:</strong> {{ edition.data }}</p>
<p style="margin: 2px 0;"><strong>Luogo:</strong> {{ edition.luogo }}</p>
<p style="margin: 2px 0;"><strong>Partecipanti:</strong> {{ edition.partecipanti.size }}</p>

<h3>Introduzione</h3>
<p>
Qui scrivi liberamente quello che vuoi su questa edizione.
</p>

<h3>Partecipanti</h3>

<ul style="margin:0; padding-left:20px;">
  {% for player_slug in edition.partecipanti %}
    {% assign player = site.data.players | where: "slug", player_slug | first %}
    <li style="margin-bottom:2px;">
      <a href="/giocatore/?id={{ player.slug }}">
        {{ player.nome }}
      </a>
    </li>
  {% endfor %}
</ul>

<h3>Fase a gironi</h3>

{% for fase in edition.partite %}
{% assign nome_fase = fase[0] %}
{% assign partite_fase = fase[1] %}

{% if nome_fase == "Girone A" or nome_fase == "Girone B" or nome_fase == "Girone C" %}
<h4>{{ nome_fase }}</h4>

<table>
<thead>
<tr>
<th>Giocatore 1</th>
<th>Giocatore 2</th>
<th>Risultato</th>
</tr>
</thead>
<tbody>
{% for match in partite_fase %}
{% assign p1 = site.data.players | where: "slug", match.giocatore_1 | first %}
{% assign p2 = site.data.players | where: "slug", match.giocatore_2 | first %}
<tr>
<td><a href="/giocatore/?id={{ p1.slug }}">{{ p1.nome }}</a></td>
<td><a href="/giocatore/?id={{ p2.slug }}">{{ p2.nome }}</a></td>
<td>{{ match.game_1 }}-{{ match.game_2 }}</td>
</tr>
{% endfor %}
</tbody>
</table>

<h4>Classifica {{ nome_fase }}</h4>

<table>
<thead>
<tr>
<th>Posizione</th>
<th>Giocatore</th>
<th>Punti</th>
<th>DG</th>
<th>GV</th>
</tr>
</thead>
<tbody id="classifica-{{ nome_fase | downcase | replace: ' ', '-' }}"></tbody>
</table>

<script>
(function() {
const players = {{ site.data.players | jsonify }};
const matches = {{ partite_fase | jsonify }};
const tableId = "classifica-{{ nome_fase | downcase | replace: ' ', '-' }}";

let stats = {};

matches.forEach(match => {
  [match.giocatore_1, match.giocatore_2].forEach(slug => {
    if (!stats[slug]) {
      stats[slug] = {
        slug: slug,
        punti: 0,
        gameVinti: 0,
        gamePersi: 0,
        vittorieContro: {}
      };
    }
  });

  const p1 = stats[match.giocatore_1];
  const p2 = stats[match.giocatore_2];

  p1.gameVinti += match.game_1;
  p1.gamePersi += match.game_2;
  p2.gameVinti += match.game_2;
  p2.gamePersi += match.game_1;

  if (match.game_1 > match.game_2) {
    p1.punti += 3;
    p1.vittorieContro[match.giocatore_2] = true;
  } else if (match.game_1 < match.game_2) {
    p2.punti += 3;
    p2.vittorieContro[match.giocatore_1] = true;
  } else {
    p1.punti += 1;
    p2.punti += 1;
  }
});

let classifica = Object.values(stats);

classifica.sort((a, b) => {
  if (b.punti !== a.punti) return b.punti - a.punti;

  if (a.vittorieContro[b.slug]) return -1;
  if (b.vittorieContro[a.slug]) return 1;

  const dgA = a.gameVinti - a.gamePersi;
  const dgB = b.gameVinti - b.gamePersi;
  if (dgB !== dgA) return dgB - dgA;

  return b.gameVinti - a.gameVinti;
});

const tbody = document.getElementById(tableId);

classifica.forEach((p, index) => {
  const player = players.find(x => x.slug === p.slug);
  const nome = player ? player.nome : p.slug;
  const dg = p.gameVinti - p.gamePersi;

  tbody.innerHTML += `
<tr>
<td>${index + 1}</td>
<td><a href="/giocatore/?id=${p.slug}">${nome}</a></td>
<td>${p.punti}</td>
<td>${dg}</td>
<td>${p.gameVinti}</td>
</tr>
  `;
});
})();
</script>
{% endif %}

{% endfor %}


{% if edition.partite["Semifinali"] %}
  <h3>Semifinali</h3>

  <table>
    <thead>
      <tr>
        <th>Giocatore 1</th>
        <th>Giocatore 2</th>
        <th>Risultato</th>
      </tr>
    </thead>
    <tbody>
      {% for match in edition.partite["Semifinali"] %}
        {% assign p1 = site.data.players | where: "slug", match.giocatore_1 | first %}
        {% assign p2 = site.data.players | where: "slug", match.giocatore_2 | first %}

        <tr>
          <td><a href="/giocatore/?id={{ p1.slug }}">{{ p1.nome }}</a></td>
          <td><a href="/giocatore/?id={{ p2.slug }}">{{ p2.nome }}</a></td>
          <td>{{ match.game_1 }}-{{ match.game_2 }}</td>
        </tr>
      {% endfor %}
    </tbody>
  </table>
{% endif %}

{% if edition.partite["Finale"] %}
  <h3>Finale</h3>

  <table>
    <thead>
      <tr>
        <th>Giocatore 1</th>
        <th>Giocatore 2</th>
        <th>Risultato</th>
      </tr>
    </thead>
    <tbody>
      {% for match in edition.partite["Finale"] %}
        {% assign p1 = site.data.players | where: "slug", match.giocatore_1 | first %}
        {% assign p2 = site.data.players | where: "slug", match.giocatore_2 | first %}

        <tr>
          <td><a href="/giocatore/?id={{ p1.slug }}">{{ p1.nome }}</a></td>
          <td><a href="/giocatore/?id={{ p2.slug }}">{{ p2.nome }}</a></td>
          <td>{{ match.game_1 }}-{{ match.game_2 }}</td>
        </tr>
      {% endfor %}
    </tbody>
  </table>
{% endif %}

---
title: "Filippi Slam Summer Edition 2025"
permalink: /edizioni/2026_summer/
author_profile: true
---

{% assign edition = site.data.editions | where: "slug", "2025_winter" | first %}

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

{% if nome_fase == "Girone A" %}
<h4>Girone A</h4>
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
<h4>Classifica Girone A</h4>

{% assign classifica = "" | split: "" %}

{% for match in partite_fase %}
{% assign p1_slug = match.giocatore_1 %}
{% assign p2_slug = match.giocatore_2 %}
{% assign g1 = match.game_1 %}
{% assign g2 = match.game_2 %}

<!-- Qui la classifica è meglio farla in JavaScript -->
{% endfor %}

<div id="classifica-girone-a"></div>

<script>
(function() {
  const players = {{ site.data.players | jsonify }};
  const matches = {{ partite_fase | jsonify }};

  let standings = {};

  matches.forEach(match => {
    const p1 = match.giocatore_1;
    const p2 = match.giocatore_2;
    const g1 = Number(match.game_1);
    const g2 = Number(match.game_2);

    if (!standings[p1]) standings[p1] = { slug: p1, punti: 0, dg: 0 };
    if (!standings[p2]) standings[p2] = { slug: p2, punti: 0, dg: 0 };

    standings[p1].dg += g1 - g2;
    standings[p2].dg += g2 - g1;

    if (g1 > g2) {
      standings[p1].punti += 3;
    } else if (g1 < g2) {
      standings[p2].punti += 3;
    } else {
      standings[p1].punti += 1;
      standings[p2].punti += 1;
    }
  });

  const ranking = Object.values(standings).sort((a, b) => {
    if (b.punti !== a.punti) return b.punti - a.punti;
    return b.dg - a.dg;
  });

  let html = `
<table>
<thead>
<tr>
<th>Posizione</th>
<th>Giocatore</th>
<th>Punti</th>
<th>DG</th>
</tr>
</thead>
<tbody>
`;

  ranking.forEach((row, index) => {
    const player = players.find(p => p.slug === row.slug);
    html += `
<tr>
<td>${index + 1}</td>
<td><a href="/giocatore/?id=${row.slug}">${player ? player.nome : row.slug}</a></td>
<td>${row.punti}</td>
<td>${row.dg}</td>
</tr>
`;
  });

  html += `
</tbody>
</table>
`;

  document.getElementById("classifica-girone-a").innerHTML = html;
})();
</script>
{% endif %}

{% if nome_fase == "Girone B" %}
<h4>Girone B</h4>
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

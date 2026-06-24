---
title: "Filippi Slam Winter Edition 2025"
permalink: /edizioni/2025_winter/
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
<tbody id="classifica-girone-a"></tbody>
</table>

<script>
(function() {
  const players = {{ site.data.players | jsonify }};
  const matches = {{ partite_fase | jsonify }};

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

  const tbody = document.getElementById("classifica-girone-a");

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
</script>{% endif %}

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
<h4>Classifica Girone B</h4>

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
<tbody id="classifica-girone-b"></tbody>
</table>

<script>
(function() {
  const players = {{ site.data.players | jsonify }};
  const matches = {{ partite_fase | jsonify }};

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

  const tbody = document.getElementById("classifica-girone-b");

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



<h3>Foto dell'edizione</h3>

{% assign edition_images = site.static_files | where_exp: "file", "file.path contains '/images/Edizioni/2025_winter/'" %}

<div class="edition-carousel">
  <button class="carousel-btn prev" onclick="changeSlide(-1)">&#10094;</button>

  <div class="carousel-image-wrapper">
    {% for image in edition_images %}
      <img class="carousel-slide"
           src="{{ image.path }}"
           alt="Foto Filippi Slam"
           style="display: {% if forloop.first %}block{% else %}none{% endif %};">
    {% endfor %}
  </div>

  <button class="carousel-btn next" onclick="changeSlide(1)">&#10095;</button>
</div>

<p id="carousel-counter" style="text-align:center; margin-top:8px;"></p>

<style>
.edition-carousel {
  position: relative;
  max-width: 700px;
  margin: 25px auto;
}

.carousel-image-wrapper {
  width: 100%;
  text-align: center;
}

.carousel-slide {
  width: 100%;
  max-height: 500px;
  object-fit: contain;
  border-radius: 10px;
}

.carousel-btn {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0,0,0,0.45);
  color: white;
  border: none;
  padding: 10px 14px;
  cursor: pointer;
  font-size: 22px;
  border-radius: 5px;
  z-index: 2;
}

.carousel-btn:hover {
  background: rgba(0,0,0,0.7);
}

.prev {
  left: 10px;
}

.next {
  right: 10px;
}
</style>

<script>
let currentSlide = 0;

function showSlide(index) {
  const slides = document.querySelectorAll(".carousel-slide");
  const counter = document.getElementById("carousel-counter");

  if (slides.length === 0) return;

  if (index >= slides.length) currentSlide = 0;
  if (index < 0) currentSlide = slides.length - 1;

  slides.forEach(slide => slide.style.display = "none");
  slides[currentSlide].style.display = "block";

  counter.textContent = `${currentSlide + 1} / ${slides.length}`;
}

function changeSlide(direction) {
  currentSlide += direction;
  showSlide(currentSlide);
}

document.addEventListener("DOMContentLoaded", function() {
  showSlide(currentSlide);
});
</script>

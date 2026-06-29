---
title: "Ranking"
permalink: /ranking/
author_profile: true
---

<div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:20px;">
  <h2>Ranking Filippi Slam</h2>

  <select id="ranking-filter" style="padding:6px 10px; border-radius:6px;">
    <option value="all" selected>Sempre</option>
    <option value="1">1 mese</option>
    <option value="6">6 mesi</option>
    <option value="12">1 anno</option>
    <option value="36">3 anni</option>
  </select>
</div>

<table id="ranking-table">
  <thead>
    <tr>
      <th>Pos.</th>
      <th>Giocatore</th>
      <th>Punti</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<script>
const players = {{ site.data.players | jsonify }};
const editions = {{ site.data.editions | jsonify }};

function parseItalianDate(dateString) {
  const mesi = {
    "gennaio": 0,
    "febbraio": 1,
    "marzo": 2,
    "aprile": 3,
    "maggio": 4,
    "giugno": 5,
    "luglio": 6,
    "agosto": 7,
    "settembre": 8,
    "ottobre": 9,
    "novembre": 10,
    "dicembre": 11
  };

  const parti = dateString.toLowerCase().split(" ");
  const giorno = parseInt(parti[0]);
  const mese = mesi[parti[1]];
  const anno = parseInt(parti[2]);

  return new Date(anno, mese, giorno);
}

function getPlayerName(slug) {
  const player = players.find(p => p.slug === slug);
  return player ? player.nome : slug;
}

function calculateRanking(months) {
  const today = new Date();
  let minDate = null;

  if (months !== "all") {
    minDate = new Date(today);
    minDate.setMonth(minDate.getMonth() - parseInt(months));
  }

  let ranking = {};

  editions.forEach(edizione => {
    if (!edizione.punti) return;

    const editionDate = parseItalianDate(edizione.data);

    if (minDate && editionDate < minDate) return;

    Object.entries(edizione.punti).forEach(([slug, punti]) => {
      if (!ranking[slug]) {
        ranking[slug] = {
          slug: slug,
          nome: getPlayerName(slug),
          punti: 0
        };
      }

      ranking[slug].punti += punti;
    });
  });

  return Object.values(ranking).sort((a, b) => b.punti - a.punti);
}

function renderRanking() {
  const filter = document.getElementById("ranking-filter").value;
  const ranking = calculateRanking(filter);
  const tbody = document.querySelector("#ranking-table tbody");

  tbody.innerHTML = "";

  ranking.forEach((giocatore, index) => {
    tbody.innerHTML += `
      <tr>
        <td>${index + 1}</td>
        <td>
          <a href="/giocatore/?id=${giocatore.slug}">
            ${giocatore.nome}
          </a>
        </td>
        <td><strong>${giocatore.punti}</strong></td>
      </tr>
    `;
  });

  if (ranking.length === 0) {
    tbody.innerHTML = `
      <tr>
        <td colspan="3">Nessun risultato disponibile per il periodo selezionato.</td>
      </tr>
    `;
  }
}

document.getElementById("ranking-filter").addEventListener("change", renderRanking);

renderRanking();
</script>

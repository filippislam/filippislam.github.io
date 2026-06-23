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

<ul>
  {% for player_slug in edition.partecipanti %}
    {% assign player = site.data.players | where: "slug", player_slug | first %}
    <li>
      <a href="/giocatore/?id={{ player.slug }}">
        {{ player.nome }}
      </a>
    </li>
  {% endfor %}
</ul>
<h3>Partiteee</h3>

{% if edition.partite["Girone A"] or edition.partite["Girone B"] %}
  <h2>Fase a gironi</h2>

  {% for nome_girone in "Girone A,Girone B" | split: "," %}
    {% assign partite_girone = edition.partite[nome_girone] %}

    {% if partite_girone %}
      <h3>{{ nome_girone }}</h3>

      <table>
        <thead>
          <tr>
            <th>Giocatore 1</th>
            <th>Giocatore 2</th>
            <th>Risultato</th>
          </tr>
        </thead>
        <tbody>
          {% for match in partite_girone %}
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
{% endif %}

{% if edition.partite["Semifinali"] %}
  <h2>Semifinali</h2>

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
  <h2>Finale</h2>

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

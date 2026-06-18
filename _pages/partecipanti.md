---
title: "Partecipanti"
permalink: /partecipanti/
author_profile: true
---

## Elenco Partecipanti

In questa sezione sono riportati tutti i partecipanti che hanno preso parte ad almeno un'edizione del Filippi Slam nel corso degli anni.<br>

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Braccio</th>
      <th>Classe</th>
      <th>Genere</th>
    </tr>
  </thead>
  <tbody>
    {% for player in site.data.players %}
    <tr>
      <td>
        <a href="/giocatore/?id={{ player.slug }}">
          {{ player.nome }}
        </a>
      </td>
      <td>{{ player.braccio }}</td>
      <td>{{ player.anno }}</td>
      <td>{{ player.genere }}</td>
    </tr>
    {% endfor %}
  </tbody>
</table>

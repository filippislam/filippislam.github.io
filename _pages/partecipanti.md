---
title: "Partecipanti"
permalink: /partecipanti/
author_profile: true
---

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
    <tr>
      <td>Matteo Filippi</td>
      <td>DX</td>
      <td>2001</td>
      <td>M</td>
    </tr>
    <tr>
      <td>Luca Bianchi</td>
      <td>SX</td>
      <td>1998</td>
      <td>M</td>
    </tr>
    <tr>
      <td>Giulia Rossi</td>
      <td>DX</td>
      <td>2003</td>
      <td>F</td>
    </tr>
  </tbody>
</table>

In questa sezione sono riportati tutti i partecipanti che hanno preso parte ad almeno un'edizione del Filippi Slam nel corso degli anni.<br>

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Braccio</th>
      <th>Anno di nascita</th>
      <th>Genere</th>
    </tr>
  </thead>
  <tbody>
    {% for player in site.data.players %}
    <tr>
      <td>{{ player.nome }}</td>
      <td>{{ player.braccio }}</td>
      <td>{{ player.anno }}</td>
      <td>{{ player.genere }}</td>
    </tr>
    {% endfor %}
  </tbody>
</table>

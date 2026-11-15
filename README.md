# Orizon — Sistema di gestione prenotazioni viaggi

Notebook Python che registra, visualizza, filtra e analizza le prenotazioni di un'agenzia di viaggi, con validazione dei dati in ingresso e controllo dei duplicati.

Progetto del corso **Python** del Master in Data Science, Analytics e Agenti AI di [start2impact](https://www.start2impact.it/).

## Il contesto

Orizon è un'agenzia di viaggi ipotetica, specializzata in turismo responsabile: viaggi di media e lunga durata, soprattutto in Asia e America, viaggi di nozze e viaggi di gruppo per adulti e famiglie.

La richiesta era un sistema per creare e gestire una collezione di elementi, con registrazione, visualizzazione, ricerca, filtraggio avanzato e statistiche. Ho scelto come elemento la **prenotazione di un viaggio**, perché è il dato su cui un'agenzia prende decisioni: quali destinazioni vendono di più, che tipo di clienti le scelgono, quanti viaggiatori si muovono verso ogni paese.

## Cosa fa

| Funzione | Cosa fa |
|---|---|
| `prenota_viaggio()` | Registra una prenotazione dopo aver validato ogni campo. Rifiuta i duplicati (stesso utente, paese e data di partenza). |
| `visualizza_prenotazioni()` | Stampa tutte le prenotazioni in formato leggibile. |
| `cerca_viaggi()` | Ricerca e filtraggio avanzato: paese, tipo di viaggio, costo massimo e durata minima, combinabili tra loro. |
| `statistiche()` | Totale prenotazioni, viaggiatori per paese, distribuzione per tipo di viaggio, destinazioni più richieste, ultima prenotazione. |
| `stampa_statistiche()` | Presenta le statistiche ordinate dal valore più alto al più basso. |

Ogni prenotazione ha questi attributi: id utente, id viaggio, paese di destinazione, durata in giorni, costo, numero di viaggiatori, data di partenza, tipo di viaggio (`nozze`, `gruppo`, `famiglia`, `single`).

## Esempio di risultato

Con le 10 prenotazioni di esempio incluse nel notebook:

```text
=== Statistiche prenotazioni ===

Prenotazioni totali: 10

Viaggiatori per paese:
  Argentina: 14
  Cile: 12
  Italia: 5
  Germania: 4
  Spagna: 1

Destinazioni più richieste: Argentina, Cile (3 prenotazioni)

Prenotazioni per tipo di viaggio:
  Famiglia: 3
  Gruppo: 3
  Nozze: 2
  Single: 2
```

Un filtro con più condizioni insieme:

```python
cerca_viaggi(gestione_viaggi, paese_destinazione="cile", tipo_viaggio="famiglia",
             costo_massimo=2000, durata_minima=15)
```

## Scelte di progetto

- **Lista di dizionari** come struttura dati: ogni prenotazione è un dizionario con chiavi leggibili, e la lista mantiene l'ordine di inserimento (utile per sapere qual è l'ultima registrata).
- **Qualità del dato all'ingresso**: una prenotazione con un campo non valido viene rifiutata con un messaggio che spiega il motivo. Sono gestiti tipi sbagliati, valori nulli, numeri zero o negativi, date inesistenti (es. 30 febbraio) e i booleani, che in Python verrebbero accettati come numeri interi.
- **Normalizzazione prima del confronto**: paese e tipo di viaggio sono salvati in minuscolo e senza spazi, la data sempre nel formato `YYYY-MM-DD`. Così `"ARGENTINA"`, `" argentina "` e la data `2026-5-5` vengono riconosciuti come duplicati di una prenotazione già presente.
- **Calcolo separato dalla stampa**: `statistiche()` restituisce un dizionario e `stampa_statistiche()` lo mostra a schermo. I risultati restano riutilizzabili da altro codice.
- **Pari merito gestiti**: se più paesi hanno lo stesso numero massimo di prenotazioni, vengono restituiti tutti.
- **Casi di test nel notebook**: ogni funzione è seguita da celle che provano sia i casi validi sia quelli da rifiutare, con l'esito atteso scritto nel commento.

## Limiti

- I dati vivono in memoria: chiudendo il notebook si perdono. Non c'è salvataggio su file o database.
- Non è prevista la cancellazione di una prenotazione; l'id viaggio progressivo si basa su questo.
- Non viene controllata la coerenza tra tipo di viaggio e numero di viaggiatori (es. un viaggio `single` con 3 persone è accettato).
- I test sono celle dimostrative, non test automatici.

## Come eseguirlo

Serve Python 3.10 o successivo e Jupyter. Non ci sono librerie esterne: il notebook usa solo `datetime` della libreria standard.

```bash
pip install notebook
```

```bash
jupyter notebook orizon_gestione_prenotazioni.ipynb
```

Esegui le celle dall'alto verso il basso (*Run All*). GitHub mostra comunque il notebook con tutti i risultati già visibili, senza installare nulla.

## Competenze messe in pratica

Python di base: funzioni con docstring e parametri facoltativi, liste, dizionari e tuple, cicli e condizioni, validazione dell'input, gestione delle eccezioni (`try`/`except`), gestione delle date con `datetime`, aggregazioni e conteggi senza librerie esterne.

## Autore

Danilo Manzo

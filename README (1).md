# L'impronta semantica degli LLM: Analisi comparativa tra musica reale e artificiale

Progetto per il corso **Tecnologie dei dati e del linguaggio** (Prof. Alfio Ferrara — Dott. Sergio Picascia, Dott.ssa Elisabetta Rocchetti), Studi Umanistici.
Sviluppo della **Traccia 3 — "Stereotipi, bias e rappresentazioni culturali"**, applicata al dominio musicale.

**Autrice:** Francesca Bubba

---

## Domanda di ricerca e ipotesi

**Domanda di ricerca:** un LLM, generando testi di canzoni per generi musicali legati a tradizioni culturali diverse, tende a renderli più simili tra loro (in termini semantici) rispetto a quanto lo siano realmente i testi originali dello stesso genere?

**Ipotesi:** nel corpus reale, i generi musicali restano linguisticamente e tematicamente più distinti tra loro rispetto ai testi generati dal modello, che tendono invece a un maggiore appiattimento reciproco (misurato come similarità semantica più alta tra coppie di generi).

## Fonti e dati

- **Testi generati:** prodotti da un modello open-weight scaricato da Hugging Face ed eseguito localmente — [`Qwen/Qwen2.5-3B-Instruct`](https://huggingface.co/Qwen/Qwen2.5-3B-Instruct), non gated.
- **Corpus di riferimento (testi reali):** raccolto tramite la [Genius API ufficiale](https://docs.genius.com/) con il pacchetto [`lyricsgenius`](https://pypi.org/project/lyricsgenius/), a partire da 3 artisti rappresentativi per ciascuno dei 6 generi selezionati (trap italiana, pop anglosassone, reggaeton, K-pop, afrobeats, fado).
- **Traduzione (normalizzazione multilingue):** [`deep-translator`](https://pypi.org/project/deep-translator/), per riportare tutti i testi in italiano prima del confronto semantico.
- **Analisi semantica:** [`sentence-transformers`](https://www.sbert.net/), modello multilingue `paraphrase-multilingual-MiniLM-L12-v2`.

Un tentativo iniziale di raccogliere i testi reali tramite scraping delle pagine pubbliche di Genius.com è stato abbandonato per via del blocco anti-bot del sito (HTTP 403) e dei suoi Termini di Servizio, che vietano lo scraping — da qui la scelta della Genius API ufficiale (gratuita).

## Metodologia

1. Definizione di 6 generi musicali e 3 temi fissi (amore, festa, nostalgia), con un unico prompt per la generazione.
2. Generazione dei testi con il modello locale, in batch (54 testi: 6 generi × 3 temi × 3 ripetizioni).
3. Raccolta del corpus reale tramite Genius API.
4. Preprocessing: pulizia del testo e traduzione in italiano dei testi non italiani.
5. Calcolo degli embedding semantici e delle matrici di similarità coseno tra generi, separatamente per testi generati e corpus reale.
6. Verifica statistica dell'ipotesi con un test di Wilcoxon sulle coppie di similarità tra generi.

## Limiti dello studio

- **Campione ridotto:** 54 testi generati e solo 16 testi reali (2 brani di fado non reperiti su Genius) sono un volume contenuto per conclusioni quantitative robuste.
- **Possibile artefatto della traduzione automatica:** il pattern di similarità quasi uniforme tra i generi non italiani, osservato sia nei testi generati sia nel corpus reale, suggerisce che la normalizzazione linguistica tramite traduzione possa aver introdotto un'omogeneizzazione indipendente dal fenomeno oggetto di studio.
- **Modello locale di dimensioni contenute:** `Qwen2.5-3B-Instruct` è più piccolo e meno capace di un LLM commerciale, il che può influire sulla qualità e naturalezza dei testi generati.
- **Rappresentatività degli artisti scelti:** la selezione di 3 artisti per genere è un punto di partenza arbitrario, non una campionatura sistematica della produzione musicale di ciascun genere.

## Dichiarazione sull'utilizzo dell'IA

Parti di questo progetto — in particolare la strutturazione del flusso di lavoro metodologico, la scrittura del codice Python e la stesura dei testi descrittivi — sono state sviluppate con l'assistenza di **Claude (Anthropic)**, secondo quanto consentito dalle linee guida del corso. Tutti i contenuti prodotti con l'assistenza dell'IA sono stati rivisti, adattati ed eseguiti dall'autrice, che si assume la piena responsabilità per il contenuto finale, la sua accuratezza e la sua integrità accademica.

## Riferimenti

- Genius API — https://docs.genius.com/
- `lyricsgenius` (PyPI) — https://pypi.org/project/lyricsgenius/
- `Qwen/Qwen2.5-3B-Instruct` (Hugging Face) — https://huggingface.co/Qwen/Qwen2.5-3B-Instruct
- `sentence-transformers` — https://www.sbert.net/
- `deep-translator` (PyPI) — https://pypi.org/project/deep-translator/

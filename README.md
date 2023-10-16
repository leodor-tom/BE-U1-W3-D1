# BE-U1-W3-D1

# Estrarre il nome e cognome dei clienti nati nel 1982.
SELECT nome, cognome FROM public.clienti WHERE data_nascita = 1982

# Estarre il numero di fatture con iva al 20%.
SELECT numero_fattura FROM public.fatture where iva = 20 (id delle fatture con iva al 20%)
SELECT COUNT(numero_fattura) FROM public.fatture where iva = 20 (numero totale di fatture con iva al 20%)

# Riportare il numero di fatture e la somma dei relativi importi divisi per anno di fatturazione.
SELECT data_fattura, 
COUNT(numero_fattura) AS totale_fatture, 
SUM(importo) AS somma_importo
FROM public.fatture
GROUP BY data_fattura

# Estrarre i prodotti attivati nel 2017 e che sono in produzione oppure in commercio.
SELECT id_prodotto
FROM public.prodotti
WHERE DATE_PART('year', data_attivazione) = 2017 AND in_produzione = true OR in_commercio = true

# Considerando sole le fatture con iva al 20% estrarre il numero di fatture per ogni anno.
SELECT DATE_PART('year', data_fattura) as data_fattura,
COUNT(numero_fattura) AS totale_fatture,
iva
FROM public.fatture
WHERE iva = 20
GROUP BY DATE_PART('year', data_fattura), iva

# Estrarre gli anni in cui sono state registrate più di due fatture con tipologia 'A'.
SELECT DATE_PART('year', data_fattura) as data_fattura,
COUNT(numero_fattura) AS totale_fatture,
tipologia
FROM public.fatture
WHERE tipologia = 'A' 
GROUP BY DATE_PART('year', data_fattura), tipologia
HAVING COUNT(numero_fattura) > 2

# Riportare l'elenco delle fatture (numero, importo, iva e data) con in aggiunta il nome del fornitore.
SELECT numero_fattura, importo, iva, data_fattura, fornitori.nome FROM public.fatture
JOIN public.fornitori ON fatture.numero_fornitore = fornitori.numero_fornitore

# Estrarre il totale degli importi delle fatture divisi per residenza dei clienti.
SELECT clienti.regione_residenza, SUM(importo)
FROM public.fatture
JOIN public.clienti ON fatture.id_cliente = clienti.numero_cliente
GROUP BY clienti.regione_residenza

# Estrarre il numero di clienti nati nel 1980 che hanno almeno una fattura superiore a 50 euro.
SELECT COUNT(DISTINCT numero_cliente)
FROM public.clienti
JOIN public.fatture ON clienti.numero_cliente = fatture.id_cliente
WHERE data_nascita = 1980
AND fatture.importo > 50

# Estrarre una colonna di nome "denominazione" contenente il nome, seguito da un carattere "-", seguito dal cognome, per i soli residenti nella regione lombardia.
SELECT CONCAT(nome, '-', cognome) AS denominazione,
regione_residenza
FROM public.clienti
WHERE regione_residenza = 'lombardia'

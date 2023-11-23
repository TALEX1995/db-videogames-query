# Select

### 1- Selezionare tutte le software house americane (3)

SELECT \*  
FROM `software_houses`  
WHERE country LIKE 'United States';

### 2. Selezionare tutti i giocatori della cittÃ di 'Rogahnland' (2)

SELECT \*  
FROM `players`  
WHERE city LIKE 'Rogahnland';

### 3. Selezionare tutti i giocatori il cui nome finisce per "a" (220)

SELECT \*  
FROM `players`  
WHERE name LIKE '%a';

### 4. Selezionare tutte le recensioni scritte dal giocatore con ID = 800 (11)

SELECT \*  
FROM `reviews`  
WHERE player_id = 800;

### 5. Contare quanti tornei ci sono stati nell'anno 2015 (9)

SELECT \*  
FROM `tournaments`  
WHERE year = 2015;

### 6. Selezionare tutti i premi che contengono nella descrizione la parola 'facere' (2)

SELECT \*  
FROM `awards`  
WHERE description LIKE '%facere%';

### 7. Selezionare tutti i videogame che hanno la categoria 2 (FPS) o 6 (RPG), mostrandoli una sola volta (del videogioco vogliamo solo l'ID) (287)

SELECT DISTINCT videogame_id  
FROM `category_videogame`  
WHERE category_id = 2  
OR category_id = 6;

### 8. Selezionare tutte le recensioni con voto compreso tra 2 e 4 (2947)

SELECT \*  
FROM `reviews`  
WHERE rating BETWEEN 2 AND 4;

### 9. Selezionare tutti i dati dei videogiochi rilasciati nell'anno 2020 (46)

SELECT \*  
FROM `videogames`  
WHERE YEAR(release_date) = 2020;

### 10. Selezionare gli id videogame che hanno ricevuto almeno una recensione da 5 stelle, mostrandoli una sola volta (443)

SELECT DISTINCT videogame_id  
FROM `reviews`  
WHERE rating = 5;

### 11. Selezionare il numero e la media delle recensioni per il videogioco con ID = 412 (review number = 12, avg_rating = 3.16 circa)

SELECT videogame_id, AVG(rating) as average_rating, COUNT(\*) as reviews_count  
FROM `reviews`  
WHERE videogame_id = 412  
GROUP BY videogame_id;

### 12. Selezionare il numero di videogame che la software house con ID = 1 ha rilasciato nel 2018 (13)

SELECT \*  
FROM `videogames`  
WHERE software_house_id = 1  
AND YEAR(release_date) = 2018;

<br>
<br>
<br>

# GROUP BY

### 1. Contare quante software house ci sono per ogni paese (3)

SELECT country, COUNT(\*)  
FROM `software_houses`  
GROUP BY country;

### 2. Contare quante recensioni ha ricevuto ogni videogioco (del videogioco vogliamo solo l'ID) (500)

SELECT videogame_id, COUNT(\*)  
FROM `reviews`  
GROUP BY videogame_id;

### 3. Contare quanti videogiochi hanno ciascuna classificazione PEGI (della classificazione PEGI vogliamo solo l'ID) (13)

SELECT pegi_label_id, COUNT(\*)  
FROM `pegi_label_videogame`  
GROUP BY pegi_label_id;

### 4. Mostrare il numero di videogiochi rilasciati ogni anno (11)

SELECT YEAR(release_date) as year_release, COUNT(\*)  
FROM `videogames`  
GROUP BY year_release;

### 5. Contare quanti videogiochi sono disponbiili per ciascun device (del device vogliamo solo l'ID) (7)

SELECT device_id, count(\*)  
FROM `device_videogame  `
GROUP BY device_id;

### 6. Ordinare i videogame in base alla media delle recensioni (del videogioco vogliamo solo l'ID) (500)

SELECT videogame_id, AVG(rating) as average_rating, COUNT(\*)  
FROM `reviews`  
GROUP BY videogame_id  
ORDER BY average_rating DESC;

<br>
<br>
<br>

# JOIN

### 1. Selezionare i dati di tutti giocatori che hanno scritto almeno una recensione, mostrandoli una sola volta (996)

SELECT DISTINCT players.\*  
FROM `reviews`  
JOIN players  
ON players.id = player_id;

### 2. Sezionare tutti i videogame dei tornei tenuti nel 2016, mostrandoli una sola volta (226)

SELECT DISTINCT videogames.id  
FROM `videogames`  
JOIN tournament_videogame  
ON videogames.id = tournament_videogame.videogame_id  
JOIN tournaments  
ON tournaments.id = tournament_videogame.tournament_id  
WHERE tournaments.year = 2016;

### 3. Mostrare le categorie di ogni videogioco (1718)

SELECT videogames.\*, categories.name as type_category  
FROM `videogames`  
JOIN category_videogame  
ON videogames.id = category_videogame.videogame_id  
JOIN categories  
ON categories.id = category_videogame.category_id;

### 4. Selezionare i dati di tutte le software house che hanno rilasciato almeno un gioco dopo il 2020, mostrandoli una sola volta (6)

SELECT DISTINCT software_houses.\*  
FROM `software_houses`  
JOIN videogames  
ON software_houses.id = videogames.software_house_id  
WHERE YEAR(videogames.release_date) >= 2020;

### 5. Selezionare i premi ricevuti da ogni software house per i videogiochi che ha prodotto (55)

SELECT awards.name, award_videogame.year, videogames.name as videogame_name, videogames.release_date, software_houses.name as software_name  
FROM `awards`  
JOIN award_videogame  
ON awards.id = award_videogame.award_id  
JOIN videogames  
ON videogames.id = award_videogame.videogame_id  
JOIN software_houses  
ON software_houses.id = videogames.software_house_id;

### 6. Selezionare categorie e classificazioni PEGI dei videogiochi che hanno ricevuto recensioni da 4 e 5 stelle, mostrandole una sola volta (3363)

SELECT DISTINCT videogames.id, videogames.name as nome_videogioco, categories.name as nome_categoria, pegi_labels.name as pegi_name
FROM `videogames`  
JOIN reviews  
ON videogames.id = reviews.videogame_id  
JOIN category_videogame  
ON category_videogame.videogame_id = videogames.id  
JOIN categories  
ON categories.id = category_videogame.category_id  
JOIN pegi_label_videogame  
ON videogames.id = pegi_label_videogame.videogame_id  
JOIN pegi_labels  
ON pegi_labels.id = pegi_label_videogame.pegi_label_id  
WHERE reviews.rating >= 4;

### 7. Selezionare quali giochi erano presenti nei tornei nei quali hanno partecipato i giocatori il cui nome inizia per 'S' (474)

SELECT DISTINCT videogames.\*  
FROM `videogames`  
JOIN tournament_videogame as tv  
ON tv.videogame_id = videogames.id  
JOIN tournaments as ts  
ON ts.id = tv.tournament_id  
JOIN player_tournament as pt  
ON pt.tournament_id = ts.id  
JOIN players  
ON players.id = pt.player_id  
WHERE players.name LIKE 'S%';

### 8. Selezionare le cittÃ in cui Ã¨ stato giocato il gioco dell'anno del 2018 (36)

SELECT ts.city  
FROM `videogames`  
JOIN tournament_videogame as tv  
ON tv.videogame_id = videogames.id  
JOIN tournaments as ts  
ON ts.id = tv.tournament_id  
JOIN award_videogame as av  
ON av.videogame_id = videogames.id  
JOIN awards  
ON awards.id = av.award_id  
WHERE awards.name = "Gioco dell'anno"  
AND av.year = 2018;

### 9. Selezionare i giocatori che hanno giocato al gioco piÃ¹ atteso del 2018 in un torneo del 2019 (3306)

SELECT players.\*  
FROM `players`  
JOIN player_tournament as pt  
ON pt.player_id = players.id  
JOIN tournaments as ts  
ON ts.id = pt.tournament_id  
JOIN tournament_videogame as tv  
ON tv.tournament_id = ts.id  
JOIN videogames  
ON tv.videogame_id = videogames.id  
JOIN award_videogame as av  
ON videogames.id = av.videogame_id  
JOIN awards  
ON awards.id = av.award_id  
WHERE awards.name = "Gioco più atteso"  
AND ts.year = 2019  
AND av.year = 2018;

# day87_lev1_1_mongoDB-quering

_Source:_ https://github.com/Ninni-cfm/day87_lev1_1_mongoDB-quering

---

## JS Express CodeFlow Übung lev1_1: Mongodb quering

### Aufgabenstellung

**installiere mongodb shell!**

-   Nutze die zip-Datei aus dem Classroom
-   entpacke diese Datei in einen neuen Ordner
-   öffne das Terminal und navigiere zu diesem Ordner
    -   Überprüfe mit _pwd_ und _ls_ ob du in dem richtigen Ordner bist!
-   im richtigen Ordner starte die mongo shell
-   Verbinde dich zu deinem Sandbox Cluster und nutze den Befehl _load(‘loadMovieDetailsDataset.js’)_
-   um die Datei in deine Datenbank zu laden
-   überprüfe mit show dbs dass die Datenbank geladen wurde!

```
    > mongosh "mongodb+srv://\<username>:\<password>@\<cluster>.mongodb.net/admin"

    > load('loadMovieDetailsDataset.js')

    > show dbs

    > use video
```

---

### Abrufen in: video>movieDetails

1.  Alle Filme, bei denen der Regisseur Steven Spielberg ist - rufe nur das Titelfeld ab.

```
    > db.movieDetails.find( { "director": "Steven Spielberg" }, { "title": 1, "_id": 0 } )
```

2.  Alle Filme, in denen der Benutzerbewertungen in tomato sind mehr als 40000. Beschränke die Suche auf 20 Filme und sortiere sie nach Benutzerbewertungen.

```
    > db.movieDetails.find( { "tomato.userReviews": {"$gt": 40000 } } ).sort( { "tomato.userRating": -1 } ).limit( 20 )
```

3.  Alle Filme, die zwischen 2000 und 2005 gedreht wurden, beide Jahre eingeschlossen. Rufe nur die Felder Titel und Jahr ab.

```
    > db.movieDetails.find( { "year": { "$gte": 2000, "$lte": 2005 } }, { "title": 1, "year": 1, "_id": 0 } )
```

4.  Alle Filme, die eine Tomato-Benutzerbewertung von mehr oder gleich 4 hatten und nach 2010 entstanden sind. Rufe nur den Titel und den Regisseur ab.

```
    > db.movieDetails.find( { "tomato.userRating": { "$gte": 4 }, "year": { "$gt": 2010 } }, { "title": 1, "director": 1, "_id": 0 } )
```

5.  Alle Filme, die weniger als 1000 Benutzer-Rezensionen in Tomato haben und vor 2005 gemacht wurden. Ordne sie nach der Anzahl der Benutzer-Rezensionen und beschränke die Suche auf 10 Filme.

```
    > db.movieDetails.find( { "tomato.userReviews": { "$lt": 1000 }, "year": { "$lt": 2005 } } ).sort( { "tomato.userReviews": 1 } ).limit( 10 )
```

6.  Alle Filme, die das Tomato-Feld nicht enthält.

```
    > db.movieDetails.find( { "tomato": { "$eq": null } } )
```

7.  Alle Filme, die mindestens 100 imdb-Stimmen, aber weniger als 1000 haben. Rufe nur die Titelfelder und imdb-Bewertung ab.

```
    > db.movieDetails.find( { "imdb.votes": { "$gte": 100, "$lt": 1000 } }, { "title": 1, "imdb.rating": 1, "_id": 0 } )
```

8.  Ordnen Sie alle Filme nach ihrer imdb-Bewertung in absteigender Reihenfolge.

```
    > db.movieDetails.find( {}, { "title": 1, "imdb.rating": 1, "_id": 0 } ).sort( { "imdb.rating": -1 } )
```

9.  Rufe die 10 Filme mit mehr imdb-Bewertung ab, geordnet nach der imdb-Bewertung

```
    > db.movieDetails.find( {} ).sort( { "imdb.rating": -1, "imdb.votes": -1  } ).limit( 10 ).pretty()
```

oder

9.  Rufe die 10 Filme mit mehr imdb-Bewertung**en** ab, geordnet nach der imdb-Bewertung

```
    > db.movieDetails.find( {} ).sort( { "imdb.votes": -1, "imdb.rating": -1  } ).limit( 10 ).pretty()
```

10. All the movies with genres Crime and Drama, retrieving only their title and genres, order by the imdb rating.

```
    > db.movieDetails.aggregate( [ { "$match": {"genres": [ "Crime", "Drama" ] } }, { "$project": { "title": 1, "genres": 1, "_id": 0 } } ] ).sort( { "imdb.rating": -1 } )
```

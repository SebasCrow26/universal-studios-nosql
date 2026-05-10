# Universal Studios Colombia — NoSQL Data Lake (Online Record Store)

## Project Overview

This project was developed for Universal Studios Colombia as part of a NoSQL databases workshop. The goal was to build a Data Lake for an online record store using **MongoDB** as the NoSQL database engine. The dataset covers six internationally and Latin American renowned artists across different music genres.

Each album is modeled as a **collection** in MongoDB, and each song is a **document** within its corresponding collection. This document-oriented approach allows flexible, schema-less storage that adapts naturally to the varying metadata of songs across different genres and eras.

---

## Artists and Albums

| Artist | Album | Year | Genre |
|---|---|---|---|
| Lady Gaga | The Fame | 2008 | Dance-pop / Synth-pop |
| Diomedes Díaz | El Cacique | 1988 | Vallenato |
| BTS | Map of the Soul: 7 | 2020 | K-pop |
| Michael Jackson | Thriller | 1982 | Funk-pop / R&B |
| Queen | A Night at the Opera | 1975 | Progressive Rock |
| Ivy Queen | Diva | 2003 | Reggaeton |

---

## Data Model Structure

Each document (song) contains the following fields:

```json
{
  "_id": "unique identifier (e.g. lg001)",
  "titulo": "song title",
  "anio_salida": 2008,
  "autor": "artist or band name",
  "id_imagen_portada": "images/album_cover.jpg",
  "genero": "music genre",
  "duracion": "duration in mm:ss format",
  "numero_pista": 1
}
```

**Mandatory fields:** `_id`, `titulo`, `anio_salida`, `autor`, `id_imagen_portada`

**Optional fields added:** `genero`, `duracion`, `numero_pista`

---

## Task Summaries

### Task 1 — Initial Database Setup (branch: `main`)
Created the `universal_studios` database in MongoDB Atlas with six collections, one per artist album. Each collection was populated with the most well-known songs from the selected album using `insertMany()`. Album cover images were collected and stored in the `images/` folder. The full database was exported to CSV format.

**Collections created:**
- `lady_gaga_the_fame` — 8 documents
- `diomedes_diaz_el_cacique` — 8 documents
- `bts_map_of_the_soul_7` — 8 documents
- `michael_jackson_thriller` — 9 documents
- `queen_a_night_at_the_opera` — 8 documents
- `ivy_queen_diva` — 8 documents

### Task 2 — Database Update (branch: `actualizaciones`)
Added lesser-known songs to each album collection using `insertMany()` with new document IDs. Data integrity was verified after each insertion using `countDocuments()`. Updated CSV and BSON files were generated and pushed to the `actualizaciones` branch.

### Task 3 — Record Deletion (branch: `eliminaciones`)
Deleted at least two songs from each album collection using `deleteOne()` and `deleteMany()`. One complete artist collection was dropped using `db.collection.drop()`. Changes were verified and the resulting database was exported to updated CSV and BSON files, pushed to the `eliminaciones` branch.

---

## Design Decisions

- **One collection per album** (not per artist): This approach makes it easier to query, update, and delete songs at the album level, which mirrors how a real music store would organize its catalog.
- **String-based `_id` values**: Instead of MongoDB's default ObjectId, we used human-readable IDs (e.g. `lg001`, `mj005`) to make documents easier to identify and reference.
- **Additional optional fields**: `genero`, `duracion`, and `numero_pista` were added to enrich the dataset beyond the minimum requirements.
- **Unified `images/` folder**: All album cover images are stored in a single folder and referenced by path in each document, simulating a real media asset management system.

---

## Findings, Challenges, and Learnings

- **MongoDB's flexible schema** was one of the most interesting discoveries. Unlike SQL, we could add or omit fields per document without altering a table structure.
- **MongoSH inside Compass** made it straightforward to run bulk insertions with `insertMany()`, which was much faster than inserting documents one by one.
- **Branch management in Git** required careful attention — each task had to be committed to its own branch (`main`, `actualizaciones`, `eliminaciones`) without mixing changes.
- **BSON exports** via `mongodump` required using the terminal, which was a new experience for some team members.
- One challenge was understanding the difference between **dropping a collection** (`db.collection.drop()`) and **deleting documents** (`deleteOne` / `deleteMany`).

---

## How to Replicate This Project

### Prerequisites
- MongoDB Atlas account (free tier M0 is sufficient)
- MongoDB Compass installed
- Node.js or MongoDB Shell (mongosh) for terminal exports
- Git and a GitHub account

### Steps

1. **Clone this repository:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/universal-studios-nosql.git
   cd universal-studios-nosql
   ```

2. **Connect to MongoDB Atlas** using Compass with your connection string.

3. **Import BSON files** using mongorestore:
   ```bash
   mongorestore --uri="your_connection_string" --db=universal_studios ./bson/
   ```

4. **Or import CSV files** directly through Compass:
   - Open the target collection
   - Click "Add Data" → "Import File"
   - Select the CSV file and map fields

5. **Verify data:**
   ```js
   use("universal_studios")
   show collections
   db.lady_gaga_the_fame.countDocuments()
   ```

---

## Repository Structure

```
universal-studios-nosql/
├── images/                   # Album cover images
├── *.csv                     # CSV exports per collection
├── bson/                     # BSON exports (mongodump output)
└── README.md                 # This file
```

---

*Workshop completed by: Sebastián Ramos, Miguel Moreno, Santiago Sandoval.| Universal Studios Colombia | NoSQL Databases*

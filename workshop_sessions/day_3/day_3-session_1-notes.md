- `import sqlite3`
- `connection = sqlite3.connect('practical.db')`
- `cursor = connection.cursor()`
- `tables = cursor.execute('SELECT name from sqlite_master where type="table"')`
- `tables = cursor.fetchall()`
```Python
for t in tables:
    print(t)
```

```Python
cursor.execute('''SELECT name,sequence FROM annotation
                  WHERE uniprotID = "P16070";''')
info = cursor.fetchall()
print(info)
info[0]
info[0][1]
```

```Python
protein_id = "Q9H6A9"
cursor.execute(f'''SELECT name,sequence FROM annotation
                   WHERE uniprotID = "{protein_id}";''')
protein = cursor.fetchone()
print(protein[0])
print(protein[1])
```

```Python
def get_info(uniProtID, database):
    conn = sqlite3.connect(database)
    cursor = conn.cursor()
    cursor.execute("Select geneSymbol,go from annotation where uniprotID = ?",  (uniProtID,))
    result = cursor.fetchone()
    conn.close()
    return result
get_info("Q9H6A9", "practical.db")
```

- `import requests`

```Python
# fetch term ID from Ensembl
server = "http://rest.ensembl.org"
extension = f'/ontology/name/cochlea development?content-type=application/json'
response = requests.get(server+extension)
if response.ok:
    json_data = response.json()
    result = json_data[0]
    term_id = result['accession']
# query local database
    with sqlite3.connect('practical.db') as connection:
        cursor.execute(f'SELECT name FROM annotation WHERE go LIKE "%{gid}%";')
        hits = cursor.fetchall()
print("cochlea development:", len(hits))
```

```Python
def count_records_for_term(go_term):
    '''Given a GO term, fetch the corresponding GO identifier and
       count the number of entries with that identifier in the local
       database, "practical.db"'''
    # fetch term ID from Ensembl
    server = "http://rest.ensembl.org"
    extension = f'/ontology/name/{go_term}?content-type=application/json'
    response = requests.get(server+extension)
    if response.ok:
        json_data = response.json()
        result = json_data[0]
        term_id = result['accession']
    # query local database
        with sqlite3.connect('practical.db') as connection:
            cursor.execute(f'SELECT name FROM annotation WHERE go LIKE "%{gid}%";')
            hits = cursor.fetchall()
        return len(hits)
```

- if enough time: error handling

# Database option

**Set pebbledb (**<mark style="color:red;">**default**</mark>**)  \***

```bash
sed -i "s|db_backend =.*|db_backend=\"pebbledb\"|g" "$HOME/.nibid/config/config.toml"

sed -i "s|app-db-backend =.*|app-db-backend=\"pebbledb\"|g" "$HOME/.nibid/config/app.toml"
```

**Set goleveldb (**<mark style="color:red;">**Option**</mark>**) \*\***

```bash
sed -i "s|db_backend =.*|db_backend=\"goleveldb\"|g" "$HOME/.nibid/config/config.toml"

sed -i "s|app-db-backend =.*|app-db-backend=\"goleveldb\"|g" "$HOME/.nibid/config/app.toml"
```

**\* :** Pebbledb is the default database\
\*\*: Goleveldb is a custom database

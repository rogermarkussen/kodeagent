# Polars Referanse

Cachet dokumentasjon for vanlige polars-operasjoner. Bruk Context7 for spesielle operasjoner.

## Import

```python
import polars as pl
from polars import col as c
```

---

## filter

Filtrer rader basert på betingelser.

```python
df.filter(c.foo > 1)
df.filter((c.foo < 3) & (c.ham == "a"))
df.filter((c.foo == 1) | (c.ham == "c"))
df.filter(c.foo.gt(0) & c.eier.ne(""))
df.filter(c.foo.is_in([1, 2, 3]))
df.filter(~c.foo.is_null())
```

**Metoder:**
- `.gt()`, `.lt()`, `.ge()`, `.le()` - sammenligninger
- `.eq()`, `.ne()` - likhet
- `.is_in()`, `.is_null()`, `.is_not_null()`
- `&` (og), `|` (eller), `~` (ikke)

---

## with_columns

Legg til eller erstatt kolonner.

```python
df.with_columns(c.a ** 2)
df.with_columns((c.a * 2).alias("a_double"))
df.with_columns(
    c.weight / (c.height ** 2).alias("bmi"),
    (2024 - c.birthdate.dt.year()).alias("age")
)

# Keyword-syntaks
df.with_columns(
    ab=c.a * c.b,
    not_c=c.c.not_()
)
```

---

## select

Velg kolonner.

```python
df.select(c.a, c.b)
df.select(c("*").exclude("foo"))
df.select(c.a, c.b.alias("b_renamed"))
```

---

## join

Kombiner DataFrames.

```python
df1.join(df2, on="id", how="inner")
df1.join(df2, on="id", how="left")
df1.join(df2, on=["id", "type"], how="left")
df1.join(df2, left_on="id1", right_on="id2", how="inner")
```

**how-verdier:** `inner`, `left`, `right`, `full`, `cross`, `semi`, `anti`

---

## group_by + agg

Grupper og aggreger.

```python
df.group_by(c.kategori).agg(c.verdi.sum())
df.group_by(c.a).agg(
    c.b.sum(),
    c.c.mean(),
    c.d.count()
)
df.group_by(c.a).agg(
    total=c.b.sum(),
    snitt=c.c.mean()
)
```

---

## sort

Sorter rader.

```python
df.sort(c.a)
df.sort(c.a, descending=True)
df.sort(c.a, c.b, descending=[True, False])
df.sort(c.a, nulls_last=True)
```

---

## unique

Fjern duplikater.

```python
df.unique()
df.unique(subset=["foo"])
df.unique(subset="foo", keep="first")
df.unique(subset="foo", keep="last")
df.unique(subset="foo", keep="none")
```

**keep-verdier:** `any`, `first`, `last`, `none`

---

## when-then-otherwise

Betingede verdier.

```python
df.with_columns(
    pl.when(c.quantity < 10).then(pl.lit("Lav"))
    .when(c.quantity < 30).then(pl.lit("Medium"))
    .otherwise(pl.lit("Høy"))
    .alias("nivå")
)

df.with_columns(
    pl.when(c.foo > 2).then(1).otherwise(c.bar + 1).alias("val")
)
```

---

## String-operasjoner

```python
c.navn.str.to_uppercase()
c.navn.str.to_lowercase()
c.navn.str.len_chars()
c.navn.str.contains("mønster")
c.navn.str.starts_with("prefix")
c.navn.str.ends_with("suffix")
c.navn.str.replace("old", "new")
c.navn.str.replace_all("a", "X")
c.navn.str.split(",")
c.email.str.split("@").list.first()
```

---

## Casting

```python
c.kolonne.cast(pl.Int64)
c.kolonne.cast(pl.Float64)
c.kolonne.cast(pl.Utf8)
c.kolonne.cast(pl.Int16)
```

---

## Lazy vs Eager

```python
# Eager - leser alt med en gang
df = pl.read_parquet("fil.parquet")

# Lazy - optimaliserer spørringen
lf = pl.scan_parquet("fil.parquet")
df = lf.filter(c.verdi > 90).select(c.navn, c.verdi).collect()
```

**Bruk lazy for:**
- Store filer
- Komplekse spørringer
- Predicate pushdown (filter før lesing)

---

## Vanlige mønstre

### Rename
```python
df.rename({"old": "new"})
```

### Drop
```python
df.drop("kolonne")
df.drop(["kol1", "kol2"])
```

### Fill null
```python
c.kolonne.fill_null(0)
c.kolonne.fill_null(strategy="forward")
```

### Shift
```python
c.kolonne.shift(1)
c.kolonne.shift(-1)
```

### Over (window functions)
```python
c.verdi.sum().over(c.gruppe)
c.verdi.rank().over(c.gruppe)
```

### Horizontal operations
```python
pl.sum_horizontal([c.a, c.b, c.c])
pl.concat_str([c.fornavn, c.etternavn], separator=" ")
```

---

## Les/Skriv

```python
# Parquet
df = pl.read_parquet("fil.parquet")
df.write_parquet("fil.parquet")

# Excel
df = pl.read_excel("fil.xlsx", sheet_name="Ark1")
df.write_excel("fil.xlsx")

# CSV
df = pl.read_csv("fil.csv")
df.write_csv("fil.csv")
```

---

## Concat

```python
pl.concat([df1, df2])
pl.concat([df1, df2], how="horizontal")
pl.concat([df1, df2], how="diagonal")
```

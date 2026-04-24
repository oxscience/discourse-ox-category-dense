# OX Campus — Category Dense Layout

Discourse Theme Component fuer campus.outoftheb-ox.de.
Macht einzelne Kategorien dichter, damit mehr Content pro Screen passt.

---

## Was macht die Component?

Scoped nur auf Kategorien deren Slug in `common/common.scss` definiert ist (Default: `wissenschaft`). In allen anderen Kategorien bleibt alles wie gehabt.

**Pro aktivierte Kategorie:**
- Topic-Zeilen kompakter — 2–3x mehr Topics pro Screen
- Tag-Pills prominenter
- Avatare kleiner (20px), Meta-Infos dezenter
- Kategorie-Header minimal poliert
- **Keine Paradigmen-Aenderung:** Tabellen-Layout bleibt, nur Luft rausgenommen

**Sicherheitsnetz:**
- Greift ausschliesslich ueber `body.category-{slug}` — keine globalen Selektoren
- Komplett CSS-only, kein JavaScript
- Deaktivieren in Admin > Customize > Themes reicht zum Rollback

---

## Installation auf campus.outoftheb-ox.de

1. **Admin > Customize > Themes > Install**
2. "From a git repository" waehlen
3. URL: `https://github.com/oxscience/discourse-ox-category-dense`
4. Install druecken
5. **Wichtig:** Als Component (nicht als Theme) installieren
6. Unter "Included in these themes" das **OX Campus Theme** anhaken
7. Save

Nach Install: Cache-Bust ist Pflicht (siehe unten).

---

## Cache-Bust (Pflicht nach jedem Update)

Nach jeder Code-Aenderung am main-Branch muss Discourse das Stylesheet neu bauen. Sonst bleibt der alte Bundle live.

SSH auf den Server:

```bash
ssh root@46.224.200.248
cd /var/discourse
./launcher enter app

rails c
# Im Rails-Prompt:
Rails.cache.clear
StylesheetCache.delete_all
Stylesheet::Manager.clear_theme_cache!
exit
```

Dann im Browser: Hard-Reload (Cmd+Shift+R) und pruefen ob CSS-Hash sich geaendert hat.

---

## Scope erweitern (weitere Kategorien)

In `common/common.scss` die Zeile

```scss
$dense-scope: ".category-wissenschaft";
```

erweitern, z.B.:

```scss
$dense-scope: ".category-wissenschaft, .category-open-campus";
```

Dann commit, push, Cache-Bust.

---

## Rollback

**Weich (Component aus):** Admin > Customize > Themes > OX Campus > Component "OX Campus — Category Dense Layout" Haken raus > Save. 3 Sekunden.

**Hart (Component weg):** Component loeschen im Admin. Das Haupt-Theme bleibt unberuehrt.

**Notfall:** Das Haupt-Theme `discourse-eu-stack-theme` hat einen Backup-Tag `backup/pre-category-hybrid-2026-04-25` — falls dort je was schiefgehen sollte (was bei dieser Component nicht passieren kann, da sie separat ist).

---

## Lizenz

MIT. Siehe [LICENSE](LICENSE).

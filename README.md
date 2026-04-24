# OX Campus — Category Hybrid Layout

Discourse Theme Component fuer campus.outoftheb-ox.de.
Gibt einzelnen Kategorien ein dichteres Listen-Layout mit Featured-Cards fuer Pinned Topics.

---

## Was macht die Component?

Scoped nur auf Kategorien deren Slug in `common/common.scss` definiert ist (Default: `wissenschaft`). In allen anderen Kategorien bleibt alles wie gehabt.

**Pro aktivierte Kategorie:**
- Topic-Liste dichter (2–3x mehr Content pro Screen)
- Pinned Topics bekommen Featured-Card-Look (Akzent-Border, "Featured"-Badge, Excerpt sichtbar)
- Tag-Pills prominenter, Sekundaerinfos dezenter
- Kategorie-Header leicht poliert

**Sicherheitsnetz:**
- Greift ausschliesslich ueber `body.category-{slug}` — keine globalen Selektoren
- Komplett CSS-only, kein JavaScript
- Deaktivieren in Admin > Customize > Themes reicht zum Rollback

---

## Installation auf campus.outoftheb-ox.de

1. **Admin > Customize > Themes > Install**
2. "From a git repository" waehlen
3. URL: `https://github.com/oxscience/discourse-ox-category-hybrid`
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
$hybrid-scope: ".category-wissenschaft";
```

erweitern, z.B.:

```scss
$hybrid-scope: ".category-wissenschaft, .category-open-campus";
```

Dann commit, push, Cache-Bust.

---

## Rollback

**Weich (Component aus):** Admin > Customize > Themes > OX Campus > Component "OX Campus — Category Hybrid Layout" Haken raus > Save. 3 Sekunden.

**Hart (Component weg):** Component loeschen im Admin. Das Haupt-Theme bleibt unberuehrt.

**Notfall:** Das Haupt-Theme `discourse-eu-stack-theme` hat einen Backup-Tag `backup/pre-category-hybrid-2026-04-25` — falls dort je was schiefgehen sollte (was bei dieser Component nicht passieren kann, da sie separat ist).

---

## Roadmap (wenn Phase 1 funktioniert)

- [ ] Phase 2: Echter Card-Grid fuer Pinned Topics via Plugin-Outlet + kleines JS
- [ ] Phase 3: Thumbnails aus erstem Bild im Topic
- [ ] Phase 4: Tag-basierte Icon-Map (Evidenz, Training, Werder, ...)

---

## Lizenz

MIT. Siehe [LICENSE](LICENSE).

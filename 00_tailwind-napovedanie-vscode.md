# Tailwind CSS – ako zapnúť napovedanie tried vo VS Code

Keď máš Tailwind pripojený len cez CDN, triedy fungujú **v prehliadači**, ale VS Code ti ich sám od seba **nenapovie** v `class=""`.

**Prečo?** Odkaz v `<script>` je runtime vec – spracuje sa až v prehliadači. Editor ho vôbec nečíta, takže o Tailwinde „nevie". Napovedanie robí rozšírenie **Tailwind CSS IntelliSense**, a to sa zapne len vtedy, keď v priečinku nájde *signál*, že ide o Tailwind projekt. Aký signál, to závisí od verzie.

---

## 0. Nainštaluj rozšírenie (platí pre obe verzie)

V VS Code otvor **Extensions** (`Ctrl+Shift+X`) a nainštaluj **Tailwind CSS IntelliSense** (od Tailwind Labs). Maj aktuálnu verziu.

---

## 1. Zisti, ktorú verziu máš

Pozri sa na svoj odkaz na Tailwind:

| V HTML máš… | Verzia | Budík pre IntelliSense |
|---|---|---|
| `@tailwindcss/browser@4` (so zavináčom) | **v4** | `.css` súbor s `@import "tailwindcss";` |
| `cdn.tailwindcss.com` | **v3** | súbor `tailwind.config.js` |

Podľa toho choď na A) alebo B).

---

## A) Tailwind v4 (odkaz so zavináčom)

```html
<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
```

V4 zrušil JS konfiguráciu a prešiel na „CSS-first", takže budíkom je **CSS súbor**.

**Vytvor v priečinku `.css` súbor** (napr. `app.css`) s jediným riadkom:

```css
@import "tailwindcss";
```

Tento súbor slúži **len ako „budík"** pre editor – reálne štýluje aj tak CDN za behu, takže ho ani nemusíš linkovať do HTML. Stačí, že je v priečinku.

Ak sa napovedanie samo nechytí, nasmeruj ho v `settings.json`:

```json
"tailwindCSS.experimental.configFile": "app.css"
```

---

## B) Tailwind v3 (odkaz `cdn.tailwindcss.com`)

```html
<script src="https://cdn.tailwindcss.com"></script>
```

Tu je budíkom **konfiguračný súbor**. Vytvor v priečinku `tailwind.config.js` – stačí minimálny:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./**/*.html"],
  theme: { extend: {} },
  plugins: [],
}
```

Pri CDN používaš defaultný Tailwind, takže tento prázdny config dá presne správne napovedanie.

---

## 2. Dokončenie (platí pre obe verzie)

**Otvor celý priečinok** cez **File → Open Folder** (nie len samotný `.html` súbor) – rozšírenie hľadá budík v rámci celého priečinka.

**Zapni napovedanie vnútri úvodzoviek.** VS Code štandardne nenapovedá v reťazcoch, takže aj s budíkom to v `class="..."` často mlčí. Otvor **settings.json** (`Ctrl+Shift+P` → *Preferences: Open User Settings (JSON)*) a pridaj:

```json
"editor.quickSuggestions": {
  "strings": "on"
}
```

**Reload okna** – `Ctrl+Shift+P` → **Developer: Reload Window**.
(Ak si budík vytvoril, keď už bol projekt otvorený, editor ho zachytí až po reštarte.)

---

## Hotovo ✅

Teraz ti v `class=""` napovie `flex`, `justify-center`, `p-4` a pod. Keď prejdeš myšou nad triedou, ukáže sa aj jej CSS.

# Tailwind CSS – ako zapnúť napovedanie tried vo VS Code

Keď do HTML pridáš Tailwind len cez CDN:

```html
<script src="https://cdn.tailwindcss.com"></script>
```

…triedy fungujú **v prehliadači**, ale VS Code ti ich **nenapovie** v `class=""`.

**Prečo?** Ten `<script>` je runtime vec – skompiluje sa až v prehliadači. Editor odkaz v `<script src="...">` vôbec nečíta, takže o Tailwinde „nevie". Napovedanie robí rozšírenie **Tailwind CSS IntelliSense**, a to sa zapne len vtedy, keď v priečinku nájde konfiguračný súbor `tailwind.config.js`.

---

## Postup

### 1. Nainštaluj rozšírenie
V VS Code otvor **Extensions** (`Ctrl+Shift+X`) a nainštaluj **Tailwind CSS IntelliSense** (od Tailwind Labs).

### 2. Pridaj do priečinka súbor `tailwind.config.js`
Stačí minimálny – slúži len ako „budík" pre editor:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./**/*.html"],
  theme: { extend: {} },
  plugins: [],
}
```

### 3. Otvor celý priečinok
Projekt otvor cez **File → Open Folder** (nie len samotný `.html` súbor). Rozšírenie hľadá config v rámci celého priečinka.

### 4. Zapni napovedanie vnútri úvodzoviek
VS Code štandardne nenapovedá v reťazcoch, takže aj s configom to v `class="..."` často mlčí.
Otvor **settings.json** (`Ctrl+Shift+P` → *Preferences: Open User Settings (JSON)*) a pridaj:

```json
"editor.quickSuggestions": {
  "strings": "on"
}
```

### 5. Reload okna
`Ctrl+Shift+P` → **Developer: Reload Window**.
(Ak si config vytvoril, keď už bol projekt otvorený, editor treba reštartovať, aby ho zachytil.)

---

## Hotovo ✅

Teraz ti v `class=""` napovie `flex`, `justify-center`, `p-4` a pod. Keď prejdeš myšou nad triedou, ukáže sa aj jej CSS.

---

> **Pozn.:** Toto platí pre Tailwind v3 (čo serveruje `cdn.tailwindcss.com`). Vo v4 sa konfigurácia presunula do CSS, takže napovedanie sa tam budí inak – cez CSS súbor s `@import "tailwindcss"`. Pre prácu s CDN je ale cesta „CDN + minimálny `tailwind.config.js`" najjednoduchšia.


---

### 📌 Adresse mémoire de `Timer` = base de l’objet + offset du champ

Voici la ligne clé dans le `get` et le `set` :

```csharp
nint num = (nint)IL2CPP.Il2CppObjectBaseToPtrNotNull(this) 
           + (int)IL2CPP.il2cpp_field_get_offset(NativeFieldInfoPtr_Timer);
```

Décomposons-la en **2 morceaux importants** :

---

## 1. 🔗 `IL2CPP.Il2CppObjectBaseToPtrNotNull(this)`

👉 **Renvoie l'adresse de base de l'objet en mémoire natif** (équivalent IL2CPP de `this`).

* C’est un appel à une fonction qui transforme un objet C# en **pointeur vers sa version IL2CPP en mémoire**.
* Autrement dit : "Donne-moi l’adresse de l'objet `this` dans la mémoire natif de Unity".

➡️ Résultat : une **adresse mémoire de base** (par exemple : `0x0123ABCD`)

---

## 2. 🧮 `IL2CPP.il2cpp_field_get_offset(NativeFieldInfoPtr_Timer)`

👉 **Renvoie l’offset (décalage en octets)** du champ `Timer` **dans la structure mémoire de l’objet**.

* `NativeFieldInfoPtr_Timer` est un pointeur vers une **structure de métadonnées** décrivant le champ `Timer`.
* `il2cpp_field_get_offset(...)` extrait l'offset en mémoire de ce champ à partir de sa métadonnée.

➡️ Résultat : un **entier** (par exemple : `16`) qui indique que `Timer` est situé à 16 octets après le début de l'objet.

---

## 🧠 L'adresse finale de `Timer` est donc :

```csharp
adresse_base_objet + offset_du_champ_Timer
```

Si :

* `adresse_base_objet` = `0x0123ABCD`
* `offset_du_champ_Timer` = `0x10` (16 en décimal)

Alors :

```csharp
adresse_finale = 0x0123ABCD + 0x10 = 0x0123ABDD
```

Et à cette adresse se trouve la valeur `float` du champ `Timer`.

---

### Pour résumer simplement :

* `Il2CppObjectBaseToPtrNotNull(this)` → Où commence l’objet en mémoire.
* `il2cpp_field_get_offset(...)` → Où est le champ à l’intérieur de l’objet.
* La somme des deux → Où est **le champ `Timer` exactement** en mémoire.

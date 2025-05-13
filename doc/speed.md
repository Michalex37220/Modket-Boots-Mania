
---

## 📄 Propriétés : `SpeedMin` et `SpeedMax`

```csharp
public unsafe float SpeedMin { get; set; }
public unsafe float SpeedMax { get; set; }
```

### 🔍 Description

Ces propriétés permettent de lire et modifier directement les valeurs `SpeedMin` et `SpeedMax` d’un objet Unity **via accès mémoire natif**, en utilisant les offsets IL2CPP des champs correspondants.

Elles sont définies en `unsafe` car elles manipulent des pointeurs bas niveau vers la mémoire C++ d’un objet Unity, dans un contexte IL2CPP (Unity compilé en code natif).

---

### 🧱 Accès mémoire natif

Les propriétés utilisent le schéma suivant :

```csharp
nint objectBase = IL2CPP.Il2CppObjectBaseToPtrNotNull(this);
int fieldOffset = (int)IL2CPP.il2cpp_field_get_offset(NativeFieldInfoPtr_SpeedMin);
float* fieldAddress = (float*)(objectBase + fieldOffset);
```

Ainsi, on accède à la **position mémoire exacte** du champ dans l’objet natif Unity, puis on lit/écrit la valeur comme un `float`.

---

### 📥 Paramètres (interne)

| Élément                                     | Type     | Description                                                                 |
| ------------------------------------------- | -------- | --------------------------------------------------------------------------- |
| `NativeFieldInfoPtr_SpeedMin`               | `IntPtr` | Pointeur vers les métadonnées du champ `SpeedMin` dans la structure IL2CPP. |
| `NativeFieldInfoPtr_SpeedMax`               | `IntPtr` | Idem pour `SpeedMax`.                                                       |
| `IL2CPP.il2cpp_field_get_offset(...)`       | `int`    | Retourne l’offset mémoire (en octets) du champ dans la classe native.       |
| `IL2CPP.Il2CppObjectBaseToPtrNotNull(this)` | `nint`   | Retourne le pointeur natif vers l’instance C++ correspondante à `this`.     |

---

### ⚙️ Comportement

#### Lecture (`get`)

* Calcule l’adresse mémoire du champ via base + offset.
* Interprète cette adresse comme un pointeur vers un `float`.
* Déréférence et retourne la valeur.

#### Écriture (`set`)

* Même logique pour obtenir l’adresse.
* Écrit la valeur `float` directement à cette adresse.

---

### 🧨 Risques

| Type de risque            | Détail                                                                      |
| ------------------------- | --------------------------------------------------------------------------- |
| 🔥 **Segmentation Fault** | Si `this` est nul ou non géré par IL2CPP.                                   |
| 💥 **Corruption mémoire** | Si le champ visé n'est pas réellement un `float`.                           |
| 🚫 **Incompatibilité**    | En cas de changement de structure côté Unity (champs déplacés ou renommés). |

---

### ✅ Utilisation typique

```csharp
var obj = new SomeIl2CppBoundComponent();
obj.SpeedMin = 3.5f;
float max = obj.SpeedMax;
```



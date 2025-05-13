**Rocket Boot Mania Documentation**

## Speed

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



## StoreAuthenticatedAccount

## 📄 Méthode : `StoreAuthenticatedAccount`

```csharp
public unsafe static void StoreAuthenticatedAccount(string mail, string name, string token)
```

### 🔍 Description

Appelle la méthode native IL2CPP `StoreAuthenticatedAccount(string, string, string)` dans l'environnement Unity, en passant trois chaînes de caractères : l'e-mail, le nom de l'utilisateur et un jeton d'authentification.

Cette méthode repose sur un appel bas niveau via le runtime IL2CPP, typiquement utilisé dans des contextes de modding, d'injection de comportements, ou de wrappers auto-générés autour de fonctions natives C++ dans Unity.

---

### 📥 Paramètres

| Nom     | Type     | Description                                    |
| ------- | -------- | ---------------------------------------------- |
| `mail`  | `string` | Adresse e-mail de l’utilisateur à enregistrer. |
| `name`  | `string` | Nom de l’utilisateur.                          |
| `token` | `string` | Jeton d'authentification sécurisé.             |

---

### ⚙️ Implémentation

La méthode effectue les opérations suivantes :

1. **Allocation de la pile pour les arguments**
   Un tableau de 3 pointeurs (`System.IntPtr*`) est alloué via `stackalloc` pour contenir les arguments à transmettre à la fonction native IL2CPP.

2. **Conversion des chaînes C# vers IL2CPP**
   Chaque string (`mail`, `name`, `token`) est convertie en une chaîne IL2CPP via `IL2CPP.ManagedStringToIl2Cpp(...)` puis stockée dans le tableau.

3. **Préparation du gestionnaire d'exception**
   Un pointeur `exc` est préparé pour recevoir toute exception native qui pourrait être levée lors de l’appel.

4. **Appel de la méthode native**
   La méthode native `StoreAuthenticatedAccount` est invoquée via `IL2CPP.il2cpp_runtime_invoke(...)`, en passant :

   * Un pointeur vers la définition native de la méthode (`NativeMethodInfoPtr_StoreAuthenticatedAccount_...`)
   * `null` comme instance cible, car la méthode est statique
   * Le tableau de 3 pointeurs comme arguments
   * Un pointeur vers l'éventuelle exception

5. **Propagation des erreurs natives**
   Si une exception IL2CPP est levée pendant l’exécution, elle est repropagée côté C# via `Il2CppException.RaiseExceptionIfNecessary`.

---

### 💡 Remarques techniques

* Cette méthode utilise le mot-clé `unsafe`, car elle manipule des pointeurs natifs et effectue des allocations mémoire sur la pile (`stackalloc`).
* `System.IntPtr` est utilisé pour représenter des pointeurs natifs en toute sécurité relative dans le runtime .NET.
* `checked` et `unchecked` entourent les calculs d’offsets mémoire pour assurer la compatibilité et la sécurité sur les plateformes 32/64 bits.
* La méthode native appelée est probablement définie dans le binaire Unity C++ (généré via IL2CPP), et référencée ici via une table de pointeurs (`NativeMethodInfoPtr_...`).

---

### 🛠 Exemple d’usage

```csharp
StoreAuthenticatedAccount("test@email.com", "PlayerName", "auth-token-123");
```

Cela va appeler la fonction native correspondante dans IL2CPP Unity pour enregistrer les informations d’un utilisateur authentifié.

## timer

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
 
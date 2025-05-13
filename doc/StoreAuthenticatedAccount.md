
---

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

---

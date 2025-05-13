---

## 🔧 Constructeur statique : `AISimpleMoveManager`

```csharp
static AISimpleMoveManager()
```

### 🔍 Description

Le constructeur statique est appelé **une seule fois**, automatiquement, lors du **premier accès à la classe `AISimpleMoveManager`** (instanciation ou appel de membre statique). Son rôle est de configurer les **liaisons natives IL2CPP**, en initialisant des pointeurs vers :

* La classe native IL2CPP (Unity C++)
* Les champs (variables membres)
* Les méthodes (fonctions membres)

---

### ⚙️ Fonctionnement

#### 1. 📦 Récupération de la classe native IL2CPP

```csharp
Il2CppClassPointerStore<AISimpleMoveManager>.NativeClassPtr = IL2CPP.GetIl2CppClass("Assembly-CSharp.dll", "", "AISimpleMoveManager");
```

* Charge la **définition native** de la classe `AISimpleMoveManager` depuis l'assembly `Assembly-CSharp.dll`.
* Le nom de l’espace de noms est vide `""`, donc la classe est dans le global namespace.
* Le pointeur vers la structure native est stocké dans `Il2CppClassPointerStore`.

#### 2. 🧠 Initialisation du runtime IL2CPP

```csharp
IL2CPP.il2cpp_runtime_class_init(...);
```

* Force l'initialisation de la classe dans le runtime IL2CPP (exécution du constructeur statique côté natif, si besoin).

#### 3. 📄 Liaison des champs

```csharp
NativeFieldInfoPtr_m_SimpleMoves = IL2CPP.GetIl2CppField(..., "m_SimpleMoves");
```

* Associe chaque champ nommé (ex: `m_SimpleMoves`) à son pointeur natif via `IL2CPP.GetIl2CppField`.
* Cela permet ensuite un accès mémoire direct (`unsafe`) dans d'autres parties du code.

Champs initialisés :

* `CONST_PLAYERWATCH_FARFPS`
* `CONST_PLAYERWATCH_FPS`
* `m_TargetDistance`
* `m_UpdateRateWhenOutOfReach`
* `m_UpdateRateWhenInReach`
* `m_SimpleMoves`
* `m_CurrentTargetTm`
* `m_CurrentTargetPlayer`
* `m_CurrentTargetPos`
* `m_CurrentTargetClass`
* `m_CurrentTargetReached`
* `m_CurrentTargetReachedPos`
* `m_UpdateDt`
* `m_CachedPath`

#### 4. 🧭 Liaison des méthodes natives

```csharp
NativeMethodInfoPtr_Start_Private_Void_0 = IL2CPP.GetIl2CppMethodByToken(..., 100664547);
```

* Associe des **fonctions membres** de `AISimpleMoveManager` à leurs pointeurs natifs via un **token** (`MetadataToken`) propre au système IL2CPP.
* Permet l’appel direct de la méthode via `il2cpp_runtime_invoke`.

Méthodes liées (exemples) :

* `Start`
* `FixedUpdate`
* `Update`
* `TargetReached`
* `DoCollision`
* `SendTargetReachable`
* `OnAISimpleMoveDestroyed`
* `GetPathToTarget`
* `GetSqrDistanceToTarget`
* etc.

---

### 🛠️ But de cette initialisation

Ce constructeur statique est essentiel pour permettre :

✅ Accès direct à la mémoire des instances natives Unity
✅ Appels de méthodes IL2CPP sans passer par une couche managée lourde
✅ Implémentation de wrappers rapides et performants en C#

---

### ⚠️ Attention

| Risque                 | Détail                                                                                                                         |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| ❌ Mauvais nom de champ | `GetIl2CppField` renverra `null` si le champ est mal orthographié.                                                             |
| ⚠️ Token obsolète      | `GetIl2CppMethodByToken` repose sur des `metadata tokens` extraits lors du reverse. Une mise à jour du jeu peut les invalider. |
| 💥 Segfault            | Une mauvaise gestion du pointeur peut provoquer des crashs mémoire.                                                            |

---

### ✅ Exemple d'utilisation

Une fois les pointeurs correctement initialisés :

```csharp
float dist = *(float*)((nint)IL2CPP.Il2CppObjectBaseToPtrNotNull(managerInstance) + (int)IL2CPP.il2cpp_field_get_offset(NativeFieldInfoPtr_m_TargetDistance));
```

ou

```csharp
IL2CPP.il2cpp_runtime_invoke(NativeMethodInfoPtr_Start_Private_Void_0, ptrToInstance, null, ref exc);
```

---


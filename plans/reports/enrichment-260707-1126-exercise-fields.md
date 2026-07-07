# Exercise Enrichment: gender / reps / duration / exerciseType

Added 4 fields to both `video_exercises.json` (584 items) and `gif_exercises.json` (1324 items).

## New Fields

| Field | Type | Example |
|---|---|---|
| `gender` | `[String]` | `["male", "female"]` |
| `repRange` | `[Int, Int]` | `[8, 12]` — `[0, 0]` for time-based |
| `durationRange` | `[Int, Int]` | `[30, 45]` seconds per set |
| `exerciseType` | `String` enum | `compound` |

## exerciseType Enum (6 cases)

`stretching` · `warm_up` · `compound` · `isolation` · `cardio` · `cardio_hiit`

### Detection Rules

**Video dataset** (has clean `category` + `mechanic`):
- name matches warm-up keywords → `warm_up`
- `category=plyometrics` → `cardio_hiit`
- `category=cardio` + HIIT keyword → `cardio_hiit`, else `cardio`
- `category=stretching` → `stretching`
- otherwise: `mechanic` value (compound/isolation), null → `compound`

**GIF dataset** (body-part `category`, no `mechanic`):
- name contains `stretch/mobility/release/smr/foam roll` → `stretching`
- warm-up keywords → `warm_up`
- HIIT keywords (burpee, jump, sprint, mountain climber, ...) → `cardio_hiit`
- `category=cardio` → `cardio` (or `cardio_hiit` if HIIT keyword)
- fallback: `len(secondary_muscles) >= 3` → `compound`, else `isolation`

### Distribution

| Type | Video | GIF |
|---|---|---|
| compound | 290 | 189 |
| isolation | 172 | 1028 |
| stretching | 71 | 58 |
| cardio_hiit | 37 | 22 |
| cardio | 10 | 21 |
| warm_up | 4 | 6 |

Note: GIF dataset skews heavily to `isolation` (78%) because the heuristic (secondary_muscles < 3) is conservative. Video's `mechanic` field gives cleaner compound/isolation split.

## Rep & Duration Rules

`repRange=[0, 0]` marks time-based exercises (cardio).

| exerciseType | Level | repRange | durationRange (s) |
|---|---|---|---|
| stretching | any | [1, 3] | [20, 30] |
| warm_up | any | [10, 15] | [30, 45] |
| cardio | any | [0, 0] | [60, 300] |
| cardio_hiit | any | [0, 0] | [20, 40] |
| compound | beginner | [8, 12] | [30, 45] |
| compound | intermediate | [6, 10] | [30, 60] |
| compound | expert | [3, 8] | [45, 90] |
| isolation | beginner | [10, 15] | [30, 45] |
| isolation | intermediate/expert | [8, 12] | [30, 45] |

GIF dataset has no `level` — defaults to intermediate.

## Gender

All items → `["male", "female"]` (unisex). Reason: fitness datasets don't encode gender-exclusive exercises; muscle training benefits both sexes. Field kept as array so UI can filter and future data can override individual items (e.g. `["female"]` for kegel-specific).

## Swift Enums

```swift
enum ExerciseType: String, Codable, CaseIterable {
    case stretching
    case warmUp = "warm_up"
    case compound
    case isolation
    case cardio
    case cardioHiit = "cardio_hiit"
}

enum Gender: String, Codable, CaseIterable {
    case male
    case female
}
```

## Model Additions

```swift
struct Exercise: Codable {
    // ... existing fields
    let gender: [Gender]
    let exerciseType: ExerciseType
    let repRange: [Int]        // [min, max]; [0, 0] = time-based
    let durationRange: [Int]   // [min, max] seconds per set
}
```

## Unresolved Questions

- GIF compound/isolation split (78% isolation) may be inaccurate — do you want to normalize using manual review or add a name-keyword override list (e.g. names with "squat", "deadlift", "press", "row" → compound)?
- Any exercise you want gender-restricted (e.g. kegel-style)? Currently all unisex.
- Should `warm_up` also apply to specific low-intensity variants of standard exercises (e.g. bodyweight squat = warm-up before barbell squat)?

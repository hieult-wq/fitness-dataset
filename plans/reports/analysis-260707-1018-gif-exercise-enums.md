# GIF Exercise Enums (gif_exercises.json)

Source: `gif_dataset/gif_exercises.json` (1324 items). **JSON đã được normalize** — muscle vocab đã unify.

## Resolved Decisions

1. **Muscle duplicates → normalized in JSON.** Applied mapping:
   - `trapezius` → `traps`
   - `latissimus dorsi` → `lats`
   - `pectorals` → `chest`
   - `delts` → `shoulders`
   - `deltoids` → `shoulders`
   - `quads` → `quadriceps`
   - `abs` → `abdominals`
2. **`muscle_group`, `target`, `secondary_muscles` → 1 canonical `Muscle` enum** (43 cases, union of 3 fields post-normalize).
3. **Equipment** — giữ full 28 cases.

## Summary

| Field | Unique | Nullable |
|---|---|---|
| category | 10 | no |
| equipment | 28 | no |
| muscle_group | 26 | no (uses `Muscle`) |
| target | 19 | no (uses `Muscle`) |
| secondary_muscles | 37 | no list (uses `Muscle`) |
| **Muscle (union)** | **43** | — |

## Counts

**category (10):** upper arms (292), upper legs (227), back (203), waist (169), chest (163), shoulders (143), lower legs (59), lower arms (37), cardio (29), neck (2)

**equipment (28):** body weight (325), dumbbell (294), cable (157), barbell (154), leverage machine (81), band (54), smith machine (48), kettlebell (41), weighted (36), stability ball (28), ez barbell (23), assisted (15), sled machine (15), medicine ball (13), rope (10), roller (8), resistance band (7), bosu ball (3), olympic barbell (2), wheel roller (2), elliptical machine (1), hammer (1), skierg machine (1), stationary bike (1), stepmill machine (1), tire (1), trap bar (1), upper body ergometer (1)

**Muscle (43, union counts across 3 fields):** shoulders (786), triceps (570), biceps (509), forearms (479), hamstrings (444), glutes (351), quadriceps (315), chest (302), calves (217), abdominals (173), traps (163), hip flexors (143), obliques (139), upper back (126), core (101), lats (90), lower back (77), rhomboids (56), cardiovascular system (29), ankles (22), rear deltoids (20), spine (19), brachialis (14), back (11), rotator cuff (10), soleus (8), feet (8), adductors (6), ankle stabilizers (5), serratus anterior (5), abductors (5), wrist flexors (4), wrists (4), wrist extensors (3), hands (3), upper chest (3), levator scapulae (2), sternocleidomastoid (2), groin (1), grip muscles (1), lower abs (1), inner thighs (1), shins (1)

## Swift Enums

```swift
enum Category: String, Codable, CaseIterable {
    case upperArms = "upper arms"
    case upperLegs = "upper legs"
    case back
    case waist
    case chest
    case shoulders
    case lowerLegs = "lower legs"
    case lowerArms = "lower arms"
    case cardio
    case neck
}

enum Equipment: String, Codable, CaseIterable {
    case bodyWeight = "body weight"
    case dumbbell
    case cable
    case barbell
    case leverageMachine = "leverage machine"
    case band
    case smithMachine = "smith machine"
    case kettlebell
    case weighted
    case stabilityBall = "stability ball"
    case ezBarbell = "ez barbell"
    case assisted
    case sledMachine = "sled machine"
    case medicineBall = "medicine ball"
    case rope
    case roller
    case resistanceBand = "resistance band"
    case bosuBall = "bosu ball"
    case olympicBarbell = "olympic barbell"
    case wheelRoller = "wheel roller"
    case ellipticalMachine = "elliptical machine"
    case hammer
    case skiergMachine = "skierg machine"
    case stationaryBike = "stationary bike"
    case stepmillMachine = "stepmill machine"
    case tire
    case trapBar = "trap bar"
    case upperBodyErgometer = "upper body ergometer"
}

enum Muscle: String, Codable, CaseIterable {
    case shoulders
    case triceps
    case biceps
    case forearms
    case hamstrings
    case glutes
    case quadriceps
    case chest
    case calves
    case abdominals
    case traps
    case hipFlexors = "hip flexors"
    case obliques
    case upperBack = "upper back"
    case core
    case lats
    case lowerBack = "lower back"
    case rhomboids
    case cardiovascularSystem = "cardiovascular system"
    case ankles
    case rearDeltoids = "rear deltoids"
    case spine
    case brachialis
    case back
    case rotatorCuff = "rotator cuff"
    case soleus
    case feet
    case adductors
    case ankleStabilizers = "ankle stabilizers"
    case serratusAnterior = "serratus anterior"
    case abductors
    case wristFlexors = "wrist flexors"
    case wrists
    case wristExtensors = "wrist extensors"
    case hands
    case upperChest = "upper chest"
    case levatorScapulae = "levator scapulae"
    case sternocleidomastoid
    case groin
    case gripMuscles = "grip muscles"
    case lowerAbs = "lower abs"
    case innerThighs = "inner thighs"
    case shins
}
```

## Exercise Model

```swift
struct GifExercise: Codable, Identifiable {
    let id: String
    let name: String
    let category: Category
    let bodyPart: Category  // same values as category
    let equipment: Equipment
    let muscleGroup: Muscle
    let target: Muscle
    let secondaryMuscles: [Muscle]
    let instructions: [String: String]  // {"en": "..."}
    let instructionSteps: [String]?
    let image: String?
    let gifUrl: String?
    let mediaId: String?
    let imagePath: String?

    enum CodingKeys: String, CodingKey {
        case id, name, category, equipment, target, image
        case bodyPart = "body_part"
        case muscleGroup = "muscle_group"
        case secondaryMuscles = "secondary_muscles"
        case instructions
        case instructionSteps = "instruction_steps"
        case gifUrl = "gif_url"
        case mediaId = "media_id"
        case imagePath = "image_path"
    }
}
```

# Exercise Enums (video_exercises.json)

Source: `video_dataset/video_exercises.json` (584 items).

## Summary

| Field | Unique | Nullable |
|---|---|---|
| level | 3 | no |
| mechanic | 2 | yes (57 items null) |
| category | 7 | no |
| force | 3 | yes (18 items null) |
| primaryMuscles | 17 | no |
| secondaryMuscles | 16 | no |

Note: `mechanic` and `force` contain `null` values in the JSON. Model them as `Optional` in Swift. All muscles found in `secondaryMuscles` are a subset of `primaryMuscles` (secondary lacks `neck`), so both fields can share the same `Muscle` enum.

## Counts

**level (3):** beginner (366), intermediate (185), expert (33)

**mechanic (2 + null):** compound (330), isolation (197), null (57)

**category (7):** strength (405), stretching (74), plyometrics (37), powerlifting (34), strongman (14), cardio (11), olympic weightlifting (9)

**force (3 + null):** push (270), pull (233), static (63), null (18)

**primaryMuscles (17):** quadriceps (99), shoulders (86), chest (58), triceps (57), abdominals (50), hamstrings (47), biceps (44), middle back (27), lats (25), lower back (19), calves (18), glutes (17), traps (11), adductors (9), forearms (6), neck (6), abductors (5)

**secondaryMuscles (16):** glutes (139), hamstrings (129), shoulders (129), calves (110), triceps (94), forearms (66), lower back (59), chest (52), biceps (49), lats (43), traps (42), middle back (40), quadriceps (39), adductors (32), abdominals (29), abductors (23)

## Swift Enums

```swift
enum Level: String, Codable, CaseIterable {
    case beginner
    case intermediate
    case expert
}

enum Mechanic: String, Codable, CaseIterable {
    case compound
    case isolation
}

enum Force: String, Codable, CaseIterable {
    case push
    case pull
    case `static`
}

enum Category: String, Codable, CaseIterable {
    case strength
    case stretching
    case plyometrics
    case powerlifting
    case strongman
    case cardio
    case olympicWeightlifting = "olympic weightlifting"
}

enum Muscle: String, Codable, CaseIterable {
    case abdominals
    case abductors
    case adductors
    case biceps
    case calves
    case chest
    case forearms
    case glutes
    case hamstrings
    case lats
    case lowerBack = "lower back"
    case middleBack = "middle back"
    case neck
    case quadriceps
    case shoulders
    case traps
    case triceps
}
```

## Exercise Model

```swift
struct Exercise: Codable, Identifiable {
    let id: String
    let name: String
    let level: Level
    let mechanic: Mechanic?
    let force: Force?
    let category: Category
    let equipment: String?
    let primaryMuscles: [Muscle]
    let secondaryMuscles: [Muscle]
    let instructions: [String]
    let images: [String]
}
```

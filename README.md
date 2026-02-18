# Cool Card Game

A strategic turn-based card game built in Unity featuring tactical card-based battles,
deck management, passive abilities, status effects, and multiple game modes.

## Features

- Turn-based combat with alternating player/enemy turns
- 40+ unique passive abilities with context-aware triggers
- Status effects system (buffs, debuffs, stacking mechanics)
- Three game modes: Versus AI, Local 1v1, Roguelike
- Object pooling for performance
- Animated battle text and visual feedback
- Persistent save system for player preferences
- Localization support: English, German, Spanish, Turkish
- Customizable card rendering (sprite, animator, video)

## Game Modes

| Mode | Description |
|------|-------------|
| **Versus AI** | Player vs AI opponent using EnemyDeckHandler |
| **Local 1v1** | Two human players alternate on same device |
| **Roguelike** | Progressive encounters; win cards to build your deck |

## Architecture Overview

| System | Scripts |
|--------|---------|
| Battle Orchestration | RoundManager, VersusAIManager, LocalVersusManager, RogueLikeManager |
| Card Entities | Card, CardValues, Hand, CardGameObjectPool |
| Deck Management | DeckHandler, EnemyDeckHandler |
| Abilities | PassiveManager, Passive, StatusEffectsManager, StatusEffectsHolder, StatusEffect |
| Positioning | CardPositionManager, CardArranger |
| Visuals | CardRenderer, CardOverlay, BattleTextManager, DescriptionBoxManager |
| Selection | SelectedCardManager |
| Audio | AudioManager |
| Persistence | SaveSystem, PlayerData, GeneralGameManager |

## Scripts Reference

| Script | Purpose |
|--------|---------|
| Card.cs | Core card entity: stats, actions, targeting, lifecycle |
| CardValues.cs | ScriptableObject card template (name, action, resistance, passives, visuals) |
| Hand.cs | Player hand UI, drag-to-play, hand positioning |
| DeckHandler.cs | Draw pile, discard pile, hand management |
| EnemyDeckHandler.cs | AI auto-draws and plays cards each round |
| RoundManager.cs | Central battle orchestrator: turn order, round progression, game state |
| GeneralGameManager.cs | Global settings: audio, display, language, scene transitions |
| PassiveManager.cs | 40+ passive ability evaluations via timing events |
| Passive.cs | ScriptableObject: ability name, value, timing flags, description |
| StatusEffect.cs | ScriptableObject: buff/debuff name, timing, sprite, count type |
| StatusEffectsManager.cs | Apply/calculate status effects on cards |
| StatusEffectsHolder.cs | Per-card active effects tracker with visual management |
| VersusAIManager.cs | Versus AI mode controller and win/loss conditions |
| LocalVersusManager.cs | Local 1v1 controller with turn alternation |
| RogueLikeManager.cs | Roguelike controller with encounter pool and deck building |
| BattleTextManager.cs | Floating animated text (damage numbers, status messages) |
| AudioManager.cs | Centralized SFX and music playback |
| CardArranger.cs | Card library UI with sorting (name, resistance, action) |
| CardPositionManager.cs | Battle arena card positioning, drag-and-drop, spot management |
| CardGameObjectPool.cs | Object pool for card GameObjects |
| CardRenderer.cs | Card visual rendering: sprite/animator/video, stats, passives |
| SelectedCardManager.cs | Mouse hover/selection detection via Physics2D |
| CardOverlay.cs | Visual state animations (selected, playing, targeted, healing) |
| DescriptionBoxManager.cs | Passive/status tooltip boxes with localization and keyword highlighting |
| PlayerData.cs | Serializable preferences structure (audio, display, language, speed) |
| SaveSystem.cs | Disk persistence for player preferences |

## How to Open in Unity

1. Open Unity Hub → Add Project from disk
2. Navigate to the project folder and open with Unity 2023.1.22f1
3. Wait for compilation
4. Open a scene from `Assets/Scenes/`
5. Press Play (Ctrl+P)

## Controls

| Input | Action |
|-------|--------|
| Left Click + Drag (hand) | Deploy card to battlefield |
| Left Click + Drag (field) | Reorder card position |
| Hover over card | Show selection highlight |
| Hover over passive icon | Show passive description tooltip |
| Ready Button | Confirm placement and start round |
| Draw Button | Draw a card from deck |

## Key Concepts

- **Action Value** — damage dealt or healing amount
- **Resistance Value** — card health / durability
- **Passive Abilities** — permanent abilities triggering on timing events (OnPlay, OnAttack, OnHurt, OnDeath, StartOfTurn, EndOfTurn, etc.)
- **Status Effects** — temporary buffs/debuffs with stacking or duration
- **Turn Order** — cards alternate between teams; each card runs StartOfTurn → Action → EndOfTurn

## System Interaction Map

```
GeneralGameManager (persistent)
    └── Loads/Saves PlayerData via SaveSystem

[Game Mode Manager] (VersusAIManager / LocalVersusManager / RogueLikeManager)
    ├── DeckHandler        ← manages player deck draw/discard/hand
    ├── EnemyDeckHandler   ← manages AI deck
    └── RoundManager       ← orchestrates all battle flow
           ├── Card[]        ← player cards on field
           ├── Card[]        ← enemy cards on field
           ├── PassiveManager ← evaluates passive triggers per timing event
           └── StatusEffectsManager ← calculates/applies status effects

Card (entity)
    ├── CardValues (ScriptableObject config)
    ├── CardRenderer      ← visuals
    ├── CardOverlay       ← state animations
    └── StatusEffectsHolder ← active effects

UI Layer
    ├── Hand              ← drag-and-drop from hand
    ├── CardPositionManager ← arena spot management
    ├── SelectedCardManager ← hover/selection via Physics2D
    ├── DescriptionBoxManager ← tooltips
    └── BattleTextManager ← floating damage/heal text

Audio Layer
    └── AudioManager      ← persistent singleton for SFX + music
```

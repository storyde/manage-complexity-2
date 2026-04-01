# Achievements and Multi-Track Progress Plan

## Goal

Replace the single visible progress bar with a small progress dashboard that feels more like a game system while still fitting the reflective tone of the project.

Keep the existing `progress` variable as the hidden backbone for scene flow and compatibility, but introduce separate track variables and qdisplay files for the visible progression system.

## Recommended Track Model

### Keep

- `progress` remains the master scene-sequencing variable
- existing scene gating based on `progress` stays intact during the migration

### Add

- `jrny-progress` for **Voyage Through Complexity**
- `hctx-progress` for **Holistic Context Forged**
- `fltr-progress` for **Decision Checks Mastered**
- `knwl-progress` for **Concepts Understood**

## New Qdisplay Files

Create one qdisplay per track:

- `source/qdisplays/qjrny_progress.qdisplay.dry`
- `source/qdisplays/qhctx_progress.qdisplay.dry`
- `source/qdisplays/qfltr_progress.qdisplay.dry`
- `source/qdisplays/qknwl_progress.qdisplay.dry`

The current `qprogress.qdisplay.dry` can remain temporarily during migration and then be removed after the new sidebar layout is stable.

## Track Definitions

### 1. Voyage Through Complexity

Variable: `jrny-progress`

Purpose:

- track narrative travel and chapter completion
- replace the current single story bar with something more thematic

Recommended scale:

- `0..5`

Milestones:

- `0` Not started
- `1` First Landing
- `2` Tea Companion
- `3` Context Keeper
- `4` Filter Navigator
- `5` Long View

Why a short scale works:

- it feels like a chapter map, not an admin counter
- it aligns naturally with the strongest milestone achievements
- it avoids mirroring the old `progress` system too literally

Suggested icon:

- `⛵`

### 2. Holistic Context Forged

Variable: `hctx-progress`

Purpose:

- reward the player for building the actual framework
- make the left sidebar reflect real construction, not just story order

Recommended scale:

- `0..12`

Recommended increments:

- `1` decision-makers chosen
- `2` physical resources chosen
- `3` human resources chosen
- `4` money defined
- `5` statement of purpose complete for organisations, or no award for non-org play
- `6` quality of life economy chosen
- `7` quality of life relationships chosen
- `8` quality of life challenge chosen
- `9` quality of life growth chosen
- `10` quality of life purpose chosen
- `11` quality of life contribution or accomplishment cluster complete
- `12` future resource base complete and holistic context forged

Implementation note:

The exact split can be adjusted, but the bar should track substantive framework-building steps rather than every small scene transition.

Suggested icon:

- `✧`

### 3. Decision Checks Mastered

Variable: `fltr-progress`

Purpose:

- show mastery of the seven filter questions in a clean, meaningful way

Recommended scale:

- `0..7`

Recommended increments:

- `1` Cause and Effect
- `2` Weak Link complete
- `3` Marginal Reaction
- `4` Gross Profit
- `5` Source and Use complete
- `6` Sustainability complete
- `7` Gut Feel complete

Important design choice:

This track should count conceptual filter completion, not every individual sub-answer.

That means:

- Weak Link advances only when all required weak-link parts are done
- Source and Use advances only when both source and use are done
- Sustainability advances only when both behaviour and environment are done

This is much better than a raw `0..11` counter because it matches the framework and feels more meaningful.

Suggested icon:

- `🧭`

### 4. Concepts Understood

Variable: `knwl-progress`

Purpose:

- reward learning moments
- make the educational side of the story feel collectible and visible

Recommended scale:

- `0..12`

Recommended knowledge beats:

- `1` Complexity introduced
- `2` Whole Under Management introduced
- `3` Decision makers introduced
- `4` Resource base introduced
- `5` Holistic context introduced
- `6` Filter questions introduced
- `7` Cause and effect explained
- `8` Weak link explained
- `9` Marginal reaction explained
- `10` Gross profit explained
- `11` Energy or money source and use explained
- `12` Sustainability and gut feel arc completed

Suggested icon:

- `📘`

## Recommendation for Knowledge Progress

The idea of incrementing knowledge progress when the vendor or Facilyn explains something is good, but it needs guardrails so the value does not increase repeatedly on revisits.

`knwl-progress += 1`


I do not want to use the JavaScript block form described in the Dendry syntax reference:

```dry
on-arrival: {! Q.quality1 = 10; Q.quality2 = Q.quality1 * 2 !}
```

So the planning assumption should be:

- `knwl-progress` is only advanced at one-time unlock moments
- each insight needs its own unlock flag so the progress does not increase on revisits
- the UI should read from those unlock flags to show the latest unlocked insight and the concept chips

### Additional variables for insight unlocks

Yes, we should introduce more variables for knowledge timing.

The four track variables are not enough on their own, because the game also needs to know:

- whether an insight has already been unlocked
- which insight was unlocked most recently
- which small concept chips are eligible to appear in the left sidebar

Recommended additional knowledge-state variables:

- `insight-last`
- `insight-last-icon`
- `insight-complexity`
- `insight-whole`
- `insight-decision-makers`
- `insight-resource-base`
- `insight-holistic-context`
- `insight-filters`
- `insight-cause-effect`
- `insight-weak-link`
- `insight-marginal-reaction`
- `insight-gross-profit`
- `insight-source-use`
- `insight-sustainability`
- `insight-gut-feel`

Recommended use:

- each `insight-*` variable acts as a one-time unlock flag
- `insight-last` stores the label for the latest unlocked insight
- `insight-last-icon` stores the icon shown next to the latest insight line

This allows the left sidebar to cleanly show lines such as:

- `New insight: Weak Link`
- `New insight: Cause and Effect`
- `New insight: Whole Under Management`

And it also allows the right sidebar to unlock full codex entries at the correct time.

## Achievement System

### Core achievement state

Add simple qualities for achievement tracking:

- `achievement-last`
- `achievement-last-icon`
- `achievement-count`
- `achievement-first-landing`
- `achievement-tea-companion`
- `achievement-context-keeper`
- `achievement-filter-navigator`
- `achievement-long-view`
- `achievement-systems-thinker`
- `achievement-weak-link-spotter`

Optional:

- `achievement-no-loose-ends`
- `achievement-framework-builder`
- `achievement-full-context`

### Badge philosophy

Avoid arcade-style achievements.

Use achievements as moments of recognition for understanding, completion, and good stewardship.

### Improved milestone achievements

#### First Landing

Unlock when:

- the harbour is reached

Narrative role:

- first sense of arrival and entry into the world

Suggested icon:

- `⚓`

Sidebar line:

- `New achievement: First Landing`

#### Tea Companion

Unlock when:

- the teahouse is reached

Narrative role:

- marks entry into guided learning and conversation

Suggested icon:

- `☕`

Sidebar line:

- `New achievement: Tea Companion`

#### Context Keeper

Unlock when:

- the Holistic Context is fully completed

Narrative role:

- the player has moved from information gathering to coherent design

Suggested icon:

- `✧`

Sidebar line:

- `New achievement: Context Keeper`

#### Filter Navigator

Unlock when:

- all seven filter-question clusters are complete

Narrative role:

- the player can now evaluate decisions instead of just naming goals

Suggested icon:

- `🧭`

Sidebar line:

- `New achievement: Filter Navigator`

#### Long View

Unlock when:

- the outro is reached

Narrative role:

- closes the arc by emphasizing long-term stewardship

Suggested icon:

- `✦`

Sidebar line:

- `New achievement: Long View`

### Knowledge achievements

These already fit well and should remain:

- **Systems Thinker** — unlock after both complexity and whole-under-management knowledge are unlocked
- **Weak Link Spotter** — unlock after the weak-link explanation is unlocked

Good additions:

- **Context Crafter** — unlock when holistic context knowledge is introduced
- **Source and Use** — unlock when energy/money explanation is unlocked
- **Grounded Intuition** — unlock when the gut-feel explanation is unlocked after the previous filter arc

## Sidebar Layout Plan

Replace the current single top block in `progress.scene.dry` with a compact dashboard.

### Left sidebar

The left sidebar should remain the player-state and decision-support area.

Recommended contents:

- progress tracks
- latest achievement
- compact Holistic Context summary
- current decision or current filter status
- maybe 2–3 tiny unlocked concept chips
- latest unlocked insight line

Example latest insight lines:

- `New insight: Weak Link`
- `New insight: Cause and Effect`
- `New insight: Whole Under Management`

### Right sidebar

The right sidebar should remain the knowledge and reference area.

Recommended contents:

- full knowledge codex
- unlocked explanations
- deeper concept descriptions
- optional grouped sections such as `Foundations`, `Context`, and `Filters`

### Recommended order

1. dashboard title
2. four track cards
3. latest achievement line
4. latest unlocked insight line
5. earned achievement badges
6. compact Holistic Context summary
7. current decision or current filter status
8. optional concept chips
9. final summary card once the framework is complete

### Dashboard title

- `Your Framework Journey`

### Track cards

Each card should contain:

- icon
- track title
- qdisplay-rendered progress bar
- short status text
- completed state styling when full

### Track titles

- `⛵ Voyage Through Complexity`
- `✧ Holistic Context Forged`
- `🧭 Decision Checks Mastered`
- `📘 Concepts Understood`

### Achievement line

Show a short line directly under the tracks:

- `✦ New achievement: Context Keeper`

This line does not need to be truly temporary at first.

Version 1 can simply show the most recently unlocked achievement until another one replaces it.

That gives the effect without introducing complex expiry logic.

### Latest insight line

Show a short line for the most recently unlocked knowledge beat:

- `📘 New insight: Weak Link`
- `📘 New insight: Cause and Effect`
- `📘 New insight: Whole Under Management`

Version 1 should keep this simple:

- one line only
- replaced whenever a newer insight unlocks
- driven by `insight-last` and optionally `insight-last-icon`

### Earned badges

Show compact earned badges only for unlocked achievements.

Badges should be short and elegant:

- `⚓ First Landing`
- `☕ Tea Companion`
- `✧ Context Keeper`
- `🧭 Filter Navigator`
- `✦ Long View`

### Concept chips

Add 2–3 very small chips for recently relevant unlocked concepts.

Examples:

- `Whole Under Management`
- `Weak Link`
- `Cause and Effect`

These should be:

- short
- optional
- secondary to the main track cards

They should hint at unlocked knowledge without replacing the right-sidebar codex.

## Completed State and Animation

The completion effect should be subtle and celebratory.

### Visual behavior

- when a track reaches its max value, append `✦ Complete`
- apply a short shimmer effect to that card or bar
- do not animate continuously after the initial moment

### CSS approach

Add classes for:

- track card
- track card complete state
- achievement badge
- new achievement line
- shimmer effect

The shimmer should be:

- short
- elegant
- aligned with the existing glass-and-harbour aesthetic

No confetti, no looping sparkle storm, and no loud arcade treatment.

## Stronger Naming Changes

Replace utilitarian labels where possible.

### Replace

- `Creation Prozess`
- `Filter Questions`

### With

- `Voyage Through Complexity`
- `Holistic Context Forged`
- `Decision Checks Mastered`
- `Concepts Understood`

These should appear in the visible dashboard even if the underlying scene structure stays the same.

## Final Summary Card

When the player reaches the end, show a summary card in the sidebar or outro section that answers:

- who is the whole under management
- what quality of life they defined
- what future resource base they committed to
- what action they tested
- what achievements they earned

Suggested card title:

- `What You Built`

This gives closure and makes the final state feel earned.

## File-by-File Implementation Plan

### 1. `source/scenes/root.scene.dry`

Initialize all new visible-progress variables and achievement variables.

Recommended initial values:

- `jrny-progress = 0`
- `hctx-progress = 0`
- `fltr-progress = 0`
- `knwl-progress = 0`
- `achievement-count = 0`
- `insight-last = 0`
- `insight-last-icon = 0`

Initialize achievement flags to `0` only if needed for clarity.

Also initialize the one-time insight unlock flags only if explicit initialization makes the implementation easier to read.

### 2. Narrative milestone scenes

Update key scenes to award journey progress and milestone achievements:

- `3_harbour.scene.dry`
- `4_teahouse.scene.dry`
- `22_holtext.scene.dry`
- `31_fq_result.scene.dry` or `32_fq_revisit.scene.dry`
- `33_framework_outro.scene.dry`

### 3. Knowledge scenes

Add guarded one-time knowledge unlock logic to the scenes where explanation beats happen.

Primary candidates:

- `4_teahouse.scene.dry`
- `5_wum_intro.scene.dry`
- `23_fq_intro.scene.dry`
- `24_fq_cause_effect.scene.dry`
- `25_fq_weak_link.scene.dry`
- `26_fq_marginal_reaction.scene.dry`
- `27_fq_gross_profit.scene.dry`
- `28_fq_energy_money.scene.dry`
- `29_fq_sustainability.scene.dry`
- `30_fq_gut_feel.scene.dry`

### 4. Context scenes

Increment context progress only at meaningful completion points rather than every scene entry.

Primary candidates:

- `7_wum_dmkrs_one_group.scene.dry`
- `8_wum_dmkrs_org.scene.dry`
- `9_wum_rb_physical.scene.dry`
- `10_wum_rb_human.scene.dry`
- `11_wum_money.scene.dry`
- `12_statement_of_purpose.scene.dry`
- `14_qol_economic_wellbeing.scene.dry`
- `15_qol_relationships.scene.dry`
- `16_qol_challenge_growth.scene.dry`
- `17_qol_purpose.scene.dry`
- `18_qol_contribution.scene.dry`
- `20_frb_people.scene.dry`
- `21_frb_environment.scene.dry`
- `22_holtext.scene.dry`

### 5. Filter scenes

Update filter progress when each conceptual filter cluster is complete.

Primary candidates:

- `24_fq_cause_effect.scene.dry`
- `25_fq_weak_link.scene.dry`
- `26_fq_marginal_reaction.scene.dry`
- `27_fq_gross_profit.scene.dry`
- `28_fq_energy_money.scene.dry`
- `29_fq_sustainability.scene.dry`
- `30_fq_gut_feel.scene.dry`

### 6. `source/scenes/progress.scene.dry`

Refactor the top of the sidebar into the new dashboard and keep the lower written recap.

This file becomes the main composition layer for:

- track cards
- latest achievement line
- earned badges
- final summary card

### 7. New qdisplay files

Implement one display file per visible track.

Each should output:

- title
- `<progress>` element
- short status line
- `✦ Complete` when maxed

### 8. CSS layer

Add styling for the new dashboard elements in the project stylesheet used by the generated HTML.

Main targets:

- progress dashboard container
- track cards
- complete state
- shimmer animation
- achievement badges
- final summary card

## Recommended Delivery Order

### Phase 1

- add new variables
- add four qdisplay files
- replace the single progress display in the sidebar

### Phase 2

- wire up journey, context, filter, and knowledge progression
- add milestone achievements

### Phase 3

- add knowledge achievements
- add latest-achievement line
- add earned-badge rendering

### Phase 4

- add complete-state shimmer styling
- add final summary card
- remove old single-bar UI once everything is stable

## Validation Checklist

After implementation, verify:

- all new variables initialize correctly on a fresh run
- revisiting scenes does not duplicate progress or achievements
- non-organisation runs still complete the context track correctly
- weak link, source/use, and sustainability only award filter progress when their clusters are complete
- knowledge progress only increments once per concept
- the sidebar remains readable in both themes
- completion shimmer appears only on full completion states
- the final summary card renders meaningful content for all player modes

## Final Recommendation

The most important design choice is this:

Do not treat the new system as four decorative copies of the old progress bar.

Each track should represent a different kind of accomplishment:

- narrative travel
- framework construction
- decision mastery
- learning and insight

That difference is what will make the experience feel exciting instead of merely more crowded.

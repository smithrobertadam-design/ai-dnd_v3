// ============================================================
// AI DUNGEON MASTER — Production Build v2.0
// Architecture: Mobile-first, Apple HIG compliant
// AI: Collaborative narrative engine with full world state
// ============================================================

import { useState, useEffect, useRef, useCallback, useReducer } from "react";

// ─── DESIGN TOKENS ─────────────────────────────────────────
const T = {
  bg:        "#08060a",
  bgDeep:    "#050408",
  surface:   "rgba(18,13,24,0.97)",
  surfaceAlt:"rgba(25,18,35,0.9)",
  border:    "rgba(180,130,60,0.22)",
  borderHot: "rgba(220,170,80,0.7)",
  gold:      "#c8a44a",
  goldBright:"#e8c870",
  goldDim:   "rgba(200,164,74,0.5)",
  textPrimary:  "#ede0c4",
  textSecondary:"#9a8a6a",
  textMuted:    "rgba(154,138,106,0.5)",
  hp:        "#3d9e58",
  hpLow:     "#c44040",
  hpMid:     "#c08030",
  magic:     "#7060b8",
  magicBright:"#a090e8",
  enemy:     "#b83030",
  ally:      "#3060a8",
  s1:"4px", s2:"8px", s3:"12px", s4:"16px", s5:"20px", s6:"24px",
  xs:"10px", sm:"12px", md:"14px", lg:"16px", xl:"20px", xxl:"26px",
  r1:"6px", r2:"10px", r3:"16px", r4:"24px",
  display:"'Cinzel Decorative', serif",
  heading:"'Cinzel', serif",
  body:"'Crimson Text', serif",
  mono:"'Courier New', monospace",
};

// ─── WORLD STATE ───────────────────────────────────────────
const createWorldState = () => ({
  session: {
    id: Date.now().toString(36),
    turn: 0,
    round: 0,
    phase: "exploration",
    location: "Unknown",
    time: "Dawn",
    weather: "Clear",
  },
  character: null,
  combat: {
    active: false,
    round: 1,
    initiative: [],
    currentTurnIndex: 0,
    log: [],
  },
  npcs: [],
  quests: [],
  worldFacts: [],
  inventory: [],
  gold: 10,
  storyContext: "",
  consequences: [],
});

// ─── GAME STATE REDUCER ────────────────────────────────────
const gameReducer = (state, action) => {
  switch (action.type) {
    case "SET_PHASE":
      return { ...state, session: { ...state.session, phase: action.phase } };
    case "START_COMBAT":
      return { ...state, combat: { ...state.combat, active: true, round: 1, initiative: action.initiative, currentTurnIndex: 0, log: ["⚔️ Combat begins!"] } };
    case "END_COMBAT":
      return { ...state, combat: { ...state.combat, active: false, initiative: [], log: [] }, session: { ...state.session, phase: "exploration" } };
    case "NEXT_TURN": {
      const nextIdx = (state.combat.currentTurnIndex + 1) % state.combat.initiative.length;
      const newRound = nextIdx === 0 ? state.combat.round + 1 : state.combat.round;
      return { ...state, combat: { ...state.combat, currentTurnIndex: nextIdx, round: newRound } };
    }
    case "UPDATE_COMBAT_HP":
      return { ...state, combat: { ...state.combat, initiative: state.combat.initiative.map(e => e.name === action.name ? { ...e, hp: Math.max(0, Math.min(e.maxHp, e.hp + action.delta)) } : e) } };
    case "ADD_COMBAT_LOG":
      return { ...state, combat: { ...state.combat, log: [action.entry, ...state.combat.log].slice(0, 30) } };
    case "UPDATE_CHAR_HP":
      return {
        ...state,
        character: state.character ? {
          ...state.character,
          hp: { ...state.character.hp, current: Math.max(0, Math.min(state.character.hp.max, state.character.hp.current + action.delta)) }
        } : state.character
      };
    case "ADD_NPC":
      return { ...state, npcs: [...state.npcs.filter(n => n.name !== action.npc.name), action.npc] };
    case "ADD_QUEST":
      return { ...state, quests: [...state.quests, action.quest] };
    case "UPDATE_QUEST":
      return { ...state, quests: state.quests.map(q => q.title === action.title ? { ...q, ...action.updates } : q) };
    case "ADD_WORLD_FACT":
      return { ...state, worldFacts: [...state.worldFacts, action.fact].slice(-20) };
    case "UPDATE_LOCATION":
      return { ...state, session: { ...state.session, location: action.location, time: action.time || state.session.time } };
    case "ADD_CONSEQUENCE":
      return { ...state, consequences: [...state.consequences, action.consequence].slice(-10) };
    case "UPDATE_INVENTORY":
      return { ...state, inventory: action.inventory };
    case "UPDATE_GOLD":
      return { ...state, gold: Math.max(0, state.gold + action.delta) };
    case "UPDATE_STORY_CONTEXT":
      return { ...state, storyContext: action.context };
    // SET_SETTING is handled in the root App via fullDispatch — not in this reducer
    default:
      return state;
  }
};

// ─── D&D MATH ENGINE ──────────────────────────────────────
const DnD = {
  rollDie: (sides) => Math.floor(Math.random() * sides) + 1,
  roll: (count, sides, bonus = 0) => { let t = bonus; for (let i = 0; i < count; i++) t += Math.floor(Math.random() * sides) + 1; return t; },
  rollAdvantage: (sides) => Math.max(Math.floor(Math.random() * sides) + 1, Math.floor(Math.random() * sides) + 1),
  rollDisadvantage: (sides) => Math.min(Math.floor(Math.random() * sides) + 1, Math.floor(Math.random() * sides) + 1),
  mod: (score) => Math.floor((score - 10) / 2),
  modStr: (m) => m >= 0 ? `+${m}` : `${m}`,
  prof: (level) => Math.ceil(level / 4) + 1,
  roll4d6: () => { const r = [1, 2, 3, 4].map(() => Math.floor(Math.random() * 6) + 1); return r.sort((a, b) => b - a).slice(0, 3).reduce((a, b) => a + b, 0); },
  hitDice: { Barbarian: 12, Bard: 8, Cleric: 8, Druid: 8, Fighter: 10, Monk: 8, Paladin: 10, Ranger: 10, Rogue: 8, Sorcerer: 6, Warlock: 8, Wizard: 6, Artificer: 8 },
  calcMaxHP: (classes, conScore) => {
    const conMod = Math.floor((conScore - 10) / 2);
    return classes.reduce((total, cls, idx) => {
      const hd = DnD.hitDice[cls.name] || 8;
      if (idx === 0) return total + hd + conMod + (cls.level - 1) * (Math.floor(hd / 2) + 1 + conMod);
      return total + cls.level * (Math.floor(hd / 2) + 1 + conMod);
    }, 0);
  },
};

// ─── DATA TABLES ───────────────────────────────────────────
const DATA = {
  races: [
    { name: "Human", source: "PHB", ab: { str: 1, dex: 1, con: 1, int: 1, wis: 1, cha: 1 }, spd: 30, sz: "Medium", traits: ["Extra Language", "Skill Versatility"] },
    { name: "Human (Variant)", source: "PHB", ab: {}, spd: 30, sz: "Medium", traits: ["One Skill", "One Feat", "Extra Language"] },
    { name: "Elf (High)", source: "PHB", ab: { dex: 2, int: 1 }, spd: 30, sz: "Medium", traits: ["Darkvision 60ft", "Fey Ancestry", "Trance", "Keen Senses", "Cantrip"] },
    { name: "Elf (Wood)", source: "PHB", ab: { dex: 2, wis: 1 }, spd: 35, sz: "Medium", traits: ["Darkvision 60ft", "Fey Ancestry", "Fleet of Foot", "Mask of the Wild"] },
    { name: "Elf (Drow)", source: "PHB", ab: { dex: 2, cha: 1 }, spd: 30, sz: "Medium", traits: ["Superior Darkvision 120ft", "Sunlight Sensitivity", "Drow Magic"] },
    { name: "Dwarf (Hill)", source: "PHB", ab: { con: 2, wis: 1 }, spd: 25, sz: "Medium", traits: ["Darkvision 60ft", "Dwarven Resilience", "Stonecunning", "Dwarven Toughness +1HP/level"] },
    { name: "Dwarf (Mountain)", source: "PHB", ab: { con: 2, str: 2 }, spd: 25, sz: "Medium", traits: ["Darkvision 60ft", "Dwarven Armor Training", "Dwarven Resilience"] },
    { name: "Halfling (Lightfoot)", source: "PHB", ab: { dex: 2, cha: 1 }, spd: 25, sz: "Small", traits: ["Lucky", "Brave", "Halfling Nimbleness", "Naturally Stealthy"] },
    { name: "Halfling (Stout)", source: "PHB", ab: { dex: 2, con: 1 }, spd: 25, sz: "Small", traits: ["Lucky", "Brave", "Halfling Nimbleness", "Stout Resilience"] },
    { name: "Gnome (Rock)", source: "PHB", ab: { int: 2, con: 1 }, spd: 25, sz: "Small", traits: ["Darkvision", "Gnome Cunning", "Artificer's Lore", "Tinker"] },
    { name: "Gnome (Forest)", source: "PHB", ab: { int: 2, dex: 1 }, spd: 25, sz: "Small", traits: ["Darkvision", "Gnome Cunning", "Natural Illusionist", "Speak with Small Beasts"] },
    { name: "Half-Elf", source: "PHB", ab: { cha: 2 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Fey Ancestry", "Skill Versatility (2 skills)"] },
    { name: "Half-Orc", source: "PHB", ab: { str: 2, con: 1 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Menacing", "Relentless Endurance", "Savage Attacks"] },
    { name: "Tiefling", source: "PHB", ab: { int: 1, cha: 2 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Hellish Resistance (fire)", "Infernal Legacy"] },
    { name: "Dragonborn", source: "PHB", ab: { str: 2, cha: 1 }, spd: 30, sz: "Medium", traits: ["Draconic Ancestry", "Breath Weapon", "Damage Resistance"] },
    { name: "Aasimar", source: "VGtM", ab: { wis: 1, cha: 2 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Celestial Resistance", "Healing Hands", "Light Bearer"] },
    { name: "Tabaxi", source: "VGtM", ab: { dex: 2, cha: 1 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Feline Agility", "Cat's Claws", "Cat's Talent"] },
    { name: "Kenku", source: "VGtM", ab: { dex: 2, wis: 1 }, spd: 30, sz: "Medium", traits: ["Expert Forgery", "Kenku Training", "Mimicry"] },
    { name: "Firbolg", source: "VGtM", ab: { wis: 2, str: 1 }, spd: 30, sz: "Medium", traits: ["Firbolg Magic", "Hidden Step", "Powerful Build", "Speech of Beast and Leaf"] },
    { name: "Goliath", source: "VGtM", ab: { str: 2, con: 1 }, spd: 30, sz: "Medium", traits: ["Stone's Endurance", "Powerful Build", "Mountain Born"] },
    { name: "Lizardfolk", source: "VGtM", ab: { con: 2, wis: 1 }, spd: 30, sz: "Medium", traits: ["Cunning Artisan", "Hold Breath", "Natural Armor", "Hungry Jaws"] },
    { name: "Triton", source: "VGtM", ab: { str: 1, con: 1, cha: 1 }, spd: 30, sz: "Medium", traits: ["Amphibious", "Control Air and Water", "Emissary of the Sea"] },
    { name: "Bugbear", source: "VGtM", ab: { str: 2, dex: 1 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Long-Limbed", "Surprise Attack"] },
    { name: "Goblin", source: "VGtM", ab: { dex: 2, con: 1 }, spd: 30, sz: "Small", traits: ["Darkvision", "Fury of the Small", "Nimble Escape"] },
    { name: "Hobgoblin", source: "VGtM", ab: { con: 2, int: 1 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Martial Training", "Saving Face"] },
    { name: "Yuan-ti Pureblood", source: "VGtM", ab: { int: 1, cha: 2 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Innate Spellcasting", "Magic Resistance", "Poison Immunity"] },
    { name: "Eladrin", source: "MToF", ab: { dex: 2, int: 1 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Fey Ancestry", "Fey Step (teleport 30ft)", "Trance"] },
    { name: "Githyanki", source: "MToF", ab: { str: 2, int: 1 }, spd: 30, sz: "Medium", traits: ["Martial Prodigy", "Githyanki Psionics", "Astral Knowledge"] },
    { name: "Githzerai", source: "MToF", ab: { wis: 2, int: 1 }, spd: 30, sz: "Medium", traits: ["Mental Discipline", "Githzerai Psionics", "Psychic Resilience"] },
    { name: "Custom Lineage", source: "TCoE", ab: {}, spd: 30, sz: "Medium", traits: ["Choose 1 Feat", "Darkvision (optional)", "1 Skill Proficiency"] },
    { name: "Fairy", source: "WBtW", ab: { dex: 1, wis: 1, cha: 1 }, spd: 30, sz: "Small", traits: ["Fairy Magic", "Flight 30ft", "Dragonfly Wings"] },
    { name: "Harengon", source: "WBtW", ab: {}, spd: 30, sz: "Medium", traits: ["Hare-Trigger", "Leporine Senses", "Lucky Footwork", "Rabbit Hop"] },
    { name: "Owlin", source: "SCoC", ab: { wis: 2 }, spd: 30, sz: "Medium", traits: ["Darkvision 120ft", "Flight 30ft", "Silent Feathers"] },
    { name: "Dhampir", source: "VRGtR", ab: {}, spd: 30, sz: "Medium", traits: ["Darkvision", "Deathless Nature", "Spider Climb", "Vampiric Bite"] },
    { name: "Warforged", source: "ERftLW", ab: { con: 2 }, spd: 30, sz: "Medium", traits: ["Integrated Protection", "Sentry's Rest", "Constructed Resilience", "Specialized Design"] },
    { name: "Changeling", source: "ERftLW", ab: { cha: 2 }, spd: 30, sz: "Medium", traits: ["Changeling Instincts", "Shapechanger"] },
    { name: "Kalashtar", source: "ERftLW", ab: { wis: 1, cha: 2 }, spd: 30, sz: "Medium", traits: ["Dual Mind", "Mental Discipline", "Mind Link", "Severed from Dreams"] },
    { name: "Aarakocra", source: "MPMM", ab: { dex: 2, wis: 1 }, spd: 25, sz: "Medium", traits: ["Flight 50ft", "Talons 1d4"] },
    { name: "Genasi (Air)", source: "MPMM", ab: { con: 2, dex: 1 }, spd: 35, sz: "Medium", traits: ["Unending Breath", "Lightning Resistance", "Mingle with Wind"] },
    { name: "Genasi (Earth)", source: "MPMM", ab: { con: 2, str: 1 }, spd: 30, sz: "Medium", traits: ["Earth Walk", "Merge with Stone"] },
    { name: "Genasi (Fire)", source: "MPMM", ab: { con: 2, int: 1 }, spd: 30, sz: "Medium", traits: ["Fire Resistance", "Reach to the Blaze"] },
    { name: "Genasi (Water)", source: "MPMM", ab: { con: 2, wis: 1 }, spd: 30, sz: "Medium", traits: ["Amphibious", "Swim 30ft", "Call to the Wave"] },
    { name: "Tortle", source: "TP", ab: { str: 2, wis: 1 }, spd: 30, sz: "Medium", traits: ["Claws 1d4", "Hold Breath 1hr", "Natural Armor AC 17", "Shell Defense"] },
    { name: "Loxodon", source: "GGtR", ab: { con: 2, wis: 1 }, spd: 30, sz: "Medium", traits: ["Powerful Build", "Loxodon Serenity", "Natural Armor", "Trunk", "Keen Smell"] },
    { name: "Simic Hybrid", source: "GGtR", ab: { con: 2, dex: 1 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Animal Enhancement (choose 2)"] },
    { name: "Vedalken", source: "GGtR", ab: { int: 2, wis: 1 }, spd: 30, sz: "Medium", traits: ["Vedalken Dispassion", "Tireless Precision", "Partially Amphibious"] },
    { name: "Shifter (Beasthide)", source: "ERftLW", ab: { con: 2, str: 1 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Shifting: +1d6 HP, +1 AC"] },
    { name: "Shifter (Longtooth)", source: "ERftLW", ab: { str: 2, dex: 1 }, spd: 30, sz: "Medium", traits: ["Darkvision", "Shifting: Bite 1d6+STR, bonus attack"] },
    { name: "Dragonborn (Chromatic)", source: "FToD", ab: { str: 2, cha: 1 }, spd: 30, sz: "Medium", traits: ["Chromatic Ancestry", "Breath Weapon", "Damage Resistance", "Darkvision"] },
    { name: "Dragonborn (Metallic)", source: "FToD", ab: { str: 2, cha: 1 }, spd: 30, sz: "Medium", traits: ["Metallic Ancestry", "Breath Weapon", "Protective Wings", "Darkvision"] },
    { name: "Dragonborn (Gem)", source: "FToD", ab: { str: 2, cha: 1 }, spd: 30, sz: "Medium", traits: ["Gem Ancestry", "Breath Weapon", "Psionic Mind", "Darkvision", "Gem Flight"] },
  ],

  classes: [
    { name: "Barbarian", hd: 12, pri: "STR", saves: ["str", "con"], skills: ["Animal Handling", "Athletics", "Intimidation", "Nature", "Perception", "Survival"], nSkills: 2, armor: ["Light", "Medium", "Shields"], weapons: ["Simple", "Martial"], subclasses: ["Path of the Berserker", "Path of the Totem Warrior", "Path of the Ancestral Guardian", "Path of the Storm Herald", "Path of the Zealot", "Path of the Beast", "Path of Wild Magic", "Path of the Giant", "Path of the World Tree"], resources: [{ name: "Rage", max: l => l < 3 ? 2 : l < 6 ? 3 : l < 12 ? 4 : l < 17 ? 5 : 6, rest: "long" }, { name: "Unarmored Defense", desc: "AC = 10+DEX+CON" }] },
    { name: "Bard", hd: 8, pri: "CHA", saves: ["dex", "cha"], skills: ["Any"], nSkills: 3, armor: ["Light"], weapons: ["Simple", "Hand Crossbow", "Longsword", "Rapier", "Shortsword"], subclasses: ["College of Lore", "College of Valor", "College of Glamour", "College of Swords", "College of Whispers", "College of Creation", "College of Eloquence", "College of Spirits"], resources: [{ name: "Bardic Inspiration", max: l => l < 5 ? 1 : l < 10 ? 2 : l < 15 ? 3 : 4, rest: "short", die: l => l < 5 ? "d6" : l < 10 ? "d8" : l < 15 ? "d10" : "d12" }] },
    { name: "Cleric", hd: 8, pri: "WIS", saves: ["wis", "cha"], skills: ["History", "Insight", "Medicine", "Persuasion", "Religion"], nSkills: 2, armor: ["Light", "Medium", "Shields"], weapons: ["Simple"], subclasses: ["Life Domain", "Light Domain", "Trickery Domain", "Knowledge Domain", "Nature Domain", "Tempest Domain", "War Domain", "Death Domain", "Arcana Domain", "Forge Domain", "Grave Domain", "Order Domain", "Twilight Domain", "Peace Domain"], resources: [{ name: "Channel Divinity", max: l => l < 6 ? 1 : l < 18 ? 2 : 3, rest: "short" }] },
    { name: "Druid", hd: 8, pri: "WIS", saves: ["int", "wis"], skills: ["Arcana", "Animal Handling", "Insight", "Medicine", "Nature", "Perception", "Religion", "Survival"], nSkills: 2, armor: ["Light", "Medium (non-metal)", "Shields"], weapons: ["Club", "Dagger", "Dart", "Javelin", "Mace", "Quarterstaff", "Scimitar", "Sickle", "Sling", "Spear"], subclasses: ["Circle of the Land", "Circle of the Moon", "Circle of Dreams", "Circle of the Shepherd", "Circle of Spores", "Circle of Stars", "Circle of Wildfire", "Circle of the Sea"], resources: [{ name: "Wild Shape", max: l => l < 20 ? 2 : 99, rest: "short" }] },
    { name: "Fighter", hd: 10, pri: "STR or DEX", saves: ["str", "con"], skills: ["Acrobatics", "Animal Handling", "Athletics", "History", "Insight", "Intimidation", "Perception", "Survival"], nSkills: 2, armor: ["All", "Shields"], weapons: ["Simple", "Martial"], subclasses: ["Champion", "Battle Master", "Eldritch Knight", "Arcane Archer", "Cavalier", "Samurai", "Echo Knight", "Psi Warrior", "Rune Knight", "Purple Dragon Knight", "Gunslinger"], resources: [{ name: "Action Surge", max: l => l < 17 ? 1 : 2, rest: "short" }, { name: "Second Wind", max: () => 1, rest: "short" }] },
    { name: "Monk", hd: 8, pri: "DEX & WIS", saves: ["str", "dex"], skills: ["Acrobatics", "Athletics", "History", "Insight", "Religion", "Stealth"], nSkills: 2, armor: [], weapons: ["Simple", "Shortsword"], subclasses: ["Way of the Open Hand", "Way of Shadow", "Way of the Four Elements", "Way of the Long Death", "Way of the Sun Soul", "Way of the Drunken Master", "Way of the Kensei", "Way of the Mercy", "Way of the Astral Self"], resources: [{ name: "Ki Points", max: l => l, rest: "short" }] },
    { name: "Paladin", hd: 10, pri: "STR & CHA", saves: ["wis", "cha"], skills: ["Athletics", "Insight", "Intimidation", "Medicine", "Persuasion", "Religion"], nSkills: 2, armor: ["All", "Shields"], weapons: ["Simple", "Martial"], subclasses: ["Oath of Devotion", "Oath of the Ancients", "Oath of Vengeance", "Oath of the Crown", "Oath of Conquest", "Oath of Redemption", "Oath of Glory", "Oath of the Watchers", "Oathbreaker"], resources: [{ name: "Lay on Hands", max: l => l * 5, rest: "long", unit: "HP" }, { name: "Divine Smite", desc: "Expend spell slot on hit" }] },
    { name: "Ranger", hd: 10, pri: "DEX & WIS", saves: ["str", "dex"], skills: ["Animal Handling", "Athletics", "Insight", "Investigation", "Nature", "Perception", "Stealth", "Survival"], nSkills: 3, armor: ["Light", "Medium", "Shields"], weapons: ["Simple", "Martial"], subclasses: ["Hunter", "Beast Master", "Gloom Stalker", "Horizon Walker", "Monster Slayer", "Fey Wanderer", "Swarmkeeper", "Drakewarden"], resources: [] },
    { name: "Rogue", hd: 8, pri: "DEX", saves: ["dex", "int"], skills: ["Acrobatics", "Athletics", "Deception", "Insight", "Intimidation", "Investigation", "Perception", "Performance", "Persuasion", "Sleight of Hand", "Stealth"], nSkills: 4, armor: ["Light"], weapons: ["Simple", "Hand Crossbow", "Longsword", "Rapier", "Shortsword"], subclasses: ["Thief", "Assassin", "Arcane Trickster", "Inquisitive", "Mastermind", "Scout", "Swashbuckler", "Phantom", "Soulknife"], resources: [{ name: "Cunning Action", desc: "Dash/Disengage/Hide as bonus action" }] },
    { name: "Sorcerer", hd: 6, pri: "CHA", saves: ["con", "cha"], skills: ["Arcana", "Deception", "Insight", "Intimidation", "Persuasion", "Religion"], nSkills: 2, armor: [], weapons: ["Dagger", "Dart", "Sling", "Quarterstaff", "Light Crossbow"], subclasses: ["Draconic Bloodline", "Wild Magic", "Divine Soul", "Shadow Magic", "Storm Sorcery", "Aberrant Mind", "Clockwork Soul", "Lunar Sorcery"], resources: [{ name: "Sorcery Points", max: l => l, rest: "long" }] },
    { name: "Warlock", hd: 8, pri: "CHA", saves: ["wis", "cha"], skills: ["Arcana", "Deception", "History", "Intimidation", "Investigation", "Nature", "Religion"], nSkills: 2, armor: ["Light"], weapons: ["Simple"], subclasses: ["The Archfey", "The Fiend", "The Great Old One", "The Hexblade", "The Celestial", "The Fathomless", "The Genie", "The Undead", "The Undying"], resources: [{ name: "Pact Magic Slots", max: l => l < 2 ? 1 : l < 11 ? 2 : l < 17 ? 3 : 4, rest: "short" }] },
    { name: "Wizard", hd: 6, pri: "INT", saves: ["int", "wis"], skills: ["Arcana", "History", "Insight", "Investigation", "Medicine", "Religion"], nSkills: 2, armor: [], weapons: ["Dagger", "Dart", "Sling", "Quarterstaff", "Light Crossbow"], subclasses: ["School of Abjuration", "School of Conjuration", "School of Divination", "School of Enchantment", "School of Evocation", "School of Illusion", "School of Necromancy", "School of Transmutation", "Bladesinging", "Order of Scribes", "Chronurgy Magic", "Graviturgy Magic", "War Magic"], resources: [{ name: "Arcane Recovery", max: () => 1, rest: "long" }] },
    { name: "Artificer", hd: 8, pri: "INT", saves: ["con", "int"], skills: ["Arcana", "History", "Investigation", "Medicine", "Nature", "Perception", "Sleight of Hand"], nSkills: 2, armor: ["Light", "Medium", "Shields"], weapons: ["Simple"], subclasses: ["Alchemist", "Armorer", "Artillerist", "Battle Smith"], resources: [{ name: "Infusions", max: l => l < 6 ? 4 : l < 10 ? 6 : l < 14 ? 8 : l < 18 ? 10 : 12, rest: "long" }] },
  ],

  backgrounds: [
    { name: "Acolyte", src: "PHB", skills: ["Insight", "Religion"], feature: "Shelter of the Faithful" },
    { name: "Charlatan", src: "PHB", skills: ["Deception", "Sleight of Hand"], feature: "False Identity" },
    { name: "Criminal", src: "PHB", skills: ["Deception", "Stealth"], feature: "Criminal Contact" },
    { name: "Entertainer", src: "PHB", skills: ["Acrobatics", "Performance"], feature: "By Popular Demand" },
    { name: "Folk Hero", src: "PHB", skills: ["Animal Handling", "Survival"], feature: "Rustic Hospitality" },
    { name: "Guild Artisan", src: "PHB", skills: ["Insight", "Persuasion"], feature: "Guild Membership" },
    { name: "Hermit", src: "PHB", skills: ["Medicine", "Religion"], feature: "Discovery" },
    { name: "Noble", src: "PHB", skills: ["History", "Persuasion"], feature: "Position of Privilege" },
    { name: "Outlander", src: "PHB", skills: ["Athletics", "Survival"], feature: "Wanderer" },
    { name: "Sage", src: "PHB", skills: ["Arcana", "History"], feature: "Researcher" },
    { name: "Sailor", src: "PHB", skills: ["Athletics", "Perception"], feature: "Ship's Passage" },
    { name: "Soldier", src: "PHB", skills: ["Athletics", "Intimidation"], feature: "Military Rank" },
    { name: "Urchin", src: "PHB", skills: ["Sleight of Hand", "Stealth"], feature: "City Secrets" },
    { name: "Far Traveler", src: "SCAG", skills: ["Insight", "Perception"], feature: "All Eyes on You" },
    { name: "Haunted One", src: "CoS", skills: ["Arcana", "Investigation"], feature: "Heart of Darkness" },
    { name: "City Watch", src: "SCAG", skills: ["Athletics", "Insight"], feature: "Watcher's Eye" },
    { name: "Clan Crafter", src: "SCAG", skills: ["History", "Insight"], feature: "Respect of the Stout Folk" },
    { name: "Courtier", src: "SCAG", skills: ["Insight", "Persuasion"], feature: "Court Functionary" },
    { name: "Faction Agent", src: "SCAG", skills: ["Insight", "Religion"], feature: "Safe Haven" },
    { name: "Feylost", src: "WBtW", skills: ["Deception", "Survival"], feature: "Feywild Connection" },
    { name: "Gladiator", src: "PHB", skills: ["Acrobatics", "Performance"], feature: "By Popular Demand" },
    { name: "Pirate", src: "PHB", skills: ["Athletics", "Perception"], feature: "Bad Reputation" },
    { name: "Spy", src: "PHB", skills: ["Deception", "Stealth"], feature: "Criminal Contact" },
    { name: "Knight", src: "PHB", skills: ["History", "Persuasion"], feature: "Retainers" },
    { name: "Investigator", src: "VRGtR", skills: ["Insight", "Investigation"], feature: "Official Inquiry" },
    { name: "Marine", src: "GoS", skills: ["Athletics", "Survival"], feature: "Steady" },
    { name: "Anthropologist", src: "ToA", skills: ["Insight", "Religion"], feature: "Adept Linguist" },
    { name: "Archaeologist", src: "ToA", skills: ["History", "Survival"], feature: "Historical Knowledge" },
    { name: "Athlete", src: "MOT", skills: ["Acrobatics", "Athletics"], feature: "Echoes of Victory" },
  ],

  feats: [
    { name: "Alert", src: "PHB", desc: "+5 initiative, can't be surprised, no advantage from hidden attackers" },
    { name: "Actor", src: "PHB", desc: "+1 CHA, advantage on Deception/Performance while disguised, mimic voices" },
    { name: "Charger", src: "PHB", desc: "After Dash, bonus action attack (+5 dmg) or shove" },
    { name: "Crossbow Expert", src: "PHB", desc: "Ignore loading, no melee disadvantage, bonus action attack" },
    { name: "Defensive Duelist", src: "PHB", pre: "DEX 13", desc: "Reaction: add proficiency to AC vs one attack with finesse weapon" },
    { name: "Dual Wielder", src: "PHB", desc: "+1 AC with two weapons, use non-light weapons for TWF" },
    { name: "Dungeon Delver", src: "PHB", desc: "Advantage vs traps & secret doors, resistance to trap damage" },
    { name: "Durable", src: "PHB", desc: "+1 CON, minimum Hit Die roll = CON score" },
    { name: "Elemental Adept", src: "PHB", pre: "Spellcaster", desc: "Ignore resistance to chosen element, treat 1s as 2s on damage" },
    { name: "Great Weapon Master", src: "PHB", desc: "Crit/kill = bonus attack; or -5 hit for +10 damage" },
    { name: "Healer", src: "PHB", desc: "Stabilize as action, heal 1+1d6+max_hit_die with healer's kit" },
    { name: "Inspiring Leader", src: "PHB", pre: "CHA 13", desc: "10-min speech: up to 6 allies gain level+CHA temp HP" },
    { name: "Lucky", src: "PHB", desc: "3 luck points/long rest — add extra d20 to attack/ability/save rolls" },
    { name: "Mage Slayer", src: "PHB", desc: "Reaction attack on spell cast; advantage vs concentration; disadvtg on their concentration saves" },
    { name: "Magic Initiate", src: "PHB", desc: "Learn 2 cantrips + 1st-level spell from any class" },
    { name: "Martial Adept", src: "PHB", desc: "2 Battle Master maneuvers, 1 superiority die (d6)" },
    { name: "Mobile", src: "PHB", desc: "+10 speed, Dash ignores difficult terrain, no opp. attacks from attacked creatures" },
    { name: "Mounted Combatant", src: "PHB", desc: "Advantage vs unmounted smaller creatures, redirect attacks to mount" },
    { name: "Observant", src: "PHB", desc: "+1 INT or WIS, +5 passive Perception & Investigation, lip reading" },
    { name: "Polearm Master", src: "PHB", desc: "Bonus butt-end attack (d4), opportunity attacks on entry" },
    { name: "Resilient", src: "PHB", desc: "+1 to ability score, proficiency in its saving throw" },
    { name: "Savage Attacker", src: "PHB", desc: "Once per turn, reroll melee weapon damage, use either result" },
    { name: "Sentinel", src: "PHB", desc: "Opp. attack stops movement; attack reaction when ally attacked; attack Disengagers" },
    { name: "Sharpshooter", src: "PHB", desc: "No long-range disadvantage, ignore 1/2 & 3/4 cover, -5 hit for +10 damage" },
    { name: "Shield Master", src: "PHB", desc: "Bonus shove; add shield to DEX saves; no damage on successful DEX save" },
    { name: "Skilled", src: "PHB", desc: "Gain proficiency in any 3 skills or tools" },
    { name: "Spell Sniper", src: "PHB", pre: "Spellcaster", desc: "Double range of attack roll spells, ignore half/3/4 cover, 1 attack cantrip" },
    { name: "Tavern Brawler", src: "PHB", desc: "+1 STR/CON, d4 unarmed, proficiency with improvised weapons, grapple as bonus action" },
    { name: "Tough", src: "PHB", desc: "+2 HP per level (retroactive)" },
    { name: "War Caster", src: "PHB", pre: "Spellcaster", desc: "Advantage on concentration saves, cast with occupied hands, opp-attack spells" },
    { name: "Weapon Master", src: "PHB", desc: "+1 STR/DEX, proficiency in 4 weapons" },
    { name: "Fey Touched", src: "TCoE", desc: "+1 INT/WIS/CHA, Misty Step + one 1st-level divination/enchantment spell 1/day each" },
    { name: "Shadow Touched", src: "TCoE", desc: "+1 INT/WIS/CHA, Invisibility + one 1st-level illusion/necromancy spell 1/day" },
    { name: "Skill Expert", src: "TCoE", desc: "+1 any ability, proficiency in one skill, expertise in one skill" },
    { name: "Telekinetic", src: "TCoE", desc: "+1 INT/WIS/CHA, Mage Hand cantrip enhanced, bonus action shove 5ft" },
    { name: "Telepathic", src: "TCoE", desc: "+1 INT/WIS/CHA, Detect Thoughts 1/day, telepathic speech 60ft" },
    { name: "Chef", src: "TCoE", desc: "+1 CON/WIS, cook meals for short rest healing or temp HP" },
    { name: "Crusher", src: "TCoE", desc: "+1 STR/CON, push creature on bludgeoning hit, advantage trigger on critical" },
    { name: "Piercer", src: "TCoE", desc: "+1 STR/DEX, reroll one piercing damage die, extra die on critical" },
    { name: "Slasher", src: "TCoE", desc: "+1 STR/DEX, reduce speed 10ft on slashing hit, disadvantage on attacks on critical" },
    { name: "Eldritch Adept", src: "TCoE", pre: "Pact Magic or Spellcasting", desc: "Learn one Eldritch Invocation" },
    { name: "Fighting Initiate", src: "TCoE", pre: "Martial weapon proficiency", desc: "Learn one Fighting Style from Fighter list" },
    { name: "Metamagic Adept", src: "TCoE", pre: "Spellcaster", desc: "Two Metamagic options, 2 sorcery points" },
    { name: "Gunner", src: "TCoE", desc: "+1 DEX, firearm proficiency, ignore loading, no melee disadvantage" },
    { name: "Poisoner", src: "TCoE", desc: "Proficiency with Poisoner's Kit, ignore resistance, apply as bonus action" },
    { name: "Artificer Initiate", src: "TCoE", desc: "One artificer cantrip + 1st-level spell, one tool proficiency" },
    { name: "Ability Score Improvement", src: "PHB", desc: "+2 to one ability score or +1 to two different ability scores" },
  ],

  skills: [
    { name: "Acrobatics", ab: "dex" }, { name: "Animal Handling", ab: "wis" }, { name: "Arcana", ab: "int" },
    { name: "Athletics", ab: "str" }, { name: "Deception", ab: "cha" }, { name: "History", ab: "int" },
    { name: "Insight", ab: "wis" }, { name: "Intimidation", ab: "cha" }, { name: "Investigation", ab: "int" },
    { name: "Medicine", ab: "wis" }, { name: "Nature", ab: "int" }, { name: "Perception", ab: "wis" },
    { name: "Performance", ab: "cha" }, { name: "Persuasion", ab: "cha" }, { name: "Religion", ab: "int" },
    { name: "Sleight of Hand", ab: "dex" }, { name: "Stealth", ab: "dex" }, { name: "Survival", ab: "wis" },
  ],

  spells: {
    C: ["Acid Splash", "Blade Ward", "Booming Blade", "Chill Touch", "Control Flames", "Create Bonfire", "Dancing Lights", "Druidcraft", "Eldritch Blast", "Fire Bolt", "Frostbite", "Green-Flame Blade", "Guidance", "Gust", "Infestation", "Light", "Mage Hand", "Magic Stone", "Mending", "Message", "Mind Sliver", "Minor Illusion", "Mold Earth", "Poison Spray", "Prestidigitation", "Produce Flame", "Ray of Frost", "Resistance", "Sacred Flame", "Shape Water", "Shillelagh", "Shocking Grasp", "Spare the Dying", "Sword Burst", "Thaumaturgy", "Thunderclap", "Toll the Dead", "True Strike", "Vicious Mockery", "Word of Radiance"],
    1: ["Absorb Elements", "Alarm", "Animal Friendship", "Armor of Agathys", "Arms of Hadar", "Bane", "Bless", "Burning Hands", "Catapult", "Cause Fear", "Charm Person", "Chromatic Orb", "Color Spray", "Command", "Comprehend Languages", "Cure Wounds", "Detect Evil and Good", "Detect Magic", "Disguise Self", "Divine Favor", "Entangle", "Expeditious Retreat", "Faerie Fire", "False Life", "Feather Fall", "Find Familiar", "Fog Cloud", "Goodberry", "Grease", "Guiding Bolt", "Healing Word", "Hellish Rebuke", "Heroism", "Hex", "Hunter's Mark", "Ice Knife", "Identify", "Inflict Wounds", "Jump", "Longstrider", "Mage Armor", "Magic Missile", "Protection from Evil and Good", "Ray of Sickness", "Sanctuary", "Shield", "Shield of Faith", "Silent Image", "Sleep", "Speak with Animals", "Tasha's Hideous Laughter", "Thunderwave", "Unseen Servant", "Witch Bolt", "Wrathful Smite", "Zephyr Strike"],
    2: ["Aid", "Alter Self", "Animal Messenger", "Arcane Lock", "Augury", "Barkskin", "Blindness/Deafness", "Blur", "Branding Smite", "Calm Emotions", "Cloud of Daggers", "Continual Flame", "Crown of Madness", "Darkness", "Darkvision", "Detect Thoughts", "Dragon's Breath", "Earthbind", "Enhance Ability", "Enlarge/Reduce", "Enthrall", "Find Steed", "Find Traps", "Flaming Sphere", "Gentle Repose", "Gust of Wind", "Heat Metal", "Hold Person", "Invisibility", "Knock", "Lesser Restoration", "Levitate", "Locate Object", "Magic Weapon", "Maximilian's Earthen Grasp", "Mirror Image", "Misty Step", "Moonbeam", "Pass Without Trace", "Prayer of Healing", "Protection from Poison", "Ray of Enfeeblement", "Rope Trick", "Scorching Ray", "See Invisibility", "Shadow Blade", "Shatter", "Silence", "Spider Climb", "Spike Growth", "Spiritual Weapon", "Suggestion", "Web", "Zone of Truth"],
    3: ["Animate Dead", "Aura of Vitality", "Beacon of Hope", "Bestow Curse", "Blink", "Call Lightning", "Catnap", "Clairvoyance", "Conjure Animals", "Counterspell", "Create Food and Water", "Crusader's Mantle", "Daylight", "Dispel Magic", "Elemental Weapon", "Enemies Abound", "Erupting Earth", "Fear", "Fireball", "Fly", "Gaseous Form", "Haste", "Hunger of Hadar", "Hypnotic Pattern", "Lightning Bolt", "Magic Circle", "Major Image", "Mass Healing Word", "Meld into Stone", "Nondetection", "Plant Growth", "Protection from Energy", "Remove Curse", "Revivify", "Sending", "Sleet Storm", "Slow", "Speak with Dead", "Spirit Guardians", "Stinking Cloud", "Thunder Step", "Tidal Wave", "Tongues", "Vampiric Touch", "Wall of Sand", "Wall of Water", "Water Breathing", "Wind Wall"],
    4: ["Arcane Eye", "Aura of Life", "Banishment", "Blight", "Charm Monster", "Compulsion", "Confusion", "Control Water", "Death Ward", "Dimension Door", "Divination", "Dominate Beast", "Evard's Black Tentacles", "Fabricate", "Fire Shield", "Freedom of Movement", "Giant Insect", "Grasping Vine", "Greater Invisibility", "Guardian of Faith", "Hallucinatory Terrain", "Ice Storm", "Polymorph", "Stone Shape", "Stoneskin", "Wall of Fire"],
    5: ["Animate Objects", "Antilife Shell", "Awaken", "Bigby's Hand", "Circle of Power", "Cloudkill", "Commune", "Cone of Cold", "Conjure Elemental", "Contact Other Plane", "Contagion", "Control Winds", "Creation", "Dominate Person", "Dream", "Flame Strike", "Geas", "Greater Restoration", "Hold Monster", "Insect Plague", "Legend Lore", "Mass Cure Wounds", "Mislead", "Modify Memory", "Passwall", "Raise Dead", "Scrying", "Seeming", "Steel Wind Strike", "Swift Quiver", "Synaptic Static", "Telekinesis", "Teleportation Circle", "Wall of Force", "Wall of Stone"],
  },

  weapons: {
    "Simple Melee": ["Club (1d4 B)", "Dagger (1d4 P, finesse, light, thrown 20/60)", "Greatclub (1d8 B, two-handed)", "Handaxe (1d6 S, light, thrown 20/60)", "Javelin (1d6 P, thrown 30/120)", "Light Hammer (1d4 B, light, thrown 20/60)", "Mace (1d6 B)", "Quarterstaff (1d6 B, versatile 1d8)", "Sickle (1d4 S, light)", "Spear (1d6 P, thrown 20/60, versatile 1d8)"],
    "Simple Ranged": ["Crossbow Light (1d8 P, 80/320ft, loading)", "Dart (1d4 P, finesse, thrown 20/60)", "Shortbow (1d6 P, 80/320ft)", "Sling (1d4 B, 30/120ft)"],
    "Martial Melee": ["Battleaxe (1d8 S, versatile 1d10)", "Flail (1d8 B)", "Glaive (1d10 S, heavy, reach, two-handed)", "Greataxe (1d12 S, heavy, two-handed)", "Greatsword (2d6 S, heavy, two-handed)", "Halberd (1d10 S, heavy, reach, two-handed)", "Lance (1d12 P, reach)", "Longsword (1d8 S, versatile 1d10)", "Maul (2d6 B, heavy, two-handed)", "Morningstar (1d8 P)", "Pike (1d10 P, heavy, reach, two-handed)", "Rapier (1d8 P, finesse)", "Scimitar (1d6 S, finesse, light)", "Shortsword (1d6 P, finesse, light)", "Trident (1d6 P, thrown, versatile 1d8)", "War Pick (1d8 P)", "Warhammer (1d8 B, versatile 1d10)", "Whip (1d4 S, finesse, reach)"],
    "Martial Ranged": ["Crossbow Hand (1d6 P, 30/120ft, light)", "Crossbow Heavy (1d10 P, 100/400ft, heavy, loading)", "Longbow (1d8 P, 150/600ft, heavy)", "Net (—, thrown 5/15ft)"],
    "Firearms (Optional)": ["Pistol (1d10 P, 30/90ft, loading)", "Musket (1d12 P, 40/120ft, loading, two-handed)", "Blunderbuss (2d8 P, 15/30ft, loading)"],
  },

  armor: {
    "Light Armor": ["Padded (AC 11+DEX, Stealth disadv)", "Leather (AC 11+DEX)", "Studded Leather (AC 12+DEX)"],
    "Medium Armor": ["Hide (AC 12+DEX≤2)", "Chain Shirt (AC 13+DEX≤2)", "Scale Mail (AC 14+DEX≤2, Stealth disadv)", "Breastplate (AC 14+DEX≤2)", "Half Plate (AC 15+DEX≤2, Stealth disadv)"],
    "Heavy Armor": ["Ring Mail (AC 14)", "Chain Mail (AC 16, STR 13)", "Splint (AC 17, STR 15)", "Plate (AC 18, STR 15)"],
    "Shields": ["Shield (+2 AC)"],
  },

  settings: [
    { name: "The Forgotten Realms", desc: "Faerun — Baldur's Gate to Waterdeep. Ancient ruins, dragon empires, the Underdark beneath. Politics, magic, and myth.", icon: "🏰", tone: "High Fantasy Epic" },
    { name: "Eberron", desc: "Magitech noir. Artificer guilds, elemental airships, the Last War's scars. Intrigue and invention.", icon: "⚙️", tone: "Noir Mystery" },
    { name: "Ravenloft (Domains of Dread)", desc: "Gothic horror. Misty demi-planes ruled by Darklords. Every domain is a pocket nightmare.", icon: "🩸", tone: "Gothic Horror" },
    { name: "Planescape", desc: "The infinite multiverse. Sigil — the City of Doors. Factions, philosophy, and realms beyond mortal comprehension.", icon: "🌀", tone: "Cosmic Weird" },
    { name: "Greyhawk", desc: "Classic high fantasy. Oerth's legendary dungeons, Castle Greyhawk, and the origins of the game itself.", icon: "⚔️", tone: "Classic D&D" },
    { name: "Dark Sun (Athas)", desc: "A dying desert world. No gods answer prayers. Survival, psionics, and brutal city-states.", icon: "☀️", tone: "Post-Apocalyptic" },
    { name: "Spelljammer", desc: "Fantasy space opera. Sail crystal spheres on spelljamming ships between alien worlds.", icon: "🚀", tone: "Space Fantasy" },
    { name: "Dragonlance (Krynn)", desc: "The War of the Lance. Dragonarmies, Raistlin Majere, and the gods of Krynn at war.", icon: "🐉", tone: "War Epic" },
    { name: "Custom World", desc: "Your world. Describe it in play and the AI will weave it into canonical existence.", icon: "✨", tone: "Anything You Imagine" },
  ],
};

// ─── SPELL SLOTS TABLE ─────────────────────────────────────
const SPELL_SLOTS = { 1: [2, 0, 0, 0, 0, 0, 0, 0, 0], 2: [3, 0, 0, 0, 0, 0, 0, 0, 0], 3: [4, 2, 0, 0, 0, 0, 0, 0, 0], 4: [4, 3, 0, 0, 0, 0, 0, 0, 0], 5: [4, 3, 2, 0, 0, 0, 0, 0, 0], 6: [4, 3, 3, 0, 0, 0, 0, 0, 0], 7: [4, 3, 3, 1, 0, 0, 0, 0, 0], 8: [4, 3, 3, 2, 0, 0, 0, 0, 0], 9: [4, 3, 3, 3, 1, 0, 0, 0, 0], 10: [4, 3, 3, 3, 2, 0, 0, 0, 0], 11: [4, 3, 3, 3, 2, 1, 0, 0, 0], 12: [4, 3, 3, 3, 2, 1, 0, 0, 0], 13: [4, 3, 3, 3, 2, 1, 1, 0, 0], 14: [4, 3, 3, 3, 2, 1, 1, 0, 0], 15: [4, 3, 3, 3, 2, 1, 1, 1, 0], 16: [4, 3, 3, 3, 2, 1, 1, 1, 0], 17: [4, 3, 3, 3, 2, 1, 1, 1, 1], 18: [4, 3, 3, 3, 3, 1, 1, 1, 1], 19: [4, 3, 3, 3, 3, 2, 1, 1, 1], 20: [4, 3, 3, 3, 3, 2, 2, 1, 1] };

// ─── AI SYSTEM PROMPT BUILDER ──────────────────────────────
const buildSystemPrompt = (worldState, setting) => {
  const ws = worldState;
  const char = ws.character;
  const combat = ws.combat;

  return `You are a collaborative AI Dungeon Master — a master storyteller and rules arbiter for a D&D 5e tabletop RPG adventure set in ${setting?.name || "a rich fantasy world"} (tone: ${setting?.tone || "Epic Fantasy"}).

═══════════════════════════════════════
CORE PHILOSOPHY — READ CAREFULLY
═══════════════════════════════════════
• You are a COLLABORATOR, never an adversary to the player.
• NEVER take away player agency. If a player declares an action, honor it. Add consequences, drama, complications — never refusal.
• You handle ALL math automatically. Resolve rolls, calculate outcomes, track HP changes — always show your work clearly.
• NPCs are fully realized people with motives, secrets, speech patterns, and their own goals.
• The world breathes and changes. Time passes. Weather shifts. NPCs remember what happened.
• Balance tension and reward. Every obstacle has a solution. Every choice matters.
• Respond to the player's INTENT as much as their words.
• NEVER railroad. NEVER say "you can't." Instead: "You try, but..." or "The attempt reveals..."
• Maintain consistent internal logic. What you establish is canon.

═══════════════════════════════════════
CURRENT WORLD STATE
═══════════════════════════════════════
📍 Location: ${ws.session.location}
🕐 Time: ${ws.session.time} | Weather: ${ws.session.weather}
⚔️ Phase: ${ws.session.phase.toUpperCase()}
📜 Round: ${ws.session.round} (Turn ${ws.session.turn})

${char ? `═══ PLAYER CHARACTER ═══
Name: ${char.name}
Race & Class: ${char.race} ${char.class} (Level ${char.level})
HP: ${char.hp.current}/${char.hp.max}${char.hp.current < char.hp.max * 0.3 ? " ⚠️ CRITICALLY WOUNDED" : char.hp.current < char.hp.max * 0.6 ? " — Wounded" : " — Healthy"}
Proficiency Bonus: +${char.proficiencyBonus}
Conditions: ${char.conditions?.length ? char.conditions.join(", ") : "None"}
Backstory: ${char.backstory || "Unknown"}
Equipment: ${char.equipment?.slice(0, 5).join(", ") || "Basic gear"}
Spells Known: ${char.spells?.slice(0, 5).join(", ") || "None"}` : "No character created yet — treat player as a wandering adventurer."}

${ws.npcs.length ? `═══ KNOWN NPCs ═══\n${ws.npcs.map(n => `${n.name} (${n.relation}) — ${n.notes}`).join("\n")}` : ""}

${ws.quests.length ? `═══ ACTIVE QUESTS ═══\n${ws.quests.filter(q => q.status === "active").map(q => `[${q.title}]: ${q.desc}`).join("\n")}` : ""}

${ws.worldFacts.length ? `═══ ESTABLISHED WORLD FACTS ═══\n${ws.worldFacts.slice(-10).join("\n")}` : ""}

${ws.storyContext ? `═══ RECENT EVENTS ═══\n${ws.storyContext}` : ""}

${combat.active ? `═══ COMBAT STATE ═══
Round: ${combat.round}
Initiative Order: ${combat.initiative.map((e, i) => `${i === combat.currentTurnIndex ? "► " : "  "}${e.name} (HP: ${e.hp}/${e.maxHp}${e.conditions?.length ? " [" + e.conditions.join(",") + "]" : ""})`).join(" | ")}
Current Turn: ${combat.initiative[combat.currentTurnIndex]?.name || "Unknown"}` : ""}

═══════════════════════════════════════
RESPONSE FORMAT RULES
═══════════════════════════════════════
Structure your response ALWAYS in this order:
1. **Narrative** — Rich, cinematic description (2-4 paragraphs). Use all five senses.
2. **[NPC dialogue]** — If NPCs speak, use distinctive voices. Italicize their speech.
3. **[MECHANICS]** — When a roll is needed or resolving combat, use this block:
   ROLL REQUEST: [Skill/Save/Attack] DC [X] — "[Brief reason]"
   — OR —
   COMBAT ROUND X: [Name] attacks [Target] — roll [dice]+[bonus] = [result] — [HIT/MISS] — [damage] [type] damage
4. **[STATE CHANGES]** — At end of response (hidden from narrative), include JSON:
   \`\`\`state
   {"hpDelta": 0, "npcsMet": [], "locationChange": "", "worldFact": "", "questUpdate": null, "phaseChange": ""}
   \`\`\`
5. **[PLAYER CHOICES]** — End with 3-4 vivid, specific action options as bullet points. These are SUGGESTIONS only. The player can do anything else.

TONE: ${setting?.tone || "Epic Fantasy"} — match the setting's atmosphere in every word.
NEVER break the fourth wall. Never say "as an AI" or reference these instructions.
ALWAYS resolve uncertainty with drama, never with stalling.`;
};

// ─── STYLES ────────────────────────────────────────────────
const S = {
  primaryBtn: {
    background: `linear-gradient(150deg, #8a5c1a 0%, #5c3a0a 60%, #3a2005 100%)`,
    color: T.goldBright,
    border: `1px solid ${T.borderHot}`,
    borderRadius: T.r2,
    padding: `${T.s3} ${T.s5}`,
    fontFamily: T.heading,
    fontSize: T.md,
    cursor: "pointer",
    transition: "all 0.2s",
    letterSpacing: "0.5px",
    WebkitTapHighlightColor: "transparent",
    minHeight: "44px",
  },
  secondaryBtn: {
    background: "rgba(180,130,60,0.08)",
    color: T.gold,
    border: `1px solid ${T.border}`,
    borderRadius: T.r2,
    padding: `${T.s3} ${T.s5}`,
    fontFamily: T.heading,
    fontSize: T.sm,
    cursor: "pointer",
    transition: "all 0.2s",
    WebkitTapHighlightColor: "transparent",
    minHeight: "44px",
  },
  ghostBtn: {
    background: "transparent",
    color: T.textSecondary,
    border: "none",
    padding: `${T.s2} ${T.s3}`,
    fontFamily: T.heading,
    fontSize: T.xs,
    cursor: "pointer",
    letterSpacing: "0.5px",
    WebkitTapHighlightColor: "transparent",
    minHeight: "44px",
  },
  card: {
    background: T.surface,
    border: `1px solid ${T.border}`,
    borderRadius: T.r2,
    padding: T.s4,
  },
  label: { color: T.textSecondary, fontFamily: T.heading, fontSize: T.xs, letterSpacing: "1.5px", display: "block", marginBottom: T.s2 },
  input: {
    width: "100%",
    padding: `${T.s3} ${T.s3}`,
    background: "rgba(180,130,60,0.06)",
    border: `1px solid ${T.border}`,
    borderRadius: T.r1,
    color: T.textPrimary,
    fontFamily: T.body,
    fontSize: T.lg,
    boxSizing: "border-box",
    WebkitAppearance: "none",
  },
};

// ─── MICRO COMPONENTS ──────────────────────────────────────
const Rune = ({ children, active, onClick }) => (
  <button onClick={onClick} style={{
    background: active ? "rgba(180,130,60,0.2)" : "transparent",
    color: active ? T.goldBright : T.textSecondary,
    border: `1px solid ${active ? T.borderHot : T.border}`,
    borderRadius: T.r1,
    padding: `3px 10px`,
    fontFamily: T.heading,
    fontSize: T.xs,
    cursor: "pointer",
    letterSpacing: "0.5px",
    minHeight: "32px",
    WebkitTapHighlightColor: "transparent",
    transition: "all 0.15s",
  }}>{children}</button>
);

const Pip = ({ filled, color = "#7060b8", size = 14 }) => (
  <div style={{
    width: size, height: size, borderRadius: "50%",
    background: filled ? color : "transparent",
    border: `2px solid ${filled ? color : "rgba(180,130,60,0.3)"}`,
    transition: "all 0.2s",
  }} />
);

const StatBox = ({ label, value, color }) => (
  <div style={{ textAlign: "center", background: T.surface, border: `1px solid ${T.border}`, borderRadius: T.r2, padding: `${T.s3} ${T.s4}`, minWidth: 58 }}>
    <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textSecondary, letterSpacing: "1px", marginBottom: 2 }}>{label}</div>
    <div style={{ fontFamily: T.heading, fontSize: T.xxl, fontWeight: 700, color: color || T.goldBright, lineHeight: 1 }}>{value}</div>
  </div>
);

const HPBar = ({ current, max, temp = 0, height = 10 }) => {
  const pct = Math.max(0, Math.min(100, (current / max) * 100));
  const color = pct > 60 ? T.hp : pct > 25 ? T.hpMid : T.hpLow;
  return (
    <div>
      <div style={{ height, background: "rgba(255,255,255,0.06)", borderRadius: height / 2, overflow: "hidden", position: "relative" }}>
        <div style={{ position: "absolute", inset: 0, background: `linear-gradient(90deg, ${color}88 0%, ${color} 100%)`, width: `${pct}%`, borderRadius: height / 2, transition: "width 0.6s cubic-bezier(0.4,0,0.2,1)" }} />
        {temp > 0 && <div style={{ position: "absolute", right: 0, top: 0, bottom: 0, width: `${Math.min(100, (temp / max) * 100)}%`, background: "rgba(100,140,220,0.5)", borderRadius: height / 2 }} />}
      </div>
      <div style={{ display: "flex", justifyContent: "space-between", marginTop: 3 }}>
        <span style={{ fontFamily: T.heading, fontSize: T.sm, color, fontWeight: 700 }}>{current}/{max} HP</span>
        {temp > 0 && <span style={{ fontFamily: T.heading, fontSize: T.xs, color: "#6090d0" }}>+{temp} temp</span>}
      </div>
    </div>
  );
};

const AbilityBox = ({ label, score, onChange }) => {
  const mod = DnD.mod(score);
  return (
    <div style={{ display: "flex", flexDirection: "column", alignItems: "center", background: T.surface, border: `1px solid ${T.border}`, borderRadius: T.r2, padding: "10px 8px", minWidth: 62 }}>
      <div style={{ fontFamily: T.heading, fontSize: 9, color: T.textSecondary, letterSpacing: "1px", marginBottom: 2 }}>{label}</div>
      <div style={{ fontFamily: T.heading, fontSize: 20, fontWeight: 700, color: T.goldBright, lineHeight: 1 }}>{DnD.modStr(mod)}</div>
      <input type="number" min={1} max={30} value={score}
        onChange={e => onChange && onChange(Number(e.target.value))}
        readOnly={!onChange}
        style={{ width: 38, textAlign: "center", background: "rgba(180,130,60,0.1)", border: `1px solid ${T.border}`, borderRadius: 4, color: T.gold, fontFamily: T.heading, fontSize: T.sm, marginTop: 4, padding: "2px 0" }} />
    </div>
  );
};

// ─── DICE ROLLER ───────────────────────────────────────────
const DiceRoller = ({ onRoll, compact }) => {
  const [die, setDie] = useState(20);
  const [result, setResult] = useState(null);
  const [rolling, setRolling] = useState(false);
  const [modifier, setModifier] = useState(0);

  const roll = (withAdv, withDisadv) => {
    setRolling(true);
    setTimeout(() => {
      let r;
      if (withAdv) r = DnD.rollAdvantage(die);
      else if (withDisadv) r = DnD.rollDisadvantage(die);
      else r = DnD.rollDie(die);
      const total = r + modifier;
      setResult({ raw: r, mod: modifier, total });
      setRolling(false);
      onRoll && onRoll({ die, raw: r, mod: modifier, total, crit: r === die, fail: r === 1 });
    }, 400);
  };

  const isCrit = result?.raw === die;
  const isFail = result?.raw === 1;

  if (compact) return (
    <div style={{ display: "flex", alignItems: "center", gap: T.s2, padding: `4px ${T.s2}`, background: "rgba(180,130,60,0.06)", borderRadius: T.r1, border: `1px solid ${T.border}` }}>
      <select value={die} onChange={e => setDie(Number(e.target.value))} style={{ background: "transparent", color: T.gold, border: "none", fontFamily: T.heading, fontSize: T.sm, cursor: "pointer" }}>
        {[4, 6, 8, 10, 12, 20, 100].map(d => <option key={d} value={d} style={{ background: "#150d04" }}>d{d}</option>)}
      </select>
      <button onClick={() => roll()} style={{ background: "linear-gradient(135deg,#8a5c1a,#5c3a0a)", color: T.goldBright, border: "none", borderRadius: 5, padding: "4px 10px", fontFamily: T.heading, fontSize: T.sm, cursor: "pointer", transform: rolling ? "rotate(180deg)" : "none", transition: "transform 0.4s" }}>🎲</button>
      {result && <span style={{ fontFamily: T.heading, fontWeight: 700, fontSize: 16, color: isCrit ? "#ffd700" : isFail ? T.hpLow : T.goldBright, minWidth: 28 }}>{rolling ? "?" : result.total}</span>}
    </div>
  );

  return (
    <div style={{ ...S.card, marginBottom: T.s4 }}>
      <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textSecondary, letterSpacing: "1px", marginBottom: T.s3 }}>DICE ROLLER</div>
      <div style={{ display: "flex", gap: T.s2, alignItems: "center", flexWrap: "wrap", marginBottom: T.s3 }}>
        <select value={die} onChange={e => setDie(Number(e.target.value))} style={{ padding: `${T.s2} ${T.s3}`, background: "rgba(180,130,60,0.1)", border: `1px solid ${T.border}`, borderRadius: T.r1, color: T.gold, fontFamily: T.heading, fontSize: T.md }}>
          {[4, 6, 8, 10, 12, 20, 100].map(d => <option key={d} value={d} style={{ background: "#150d04" }}>d{d}</option>)}
        </select>
        <span style={{ color: T.textMuted }}>+</span>
        <input type="number" value={modifier} onChange={e => setModifier(Number(e.target.value))} style={{ width: 52, ...S.input, padding: `${T.s2} ${T.s2}` }} />
        {result && !rolling && (
          <div style={{ padding: `${T.s2} ${T.s4}`, background: isCrit ? "rgba(255,215,0,0.1)" : isFail ? "rgba(192,64,64,0.1)" : "rgba(180,130,60,0.1)", borderRadius: T.r1, border: `1px solid ${isCrit ? "#ffd700" : isFail ? T.hpLow : T.border}` }}>
            <span style={{ fontFamily: T.heading, fontWeight: 700, fontSize: T.xl, color: isCrit ? "#ffd700" : isFail ? T.hpLow : T.goldBright }}>{result.total}</span>
            {modifier !== 0 && <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, marginLeft: 4 }}>({result.raw}{DnD.modStr(modifier)})</span>}
            {isCrit && <span style={{ fontFamily: T.heading, fontSize: T.xs, color: "#ffd700", marginLeft: 6 }}>CRITICAL! ★</span>}
            {isFail && <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.hpLow, marginLeft: 6 }}>CRITICAL FAIL ☠</span>}
          </div>
        )}
      </div>
      <div style={{ display: "flex", gap: T.s2 }}>
        <button onClick={() => roll()} style={{ ...S.primaryBtn, flex: 1, padding: `${T.s3} ${T.s2}` }}>Roll</button>
        <button onClick={() => roll(true)} style={{ ...S.secondaryBtn, padding: `${T.s3} ${T.s3}`, fontSize: T.xs }}>Adv</button>
        <button onClick={() => roll(false, true)} style={{ ...S.secondaryBtn, padding: `${T.s3} ${T.s3}`, fontSize: T.xs }}>Dis</button>
      </div>
    </div>
  );
};

// ─── STORY MESSAGE ─────────────────────────────────────────
const StoryMessage = ({ msg, isNew }) => {
  const [displayed, setDisplayed] = useState(isNew ? "" : msg.content);
  const [done, setDone] = useState(!isNew);
  const intervalRef = useRef(null);

  useEffect(() => {
    if (!isNew || done) return;
    let i = 0;
    const text = msg.content;
    intervalRef.current = setInterval(() => {
      i += 3;
      setDisplayed(text.slice(0, i));
      if (i >= text.length) {
        clearInterval(intervalRef.current);
        setDisplayed(text);
        setDone(true);
      }
    }, 18);
    return () => clearInterval(intervalRef.current);
  }, []);

  const onClick = () => { if (!done) { clearInterval(intervalRef.current); setDisplayed(msg.content); setDone(true); } };

  const parseContent = (text) => text
    .replace(/\*\*(.*?)\*\*/g, `<strong style="color:${T.goldBright}">$1</strong>`)
    .replace(/\*(.*?)\*/g, `<em style="color:#b09060;font-style:italic">$1</em>`)
    .replace(/```state[\s\S]*?```/g, "")
    .replace(/\n\n/g, "<br/><br/>")
    .replace(/\n/g, "<br/>");

  if (msg.role === "system") return (
    <div style={{ textAlign: "center", margin: `${T.s2} auto`, padding: `3px ${T.s3}`, background: "rgba(180,130,60,0.06)", border: `1px solid ${T.border}`, borderRadius: T.r4, display: "inline-block", maxWidth: "90%" }}>
      <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.goldDim }}>{msg.content}</span>
    </div>
  );

  if (msg.role === "user") return (
    <div style={{ display: "flex", justifyContent: "flex-end", marginBottom: T.s4 }}>
      <div style={{ maxWidth: "78%", background: "rgba(60,40,10,0.7)", border: `1px solid rgba(180,130,60,0.25)`, borderRadius: "12px 12px 3px 12px", padding: `${T.s3} ${T.s4}` }}>
        <div style={{ fontFamily: T.body, fontSize: T.md, color: "#f0e0c0", lineHeight: 1.65 }}>{msg.content}</div>
      </div>
    </div>
  );

  return (
    <div style={{ display: "flex", gap: T.s3, marginBottom: T.s5, cursor: done ? "default" : "pointer" }} onClick={onClick}>
      <div style={{ width: 32, height: 32, borderRadius: "50%", flexShrink: 0, background: "linear-gradient(135deg,#6a3a0a,#3a1a00)", border: `1px solid ${T.border}`, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 14 }}>🎲</div>
      <div style={{ flex: 1 }}>
        <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, letterSpacing: "1px", marginBottom: T.s2 }}>DUNGEON MASTER</div>
        <div style={{ fontFamily: T.body, fontSize: 15, color: T.textPrimary, lineHeight: 1.8, whiteSpace: "pre-wrap" }}
          dangerouslySetInnerHTML={{ __html: parseContent(displayed) }} />
        {!done && <div style={{ width: 2, height: 16, background: T.gold, display: "inline-block", marginLeft: 2, animation: "blink 0.7s step-end infinite" }} />}
      </div>
    </div>
  );
};

// ─── COMBAT TRACKER ────────────────────────────────────────
const CombatTracker = ({ combat, dispatch, characterName }) => {
  if (!combat.active) return null;
  return (
    <div style={{ background: "rgba(80,20,20,0.15)", border: `1px solid rgba(180,60,60,0.3)`, borderRadius: T.r2, padding: T.s3, marginBottom: T.s3 }}>
      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: T.s3 }}>
        <span style={{ fontFamily: T.heading, fontSize: T.xs, color: "#e06060", letterSpacing: "1px" }}>⚔️ COMBAT — ROUND {combat.round}</span>
        <button onClick={() => dispatch({ type: "END_COMBAT" })} style={{ ...S.ghostBtn, color: "#e06060", fontSize: T.xs }}>End Combat</button>
      </div>
      <div style={{ display: "flex", gap: T.s2, flexWrap: "wrap", marginBottom: T.s3 }}>
        {combat.initiative.map((e, i) => {
          const isCurrent = i === combat.currentTurnIndex;
          const isPlayer = e.name === characterName;
          const hpPct = e.hp / e.maxHp;
          return (
            <div key={e.name} style={{ background: isCurrent ? "rgba(220,180,60,0.12)" : "rgba(255,255,255,0.03)", border: `1px solid ${isCurrent ? T.borderHot : "rgba(255,255,255,0.08)"}`, borderRadius: T.r1, padding: `${T.s2} ${T.s3}`, minWidth: 90, transition: "all 0.3s" }}>
              <div style={{ fontFamily: T.heading, fontSize: T.xs, color: isCurrent ? T.goldBright : isPlayer ? "#80b860" : "#e06060", fontWeight: isCurrent ? 700 : 400 }}>
                {isCurrent && "▶ "}{e.name}
              </div>
              <div style={{ display: "flex", gap: 2, marginTop: 3 }}>
                {[...Array(5)].map((_, j) => (
                  <div key={j} style={{ height: 4, flex: 1, borderRadius: 2, background: j < Math.ceil(hpPct * 5) ? (hpPct > 0.5 ? T.hp : hpPct > 0.25 ? T.hpMid : T.hpLow) : "rgba(255,255,255,0.06)" }} />
                ))}
              </div>
              <div style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted, marginTop: 2 }}>{e.hp}/{e.maxHp} HP</div>
            </div>
          );
        })}
      </div>
      <div style={{ display: "flex", gap: T.s2, flexWrap: "wrap" }}>
        <button onClick={() => dispatch({ type: "NEXT_TURN" })} style={{ ...S.primaryBtn, padding: `${T.s2} ${T.s3}`, fontSize: T.xs }}>Next Turn ▶</button>
        {combat.log.slice(0, 1).map((l, i) => <span key={i} style={{ fontFamily: T.body, fontSize: T.sm, color: T.textSecondary, alignSelf: "center" }}>{l}</span>)}
      </div>
    </div>
  );
};

// ─── BATTLE MAP ────────────────────────────────────────────
const TILE_SIZE = 44;
const MAP_W = 18;
const MAP_H = 12;

const BattleMap = ({ character }) => {
  const TILE_DEFS = {
    floor: { bg: "#1a1208", border: "#2a1e0e", symbol: "" },
    wall: { bg: "#0d0a06", border: "#2a1e10", symbol: "█" },
    water: { bg: "#080f1e", border: "#0d1c32", symbol: "≈" },
    lava: { bg: "#1e0800", border: "#4a1000", symbol: "∿" },
    grass: { bg: "#0a1408", border: "#152010", symbol: "‥" },
    door: { bg: "#1e1008", border: "#8a5a1a", symbol: "▮" },
    chest: { bg: "#1e1a06", border: "#b8980a", symbol: "◆" },
    stairs: { bg: "#10141e", border: "#304060", symbol: "⊞" },
    trap: { bg: "#1a0a0a", border: "#6a0a0a", symbol: "✕" },
  };

  const initMap = () => {
    return Array.from({ length: MAP_H }, (_, y) => Array.from({ length: MAP_W }, (_, x) => {
      if (x === 0 || x === MAP_W - 1 || y === 0 || y === MAP_H - 1) return "wall";
      if (y === 5 && x !== 7 && x !== 8) return "wall";
      if (y === 5 && (x === 7 || x === 8)) return "door";
      return "floor";
    }));
  };

  const [tiles, setTiles] = useState(initMap);
  const [tokens, setTokens] = useState(() => [
    { id: "player", x: 5, y: 7, icon: character?.name?.[0] || "P", color: "#3d9e58", hp: character?.currentHp || 20, maxHp: character?.hp || 20, type: "player", name: character?.name || "Player", ac: 14, initiative: 15 },
    { id: "enemy1", x: 13, y: 3, icon: "G", color: "#b83030", hp: 7, maxHp: 7, type: "enemy", name: "Goblin", ac: 15, initiative: 14 },
    { id: "enemy2", x: 14, y: 8, icon: "O", color: "#b85020", hp: 15, maxHp: 15, type: "enemy", name: "Orc", ac: 13, initiative: 10 },
    { id: "ally1", x: 4, y: 8, icon: "A", color: "#3060a8", hp: 20, maxHp: 20, type: "ally", name: "Ally", ac: 12, initiative: 12 },
  ]);
  const [selected, setSelected] = useState(null);
  const [paintTile, setPaintTile] = useState("wall");
  const [editMode, setEditMode] = useState(false);
  const [rangeHighlight, setRangeHighlight] = useState([]);
  const [zoom, setZoom] = useState(0.85);
  const [log, setLog] = useState(["Battle map loaded. Select a token to begin."]);
  const [showAddToken, setShowAddToken] = useState(false);
  const [newToken, setNewToken] = useState({ name: "Skeleton", hp: 13, maxHp: 13, icon: "S", color: "#b83030", type: "enemy", ac: 13 });
  const [combatOrder, setCombatOrder] = useState([]);
  const [currentTurn, setCurrentTurn] = useState(0);

  const addLog = (msg) => setLog(p => [msg, ...p].slice(0, 25));
  const selToken = tokens.find(t => t.id === selected);

  const rollInit = () => {
    const order = [...tokens].map(t => ({ ...t, initRoll: DnD.rollDie(20) + (t.type === "player" ? 2 : 1) })).sort((a, b) => b.initRoll - a.initRoll);
    setCombatOrder(order);
    setCurrentTurn(0);
    addLog("Initiative rolled! " + order.map(t => `${t.name}(${t.initRoll})`).join(" > "));
  };

  const getRangeCells = (token, ft) => {
    if (!token) return [];
    const squares = Math.ceil(ft / 5);
    const cells = [];
    for (let dy = -squares; dy <= squares; dy++) {
      for (let dx = -squares; dx <= squares; dx++) {
        const dist = Math.max(Math.abs(dx), Math.abs(dy));
        if (dist <= squares && dist > 0) {
          const nx = token.x + dx, ny = token.y + dy;
          if (nx >= 0 && nx < MAP_W && ny >= 0 && ny < MAP_H) cells.push(`${nx},${ny}`);
        }
      }
    }
    return cells;
  };

  const handleTileClick = (x, y, e) => {
    e?.stopPropagation();
    if (editMode) { setTiles(p => p.map((r, ry) => r.map((c, cx) => ry === y && cx === x ? paintTile : c))); return; }
    const tok = tokens.find(t => t.x === x && t.y === y);
    if (tok) {
      if (selected && selected !== tok.id && selToken) {
        if ((tok.type === "enemy" && selToken.type !== "enemy") || (tok.type === "player" && selToken.type === "enemy")) {
          const atkRoll = DnD.rollDie(20) + 4;
          const hit = atkRoll >= tok.ac;
          const dmg = hit ? DnD.roll(1, 8, 3) : 0;
          if (hit) { setTokens(p => p.map(t => t.id === tok.id ? { ...t, hp: Math.max(0, t.hp - dmg) } : t)); }
          addLog(`${selToken.name} → ${tok.name}: ${atkRoll >= 20 ? "CRIT! " : ""}${hit ? `Hit! ${dmg} damage` : `Miss (${atkRoll} vs AC ${tok.ac})`}`);
          setSelected(null); setRangeHighlight([]);
          return;
        }
      }
      setSelected(s => s === tok.id ? null : tok.id);
      setRangeHighlight([]);
      return;
    }
    if (selected && selToken && tiles[y][x] !== "wall") {
      const dist = Math.max(Math.abs(x - selToken.x) * 5, Math.abs(y - selToken.y) * 5);
      setTokens(p => p.map(t => t.id === selected ? { ...t, x, y } : t));
      addLog(`${selToken.name} moves to (${x},${y}) — ${dist}ft used`);
      setSelected(null); setRangeHighlight([]);
    }
  };

  const rangeSet = new Set(rangeHighlight);
  const selPos = selToken ? `${selToken.x},${selToken.y}` : "";

  return (
    <div style={{ height: "100%", display: "flex", flexDirection: "column", overflow: "hidden", background: T.bgDeep }}>
      <div style={{ padding: `${T.s2} ${T.s3}`, borderBottom: `1px solid ${T.border}`, display: "flex", gap: T.s2, alignItems: "center", flexWrap: "wrap", background: "rgba(6,4,8,0.98)", flexShrink: 0 }}>
        <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.gold, letterSpacing: "1px" }}>BATTLE MAP</span>
        <div style={{ width: 1, height: 18, background: T.border }} />
        <button onClick={rollInit} style={{ ...S.primaryBtn, padding: `${T.s2} ${T.s3}`, fontSize: T.xs }}>⚔️ Roll Initiative</button>
        <button onClick={() => setEditMode(p => !p)} style={{ ...S.secondaryBtn, padding: `${T.s2} ${T.s3}`, fontSize: T.xs, background: editMode ? "rgba(180,130,60,0.2)" : undefined }}>✏️ {editMode ? "Painting" : "Edit Map"}</button>
        {editMode && Object.keys(TILE_DEFS).map(t => (
          <Rune key={t} active={paintTile === t} onClick={() => setPaintTile(t)}>{TILE_DEFS[t].symbol || "·"} {t}</Rune>
        ))}
        {!editMode && <button onClick={() => setShowAddToken(p => !p)} style={{ ...S.secondaryBtn, padding: `${T.s2} ${T.s3}`, fontSize: T.xs }}>+ Token</button>}
        <div style={{ marginLeft: "auto", display: "flex", gap: T.s2, alignItems: "center" }}>
          <button onClick={() => setZoom(p => Math.max(0.4, p - 0.15))} style={{ ...S.ghostBtn, padding: `2px 8px` }}>−</button>
          <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textSecondary }}>{Math.round(zoom * 100)}%</span>
          <button onClick={() => setZoom(p => Math.min(1.5, p + 0.15))} style={{ ...S.ghostBtn, padding: `2px 8px` }}>+</button>
        </div>
      </div>

      {showAddToken && (
        <div style={{ padding: `${T.s2} ${T.s3}`, borderBottom: `1px solid ${T.border}`, background: "rgba(10,7,12,0.98)", display: "flex", gap: T.s2, alignItems: "center", flexWrap: "wrap", flexShrink: 0 }}>
          {[["Name", { value: newToken.name, onChange: e => setNewToken(p => ({ ...p, name: e.target.value })), style: { width: 100 } }],
            ["HP", { type: "number", value: newToken.hp, onChange: e => setNewToken(p => ({ ...p, hp: Number(e.target.value), maxHp: Number(e.target.value) })), style: { width: 60 } }],
            ["AC", { type: "number", value: newToken.ac, onChange: e => setNewToken(p => ({ ...p, ac: Number(e.target.value) })), style: { width: 50 } }],
            ["Icon", { value: newToken.icon, onChange: e => setNewToken(p => ({ ...p, icon: e.target.value.slice(0, 2) })), style: { width: 45 } }],
          ].map(([label, props]) => (
            <div key={label} style={{ display: "flex", alignItems: "center", gap: 4 }}>
              <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textSecondary }}>{label}</span>
              <input {...props} style={{ ...props.style, padding: "4px 6px", background: "rgba(180,130,60,0.08)", border: `1px solid ${T.border}`, borderRadius: 4, color: T.textPrimary, fontFamily: T.body, fontSize: T.sm }} />
            </div>
          ))}
          <select value={newToken.type} onChange={e => setNewToken(p => ({ ...p, type: e.target.value, color: e.target.value === "enemy" ? "#b83030" : e.target.value === "ally" ? "#3060a8" : "#9060b0" }))}
            style={{ padding: "4px 8px", background: "#150d04", border: `1px solid ${T.border}`, borderRadius: 4, color: T.textPrimary, fontSize: T.sm }}>
            <option value="enemy" style={{ background: "#150d04" }}>Enemy</option>
            <option value="ally" style={{ background: "#150d04" }}>Ally</option>
            <option value="npc" style={{ background: "#150d04" }}>NPC</option>
          </select>
          <button onClick={() => {
            const id = `tok_${Date.now()}`;
            setTokens(p => [...p, { ...newToken, id, x: 9, y: 5 }]);
            setShowAddToken(false);
            addLog(`${newToken.name} enters the battlefield!`);
          }} style={{ ...S.primaryBtn, padding: `${T.s2} ${T.s3}`, fontSize: T.xs }}>Add</button>
        </div>
      )}

      {combatOrder.length > 0 && (
        <div style={{ padding: `${T.s2} ${T.s3}`, borderBottom: `1px solid ${T.border}`, display: "flex", gap: T.s2, alignItems: "center", overflowX: "auto", flexShrink: 0 }}>
          <span style={{ fontFamily: T.heading, fontSize: T.xs, color: "#e06060", letterSpacing: "1px", flexShrink: 0 }}>TURN ORDER:</span>
          {combatOrder.map((t, i) => (
            <div key={t.id} onClick={() => { setCurrentTurn(i); addLog(`${t.name}'s turn.`); }}
              style={{ padding: `2px 10px`, borderRadius: T.r4, border: `1px solid ${i === currentTurn ? T.borderHot : T.border}`, background: i === currentTurn ? "rgba(220,180,60,0.15)" : "transparent", cursor: "pointer", flexShrink: 0 }}>
              <span style={{ fontFamily: T.heading, fontSize: T.xs, color: i === currentTurn ? T.goldBright : t.color }}>{i === currentTurn ? "▶ " : ""}{t.name} ({t.initRoll})</span>
            </div>
          ))}
          <button onClick={() => setCurrentTurn(p => (p + 1) % combatOrder.length)} style={{ ...S.primaryBtn, padding: `2px 10px`, fontSize: T.xs, flexShrink: 0 }}>Next ▶</button>
        </div>
      )}

      <div style={{ flex: 1, display: "flex", overflow: "hidden" }}>
        <div style={{ flex: 1, overflow: "auto", background: "#050403", cursor: selToken && !editMode ? "crosshair" : "default" }}>
          <div style={{ display: "inline-block", transformOrigin: "top left", transform: `scale(${zoom})`, padding: 20 }}>
            <div style={{ position: "relative", width: MAP_W * TILE_SIZE, height: MAP_H * TILE_SIZE }}>
              {tiles.map((row, y) => row.map((tile, x) => {
                const def = TILE_DEFS[tile] || TILE_DEFS.floor;
                const key = `${x},${y}`;
                const isSel = selPos === key;
                const inRange = rangeSet.has(key);
                const hasTok = tokens.some(t => t.x === x && t.y === y);
                return (
                  <div key={key}
                    onClick={(e) => handleTileClick(x, y, e)}
                    style={{
                      position: "absolute", left: x * TILE_SIZE, top: y * TILE_SIZE, width: TILE_SIZE - 1, height: TILE_SIZE - 1,
                      background: isSel ? "rgba(220,180,60,0.2)" : inRange ? "rgba(80,120,200,0.15)" : def.bg,
                      border: `1px solid ${isSel ? "#d4b440" : inRange ? "#4060c0" : def.border}`,
                      display: "flex", alignItems: "center", justifyContent: "center",
                      fontSize: 11, color: "rgba(255,255,255,0.15)",
                      cursor: tile === "wall" ? "not-allowed" : "pointer",
                      userSelect: "none",
                    }}>
                    {!hasTok && def.symbol}
                    <span style={{ position: "absolute", bottom: 1, right: 2, fontSize: 7, color: "rgba(255,255,255,0.1)", fontFamily: T.mono }}>{x},{y}</span>
                  </div>
                );
              }))}

              {tokens.filter(t => t.hp > 0).map(tok => {
                const isSel = selected === tok.id;
                const canAttack = selToken && selToken.id !== tok.id && ((tok.type === "enemy" && selToken.type !== "enemy") || (tok.type === "player" && selToken.type === "enemy"));
                const hpPct = tok.hp / tok.maxHp;
                const hpColor = hpPct > 0.6 ? T.hp : hpPct > 0.25 ? T.hpMid : T.hpLow;
                return (
                  <div key={tok.id}
                    onClick={(e) => handleTileClick(tok.x, tok.y, e)}
                    style={{
                      position: "absolute",
                      left: tok.x * TILE_SIZE + 2, top: tok.y * TILE_SIZE + 2,
                      width: TILE_SIZE - 4, height: TILE_SIZE - 4,
                      borderRadius: "50%",
                      background: `radial-gradient(circle at 35% 35%, ${tok.color}cc, ${tok.color}66)`,
                      border: `2px solid ${isSel ? "#ffffff" : canAttack ? "#ff6060" : "rgba(255,255,255,0.25)"}`,
                      boxShadow: isSel ? `0 0 16px ${tok.color}, 0 0 4px ${tok.color}` : canAttack ? "0 0 10px rgba(255,80,80,0.6)" : "0 2px 8px rgba(0,0,0,0.6)",
                      display: "flex", alignItems: "center", justifyContent: "center",
                      fontFamily: T.heading, fontWeight: 700, color: "white", fontSize: 13,
                      cursor: "pointer", zIndex: 10, transition: "all 0.2s",
                      userSelect: "none",
                    }}>
                    {tok.icon}
                    <div style={{ position: "absolute", bottom: -7, left: 0, right: 0, height: 3, background: "rgba(0,0,0,0.6)", borderRadius: 2, overflow: "hidden" }}>
                      <div style={{ height: "100%", width: `${(tok.hp / tok.maxHp) * 100}%`, background: hpColor, transition: "width 0.4s" }} />
                    </div>
                  </div>
                );
              })}
            </div>
          </div>
        </div>

        <div style={{ width: 200, background: "rgba(6,4,8,0.98)", borderLeft: `1px solid ${T.border}`, display: "flex", flexDirection: "column", overflow: "hidden", flexShrink: 0 }}>
          {selToken ? (
            <div style={{ padding: T.s3, borderBottom: `1px solid ${T.border}` }}>
              <div style={{ fontFamily: T.heading, fontSize: T.sm, color: selToken.color, fontWeight: 700, marginBottom: 4 }}>{selToken.name}</div>
              <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, marginBottom: T.s2 }}>{selToken.type.toUpperCase()} · AC {selToken.ac} · ({selToken.x},{selToken.y})</div>
              <HPBar current={selToken.hp} max={selToken.maxHp} height={7} />
              <div style={{ display: "flex", gap: T.s2, marginTop: T.s3, flexWrap: "wrap" }}>
                <button onClick={() => { setTokens(p => p.map(t => t.id === selected ? { ...t, hp: Math.max(0, t.hp - 1) } : t)); addLog(`${selToken.name} takes 1 damage.`); }} style={{ ...S.secondaryBtn, padding: `2px 8px`, fontSize: T.xs, flex: 1, color: T.hpLow, borderColor: "rgba(180,60,60,0.4)" }}>-1</button>
                <button onClick={() => { setTokens(p => p.map(t => t.id === selected ? { ...t, hp: Math.min(t.maxHp, t.hp + 1) } : t)); addLog(`${selToken.name} heals 1 HP.`); }} style={{ ...S.primaryBtn, padding: `2px 8px`, fontSize: T.xs, flex: 1 }}>+1</button>
              </div>
              <div style={{ display: "flex", gap: 4, marginTop: T.s2 }}>
                <button onClick={() => setRangeHighlight(getRangeCells(selToken, 30))} style={{ ...S.ghostBtn, padding: `2px 6px`, fontSize: 9, flex: 1 }}>30ft</button>
                <button onClick={() => setRangeHighlight(getRangeCells(selToken, 60))} style={{ ...S.ghostBtn, padding: `2px 6px`, fontSize: 9, flex: 1 }}>60ft</button>
                <button onClick={() => setRangeHighlight([])} style={{ ...S.ghostBtn, padding: `2px 6px`, fontSize: 9, flex: 1 }}>✕</button>
              </div>
              <button onClick={() => { setTokens(p => p.filter(t => t.id !== selected)); addLog(`${selToken.name} removed.`); setSelected(null); }} style={{ ...S.ghostBtn, padding: `4px 0`, fontSize: T.xs, width: "100%", color: "#e06060", marginTop: 4 }}>Remove</button>
            </div>
          ) : (
            <div style={{ padding: T.s3, borderBottom: `1px solid ${T.border}`, color: T.textMuted, fontFamily: T.body, fontSize: T.sm, lineHeight: 1.6 }}>
              Tap token to select.<br />Tap tile to move.<br />Tap enemy to attack.
            </div>
          )}

          <div style={{ flex: 1, overflow: "auto", padding: T.s3 }}>
            <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, letterSpacing: "1px", marginBottom: T.s2 }}>COMBATANTS</div>
            {tokens.map(tok => (
              <div key={tok.id} onClick={() => setSelected(s => s === tok.id ? null : tok.id)}
                style={{ padding: `${T.s2} ${T.s3}`, borderRadius: T.r1, marginBottom: 4, cursor: "pointer", border: `1px solid ${selected === tok.id ? T.border : "transparent"}`, background: selected === tok.id ? "rgba(180,130,60,0.08)" : "transparent" }}>
                <div style={{ display: "flex", justifyContent: "space-between" }}>
                  <span style={{ fontFamily: T.heading, fontSize: T.xs, color: tok.hp <= 0 ? "#666" : tok.color, textDecoration: tok.hp <= 0 ? "line-through" : "none" }}>{tok.name}</span>
                  <span style={{ fontFamily: T.heading, fontSize: T.xs, color: tok.hp / tok.maxHp > 0.5 ? T.hp : T.hpLow }}>{tok.hp}/{tok.maxHp}</span>
                </div>
              </div>
            ))}
          </div>

          <div style={{ padding: T.s3, borderTop: `1px solid ${T.border}`, maxHeight: 160, overflow: "auto" }}>
            <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, letterSpacing: "1px", marginBottom: T.s2 }}>COMBAT LOG</div>
            {log.map((l, i) => (
              <div key={i} style={{ fontFamily: T.body, fontSize: T.xs, color: i === 0 ? T.textSecondary : T.textMuted, marginBottom: 4, lineHeight: 1.5 }}>{l}</div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );
};

// ─── CHARACTER CREATION ────────────────────────────────────
const CharCreation = ({ onComplete }) => {
  const [step, setStep] = useState(0);
  const STEPS = ["Welcome", "Name", "Race", "Classes", "Subclass", "Background", "Abilities", "Feats", "Finalize"];
  const [draft, setDraft] = useState({
    name: "", race: null, classes: [], background: null, alignment: "Neutral Good",
    abilityScores: { str: 10, dex: 10, con: 10, int: 10, wis: 10, cha: 10 },
    feats: [], spells: [], equipment: [], backstory: "", notes: "",
    skillProficiencies: [], languages: ["Common"], toolProficiencies: [],
  });
  const [aiLoading, setAiLoading] = useState(false);
  const [aiOutput, setAiOutput] = useState({ backstory: "", recommendations: "" });
  const [raceFilter, setRaceFilter] = useState("All");
  const [bgFilter, setBgFilter] = useState("All");
  const [featFilter, setFeatFilter] = useState("All");
  const up = (k, v) => setDraft(p => ({ ...p, [k]: v }));

  const raceSources = ["All", ...new Set(DATA.races.map(r => r.source))];
  const bgSources = ["All", ...new Set(DATA.backgrounds.map(b => b.src))];
  const featSources = ["All", ...new Set(DATA.feats.map(f => f.src))];
  const totalLevel = draft.classes.reduce((s, c) => s + (c.level || 1), 0) || 1;

  const generateAI = async (mode) => {
    setAiLoading(true);
    const sys = mode === "backstory"
      ? "You are a D&D 5e lore expert. Write a vivid, emotionally resonant character backstory (180 words). Include: origin, a defining tragedy or triumph, their core motivation, a dangerous secret or flaw, and what drove them to adventure. Be specific. Avoid clichés. Make it feel like a real person."
      : "You are a D&D 5e optimization expert. Given this character build, provide 5 specific recommendations covering: (1) best subclass choice and why, (2) top 2 feat priorities, (3) spell selections if applicable, (4) ability score priority for ASIs, (5) one synergy or combo they should know about. Be direct and specific, not generic.";
    const msg = `${draft.name || "Hero"} — ${draft.race || "Unknown race"} ${draft.classes.map(c => `${c.name} ${c.level}`).join("/") || "adventurer"}. Background: ${draft.background || "None"}. Alignment: ${draft.alignment}. ${draft.backstory ? "Player notes: " + draft.backstory : ""}`;
    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", { method: "POST", headers: { "Content-Type": "application/json" }, body: JSON.stringify({ model: "claude-sonnet-4-20250514", max_tokens: 600, system: sys, messages: [{ role: "user", content: msg }] }) });
      const data = await res.json();
      const text = data.content?.map(b => b.text || "").join("") || "";
      setAiOutput(p => ({ ...p, [mode]: text }));
      if (mode === "backstory") up("backstory", text);
    } catch (e) { console.error(e); }
    setAiLoading(false);
  };

  const finalize = () => {
    const classes = draft.classes.length ? draft.classes : [{ name: "Fighter", level: 1, subclass: "" }];
    const conScore = draft.abilityScores.con;
    const maxHp = DnD.calcMaxHP(classes, conScore);
    const bgSkills = DATA.backgrounds.find(b => b.name === draft.background)?.skills || [];
    onComplete({
      ...draft,
      classes, class: classes[0].name, level: classes.reduce((s, c) => s + c.level, 0),
      hp: maxHp, currentHp: maxHp, tempHp: 0,
      skillProficiencies: [...new Set([...draft.skillProficiencies, ...bgSkills])],
      xp: 0, conditions: [],
    });
  };

  const stepStyle = (i) => ({
    padding: `3px 9px`, borderRadius: 12, fontFamily: T.heading, fontSize: T.xs, cursor: i < step ? "pointer" : "default",
    background: i === step ? "linear-gradient(135deg,#8a5c1a,#5c3a0a)" : i < step ? "rgba(180,130,60,0.15)" : "transparent",
    color: i === step ? T.goldBright : i < step ? T.gold : T.textMuted,
    border: `1px solid ${i === step ? "transparent" : i < step ? T.border : "rgba(255,255,255,0.05)"}`,
    minHeight: "28px",
  });

  return (
    <div style={{ height: "100%", overflow: "auto", padding: T.s4 }}>
      <div style={{ display: "flex", gap: T.s2, marginBottom: T.s5, flexWrap: "wrap" }}>
        {STEPS.map((s, i) => (
          <button key={i} onClick={() => i < step && setStep(i)} style={stepStyle(i)}>
            {i < step ? "✓ " : ""}{s}
          </button>
        ))}
      </div>

      {step === 0 && (
        <div style={{ textAlign: "center", maxWidth: 540, margin: "0 auto", paddingTop: T.s5 }}>
          <div style={{ fontSize: 56, marginBottom: T.s4 }}>⚔️</div>
          <h1 style={{ fontFamily: T.display, color: T.gold, fontSize: 24, margin: `0 0 ${T.s3}` }}>Forge Your Legend</h1>
          <p style={{ color: T.textSecondary, fontFamily: T.body, fontSize: T.md, lineHeight: 1.8, marginBottom: T.s5 }}>
            Create your hero using every official D&D 5e source from 2014 through 2024 — plus third-party content. AI assistance helps you craft your backstory and optimize your build.
          </p>
          <div style={{ display: "flex", gap: T.s3, justifyContent: "center", flexWrap: "wrap", marginBottom: T.s5 }}>
            {["50+ Races", "13 Classes + Artificer", "Multiclassing", "Subclasses", "100+ Feats", "AI Backstory", "Build Advisor"].map(f => (
              <span key={f} style={{ background: "rgba(180,130,60,0.08)", border: `1px solid ${T.border}`, borderRadius: T.r4, padding: `3px 10px`, fontFamily: T.heading, fontSize: T.xs, color: T.gold }}>{f}</span>
            ))}
          </div>
          <button onClick={() => setStep(1)} style={{ ...S.primaryBtn, fontSize: T.lg, padding: `${T.s4} ${T.s6}` }}>Begin Character Creation →</button>
        </div>
      )}

      {step === 1 && (
        <div style={{ maxWidth: 480, margin: "0 auto" }}>
          <h2 style={{ fontFamily: T.display, color: T.gold, fontSize: 20, marginBottom: T.s2 }}>Name & Alignment</h2>
          <div style={{ marginBottom: T.s4 }}>
            <label style={S.label}>CHARACTER NAME</label>
            <input value={draft.name} onChange={e => up("name", e.target.value)} placeholder="Enter your name..." style={{ ...S.input, fontSize: 20 }} />
          </div>
          <div style={{ marginBottom: T.s5 }}>
            <label style={S.label}>ALIGNMENT</label>
            <select value={draft.alignment} onChange={e => up("alignment", e.target.value)} style={{ ...S.input, fontFamily: T.heading }}>
              {["Lawful Good", "Neutral Good", "Chaotic Good", "Lawful Neutral", "True Neutral", "Chaotic Neutral", "Lawful Evil", "Neutral Evil", "Chaotic Evil"].map(a => <option key={a} value={a} style={{ background: "#150d04" }}>{a}</option>)}
            </select>
          </div>
          <div style={{ display: "flex", gap: T.s3 }}>
            <button onClick={() => setStep(0)} style={S.secondaryBtn}>← Back</button>
            <button onClick={() => draft.name && setStep(2)} style={{ ...S.primaryBtn, opacity: draft.name ? 1 : 0.5 }} disabled={!draft.name}>Choose Race →</button>
          </div>
        </div>
      )}

      {step === 2 && (
        <div>
          <h2 style={{ fontFamily: T.display, color: T.gold, fontSize: 20, marginBottom: T.s2 }}>Choose Your Race</h2>
          <div style={{ display: "flex", gap: T.s2, marginBottom: T.s3, flexWrap: "wrap" }}>
            {raceSources.map(src => <Rune key={src} active={raceFilter === src} onClick={() => setRaceFilter(src)}>{src}</Rune>)}
          </div>
          <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill,minmax(200px,1fr))", gap: T.s3, marginBottom: T.s4 }}>
            {DATA.races.filter(r => raceFilter === "All" || r.source === raceFilter).map(r => (
              <div key={r.name} onClick={() => up("race", r.name)} style={{ ...S.card, cursor: "pointer", border: `1px solid ${draft.race === r.name ? T.borderHot : T.border}`, background: draft.race === r.name ? "rgba(180,130,60,0.12)" : T.surface, transition: "all 0.15s" }}>
                <div style={{ display: "flex", justifyContent: "space-between", marginBottom: T.s2 }}>
                  <span style={{ fontFamily: T.heading, fontSize: T.sm, color: T.textPrimary, fontWeight: 700 }}>{r.name}</span>
                  <span style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted, background: "rgba(180,130,60,0.1)", padding: "1px 5px", borderRadius: 3 }}>{r.source}</span>
                </div>
                <div style={{ display: "flex", flexWrap: "wrap", gap: 3, marginBottom: T.s2 }}>
                  {Object.entries(r.ab).map(([ab, v]) => (
                    <span key={ab} style={{ fontFamily: T.heading, fontSize: 9, color: "#70a060", background: "rgba(80,160,60,0.1)", border: "1px solid rgba(80,160,60,0.2)", padding: "1px 5px", borderRadius: 3 }}>+{v} {ab.toUpperCase()}</span>
                  ))}
                </div>
                <div style={{ color: T.textMuted, fontSize: T.xs, fontFamily: T.heading, marginBottom: T.s2 }}>Spd {r.spd}ft · {r.sz}</div>
                {r.traits.slice(0, 2).map(t => <div key={t} style={{ color: T.textSecondary, fontSize: T.xs, fontFamily: T.body }}>• {t}</div>)}
              </div>
            ))}
          </div>
          <div style={{ display: "flex", gap: T.s3 }}>
            <button onClick={() => setStep(1)} style={S.secondaryBtn}>← Back</button>
            <button onClick={() => draft.race && setStep(3)} style={{ ...S.primaryBtn, opacity: draft.race ? 1 : 0.5 }} disabled={!draft.race}>Choose Class →</button>
          </div>
        </div>
      )}

      {step === 3 && (
        <div>
          <h2 style={{ fontFamily: T.display, color: T.gold, fontSize: 20, marginBottom: T.s2 }}>Choose Class(es)</h2>
          <p style={{ color: T.textSecondary, fontFamily: T.body, fontSize: T.md, marginBottom: T.s3 }}>Multiclass freely. Set starting level per class.</p>
          {draft.classes.length > 0 && (
            <div style={{ ...S.card, marginBottom: T.s4 }}>
              <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.gold, letterSpacing: "1px", marginBottom: T.s3 }}>YOUR BUILD — {totalLevel} TOTAL LEVELS</div>
              {draft.classes.map((cc, i) => (
                <div key={i} style={{ display: "flex", gap: T.s2, alignItems: "center", marginBottom: T.s2 }}>
                  <span style={{ color: T.textPrimary, fontFamily: T.body, fontSize: T.md, flex: 1 }}>{cc.name}</span>
                  <span style={{ color: T.textSecondary, fontFamily: T.heading, fontSize: T.xs }}>Level</span>
                  <input type="number" min={1} max={20} value={cc.level || 1} onChange={e => up("classes", draft.classes.map((c, j) => j === i ? { ...c, level: Number(e.target.value) } : c))}
                    style={{ width: 50, ...S.input, padding: `${T.s2} ${T.s2}`, fontSize: T.md }} />
                  <button onClick={() => up("classes", draft.classes.filter((_, j) => j !== i))} style={{ ...S.ghostBtn, color: T.hpLow, padding: `${T.s2} ${T.s3}` }}>✕</button>
                </div>
              ))}
            </div>
          )}
          <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill,minmax(220px,1fr))", gap: T.s3, marginBottom: T.s4 }}>
            {DATA.classes.map(c => {
              const picked = draft.classes.some(cc => cc.name === c.name);
              return (
                <div key={c.name} onClick={() => { if (!picked) up("classes", [...draft.classes, { name: c.name, level: 1, subclass: "" }]); }}
                  style={{ ...S.card, cursor: picked ? "default" : "pointer", opacity: picked ? 0.6 : 1, border: `1px solid ${picked ? T.borderHot : T.border}`, background: picked ? "rgba(180,130,60,0.1)" : T.surface, transition: "all 0.15s" }}>
                  <div style={{ display: "flex", justifyContent: "space-between", marginBottom: T.s2 }}>
                    <span style={{ fontFamily: T.heading, fontSize: T.sm, color: T.textPrimary, fontWeight: 700 }}>{c.name}</span>
                    <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.gold, background: "rgba(180,130,60,0.1)", padding: "1px 7px", borderRadius: 3 }}>d{c.hd}</span>
                  </div>
                  <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textSecondary, marginBottom: T.s2 }}>{c.pri} · Saves: {c.saves.map(s => s.toUpperCase()).join(", ")}</div>
                  <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted }}>{c.armor.join(", ") || "No armor"}</div>
                </div>
              );
            })}
          </div>
          <div style={{ display: "flex", gap: T.s3 }}>
            <button onClick={() => setStep(2)} style={S.secondaryBtn}>← Back</button>
            <button onClick={() => draft.classes.length && setStep(4)} style={{ ...S.primaryBtn, opacity: draft.classes.length ? 1 : 0.5 }} disabled={!draft.classes.length}>Choose Subclass →</button>
          </div>
        </div>
      )}

      {step === 4 && (
        <div>
          <h2 style={{ fontFamily: T.display, color: T.gold, fontSize: 20, marginBottom: T.s2 }}>Subclass</h2>
          {draft.classes.map((cc, i) => (
            <div key={i} style={{ marginBottom: T.s5 }}>
              <div style={{ fontFamily: T.heading, fontSize: T.sm, color: T.gold, letterSpacing: "1px", marginBottom: T.s3 }}>{cc.name.toUpperCase()} SUBCLASS</div>
              <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill,minmax(185px,1fr))", gap: T.s2 }}>
                {(DATA.classes.find(c => c.name === cc.name)?.subclasses || []).map(sub => (
                  <div key={sub} onClick={() => up("classes", draft.classes.map((c2, j) => j === i ? { ...c2, subclass: sub } : c2))}
                    style={{ ...S.card, cursor: "pointer", padding: T.s3, border: `1px solid ${cc.subclass === sub ? T.borderHot : T.border}`, background: cc.subclass === sub ? "rgba(180,130,60,0.12)" : T.surface, transition: "all 0.12s" }}>
                    <div style={{ fontFamily: T.heading, fontSize: T.xs, color: cc.subclass === sub ? T.goldBright : T.textPrimary }}>{sub}</div>
                  </div>
                ))}
              </div>
            </div>
          ))}
          <div style={{ display: "flex", gap: T.s3 }}>
            <button onClick={() => setStep(3)} style={S.secondaryBtn}>← Back</button>
            <button onClick={() => setStep(5)} style={S.primaryBtn}>Background →</button>
          </div>
        </div>
      )}

      {step === 5 && (
        <div>
          <h2 style={{ fontFamily: T.display, color: T.gold, fontSize: 20, marginBottom: T.s2 }}>Background</h2>
          <div style={{ display: "flex", gap: T.s2, marginBottom: T.s3, flexWrap: "wrap" }}>
            {bgSources.map(s => <Rune key={s} active={bgFilter === s} onClick={() => setBgFilter(s)}>{s}</Rune>)}
          </div>
          <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill,minmax(185px,1fr))", gap: T.s2, marginBottom: T.s4 }}>
            {DATA.backgrounds.filter(b => bgFilter === "All" || b.src === bgFilter).map(b => (
              <div key={b.name} onClick={() => up("background", b.name)}
                style={{ ...S.card, cursor: "pointer", padding: T.s3, border: `1px solid ${draft.background === b.name ? T.borderHot : T.border}`, background: draft.background === b.name ? "rgba(180,130,60,0.12)" : T.surface, transition: "all 0.12s" }}>
                <div style={{ display: "flex", justifyContent: "space-between", marginBottom: T.s2 }}>
                  <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textPrimary, fontWeight: 700 }}>{b.name}</span>
                  <span style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted }}>{b.src}</span>
                </div>
                <div style={{ fontFamily: T.body, fontSize: T.xs, color: T.textSecondary, marginBottom: 4 }}>{b.skills.join(", ")}</div>
                <div style={{ fontFamily: T.body, fontSize: T.xs, color: T.textMuted }}>{b.feature}</div>
              </div>
            ))}
          </div>
          <div style={{ display: "flex", gap: T.s3 }}>
            <button onClick={() => setStep(4)} style={S.secondaryBtn}>← Back</button>
            <button onClick={() => draft.background && setStep(6)} style={{ ...S.primaryBtn, opacity: draft.background ? 1 : 0.5 }} disabled={!draft.background}>Abilities →</button>
          </div>
        </div>
      )}

      {step === 6 && (
        <div style={{ maxWidth: 640, margin: "0 auto" }}>
          <h2 style={{ fontFamily: T.display, color: T.gold, fontSize: 20, marginBottom: T.s2 }}>Ability Scores</h2>
          <div style={{ display: "flex", gap: T.s2, flexWrap: "wrap", justifyContent: "center", marginBottom: T.s4 }}>
            {Object.entries(draft.abilityScores).map(([ab, score]) => (
              <AbilityBox key={ab} label={ab.toUpperCase()} score={score} onChange={val => up("abilityScores", { ...draft.abilityScores, [ab]: val })} />
            ))}
          </div>
          <div style={{ display: "flex", gap: T.s2, justifyContent: "center", marginBottom: T.s4, flexWrap: "wrap" }}>
            <button onClick={() => up("abilityScores", { str: DnD.roll4d6(), dex: DnD.roll4d6(), con: DnD.roll4d6(), int: DnD.roll4d6(), wis: DnD.roll4d6(), cha: DnD.roll4d6() })} style={{ ...S.secondaryBtn, padding: `${T.s2} ${T.s3}`, fontSize: T.sm }}>🎲 Roll 4d6 Drop Low</button>
            <button onClick={() => up("abilityScores", { str: 15, dex: 14, con: 13, int: 12, wis: 10, cha: 8 })} style={{ ...S.secondaryBtn, padding: `${T.s2} ${T.s3}`, fontSize: T.sm }}>📋 Standard Array</button>
            <button onClick={() => up("abilityScores", { str: 13, dex: 13, con: 13, int: 13, wis: 13, cha: 13 })} style={{ ...S.secondaryBtn, padding: `${T.s2} ${T.s3}`, fontSize: T.sm }}>⚖️ Point Buy</button>
          </div>
          <div style={{ ...S.card, marginBottom: T.s4, border: `1px solid rgba(112,96,184,0.3)`, background: "rgba(112,96,184,0.06)" }}>
            <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.magicBright, letterSpacing: "1px", marginBottom: T.s3 }}>✨ AI CHARACTER ASSISTANT</div>
            <div style={{ display: "flex", gap: T.s2, marginBottom: T.s3, flexWrap: "wrap" }}>
              <button onClick={() => generateAI("backstory")} disabled={aiLoading} style={{ ...S.primaryBtn, fontSize: T.xs, padding: `${T.s2} ${T.s3}`, flex: 1, opacity: aiLoading ? 0.6 : 1 }}>
                {aiLoading ? "⏳ Writing..." : "📖 Generate Backstory"}
              </button>
              <button onClick={() => generateAI("recommendations")} disabled={aiLoading} style={{ ...S.secondaryBtn, fontSize: T.xs, padding: `${T.s2} ${T.s3}`, flex: 1, opacity: aiLoading ? 0.6 : 1 }}>
                {aiLoading ? "⏳ Thinking..." : "⚔️ Build Recommendations"}
              </button>
            </div>
            {aiOutput.recommendations && (
              <div style={{ background: "rgba(112,96,184,0.08)", border: `1px solid rgba(112,96,184,0.2)`, borderRadius: T.r1, padding: T.s3, marginBottom: T.s3 }}>
                <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.magicBright, marginBottom: T.s2 }}>BUILD ADVICE</div>
                <div style={{ fontFamily: T.body, fontSize: T.sm, color: T.textSecondary, lineHeight: 1.7, whiteSpace: "pre-wrap" }}>{aiOutput.recommendations}</div>
              </div>
            )}
          </div>
          <div style={{ marginBottom: T.s3 }}>
            <label style={S.label}>BACKSTORY</label>
            <textarea value={draft.backstory} onChange={e => up("backstory", e.target.value)}
              placeholder="Your character's history, motivations, and secrets..." rows={4}
              style={{ ...S.input, height: "auto", resize: "vertical", lineHeight: 1.7, fontFamily: T.body, fontSize: T.md }} />
          </div>
          <div style={{ display: "flex", gap: T.s3 }}>
            <button onClick={() => setStep(5)} style={S.secondaryBtn}>← Back</button>
            <button onClick={() => setStep(7)} style={S.primaryBtn}>Feats →</button>
          </div>
        </div>
      )}

      {step === 7 && (
        <div>
          <h2 style={{ fontFamily: T.display, color: T.gold, fontSize: 20, marginBottom: T.s2 }}>Feats</h2>
          <div style={{ display: "flex", gap: T.s2, marginBottom: T.s3, flexWrap: "wrap" }}>
            {featSources.map(s => <Rune key={s} active={featFilter === s} onClick={() => setFeatFilter(s)}>{s}</Rune>)}
          </div>
          {draft.feats.length > 0 && (
            <div style={{ marginBottom: T.s3, display: "flex", flexWrap: "wrap", gap: T.s2 }}>
              {draft.feats.map(f => (
                <span key={f} style={{ background: "rgba(180,130,60,0.12)", border: `1px solid ${T.border}`, borderRadius: T.r4, padding: `3px 10px`, fontFamily: T.heading, fontSize: T.xs, color: T.gold }}>
                  {f} <button onClick={() => up("feats", draft.feats.filter(ff => ff !== f))} style={{ background: "none", border: "none", color: T.hpLow, cursor: "pointer", fontFamily: T.heading, fontSize: T.xs, padding: 0, marginLeft: 4 }}>✕</button>
                </span>
              ))}
            </div>
          )}
          <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill,minmax(240px,1fr))", gap: T.s2, marginBottom: T.s4 }}>
            {DATA.feats.filter(f => featFilter === "All" || f.src === featFilter).filter(f => !draft.feats.includes(f.name)).map(f => (
              <div key={f.name} onClick={() => up("feats", [...draft.feats, f.name])}
                style={{ ...S.card, cursor: "pointer", padding: T.s3, transition: "all 0.12s" }}>
                <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 4 }}>
                  <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textPrimary, fontWeight: 700 }}>{f.name}</span>
                  <span style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted }}>{f.src}</span>
                </div>
                {f.pre && <div style={{ fontFamily: T.heading, fontSize: 9, color: "rgba(180,130,60,0.5)", marginBottom: 3 }}>Req: {f.pre}</div>}
                <div style={{ fontFamily: T.body, fontSize: T.xs, color: T.textSecondary, lineHeight: 1.5 }}>{f.desc}</div>
              </div>
            ))}
          </div>
          <div style={{ display: "flex", gap: T.s3 }}>
            <button onClick={() => setStep(6)} style={S.secondaryBtn}>← Back</button>
            <button onClick={() => setStep(8)} style={S.primaryBtn}>Finalize →</button>
          </div>
        </div>
      )}

      {step === 8 && (
        <div style={{ maxWidth: 560, margin: "0 auto", textAlign: "center", paddingTop: T.s4 }}>
          <div style={{ fontSize: 56, marginBottom: T.s4 }}>🏆</div>
          <h2 style={{ fontFamily: T.display, color: T.gold, fontSize: 22, marginBottom: T.s4 }}>{draft.name} is Ready</h2>
          <div style={{ ...S.card, textAlign: "left", marginBottom: T.s5 }}>
            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: T.s2 }}>
              {[["Name", draft.name], ["Race", draft.race], ["Class", draft.classes.map(c => `${c.name} ${c.level}`).join(" / ")], ["Subclass", draft.classes.map(c => c.subclass || "—").join(" / ")], ["Background", draft.background], ["Alignment", draft.alignment], ["Feats", draft.feats.join(", ") || "None"],
                ...Object.entries(draft.abilityScores).map(([k, v]) => [k.toUpperCase(), `${v} (${DnD.modStr(DnD.mod(v))})`])
              ].map(([k, v]) => (
                <div key={k} style={{ display: "flex", justifyContent: "space-between", padding: `${T.s1} ${T.s2}`, borderRadius: 4, background: "rgba(255,255,255,0.02)" }}>
                  <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textSecondary }}>{k}</span>
                  <span style={{ fontFamily: T.body, fontSize: T.sm, color: T.textPrimary, textAlign: "right", maxWidth: "55%" }}>{v}</span>
                </div>
              ))}
            </div>
          </div>
          <div style={{ display: "flex", gap: T.s3, justifyContent: "center" }}>
            <button onClick={() => setStep(7)} style={S.secondaryBtn}>← Edit</button>
            <button onClick={finalize} style={{ ...S.primaryBtn, fontSize: T.lg, padding: `${T.s4} ${T.s5}` }}>⚔️ Enter the World</button>
          </div>
        </div>
      )}
    </div>
  );
};

// ─── CHARACTER SHEET ───────────────────────────────────────
const CharSheet = ({ character, setCharacter }) => {
  const [tab, setTab] = useState("core");
  const [usedSlots, setUsedSlots] = useState({ 1: 0, 2: 0, 3: 0, 4: 0, 5: 0, 6: 0, 7: 0, 8: 0, 9: 0 });

  if (!character) return <div style={{ padding: T.s6, textAlign: "center", color: T.textSecondary, fontFamily: T.heading }}>Create a character first.</div>;

  const totalLevel = character.classes?.reduce((s, c) => s + c.level, 0) || character.level || 1;
  const pb = DnD.prof(totalLevel);
  const cls = DATA.classes.find(c => c.name === (character.classes?.[0]?.name || character.class));
  const as = character.abilityScores;
  const spellMod = cls?.name === "Wizard" ? DnD.mod(as.int) : ["Cleric", "Druid", "Ranger"].includes(cls?.name) ? DnD.mod(as.wis) : DnD.mod(as.cha);
  const spellSaveDC = 8 + pb + spellMod;
  const spellAtkBonus = pb + spellMod;
  const isSpellcaster = ["Bard", "Cleric", "Druid", "Paladin", "Ranger", "Sorcerer", "Warlock", "Wizard", "Artificer"].includes(cls?.name || "");
  const skillProf = new Set(character.skillProficiencies || []);
  const skillExp = new Set(character.skillExpertise || []);
  const getSkillMod = (s) => { const ab = DATA.skills.find(sk => sk.name === s)?.ab || "str"; return DnD.mod(as[ab]) + (skillExp.has(s) ? pb * 2 : skillProf.has(s) ? pb : 0); };
  const slots = SPELL_SLOTS[Math.min(20, totalLevel)] || SPELL_SLOTS[1];

  const TABS = [{ id: "core", l: "Core" }, { id: "combat", l: "Combat" }, { id: "spells", l: "Spells" }, { id: "skills", l: "Skills" }, { id: "gear", l: "Gear" }, { id: "features", l: "Features" }, { id: "notes", l: "Notes" }];

  return (
    <div style={{ height: "100%", display: "flex", flexDirection: "column", overflow: "hidden" }}>
      <div style={{ padding: T.s4, background: "rgba(6,4,8,0.98)", borderBottom: `1px solid ${T.border}`, flexShrink: 0 }}>
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", flexWrap: "wrap", gap: T.s3, marginBottom: T.s3 }}>
          <div>
            <h2 style={{ fontFamily: T.display, color: T.gold, fontSize: T.xxl, margin: 0 }}>{character.name}</h2>
            <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textSecondary, marginTop: 4, letterSpacing: "0.5px" }}>
              {character.race} · {character.classes ? character.classes.map(c => `${c.name} ${c.level}${c.subclass ? ` (${c.subclass})` : ""}`).join(" / ") : `${character.class} ${totalLevel}`} · {character.background} · {character.alignment}
            </div>
          </div>
          <div style={{ display: "flex", gap: T.s2, flexWrap: "wrap" }}>
            {[
              ["AC", 10 + DnD.mod(as.dex) + (character.equipment?.includes("Shield") ? 2 : 0)],
              ["Init", DnD.modStr(DnD.mod(as.dex))],
              ["Speed", `${DATA.races.find(r => r.name === character.race)?.spd || 30}ft`],
              ["Prof", `+${pb}`],
            ].map(([l, v]) => <StatBox key={l} label={l} value={v} />)}
          </div>
        </div>
        <div style={{ display: "flex", alignItems: "center", gap: T.s3 }}>
          <div style={{ flex: 1 }}><HPBar current={character.currentHp} max={character.hp} temp={character.tempHp} /></div>
          <button onClick={() => setCharacter(p => ({ ...p, currentHp: Math.max(0, p.currentHp - 1) }))} style={{ ...S.secondaryBtn, padding: `${T.s2} ${T.s3}`, fontSize: T.lg, color: T.hpLow, borderColor: "rgba(180,60,60,0.4)", minHeight: 44 }}>−</button>
          <button onClick={() => setCharacter(p => ({ ...p, currentHp: Math.min(p.hp, p.currentHp + 1) }))} style={{ ...S.primaryBtn, padding: `${T.s2} ${T.s3}`, fontSize: T.lg, minHeight: 44 }}>+</button>
        </div>
      </div>

      <div style={{ display: "flex", background: "rgba(6,4,8,0.97)", borderBottom: `1px solid ${T.border}`, flexShrink: 0, overflowX: "auto" }}>
        {TABS.map(t => (
          <button key={t.id} onClick={() => setTab(t.id)} style={{ padding: `${T.s3} ${T.s4}`, background: "transparent", border: "none", borderBottom: `2px solid ${tab === t.id ? T.gold : "transparent"}`, color: tab === t.id ? T.gold : T.textSecondary, cursor: "pointer", fontFamily: T.heading, fontSize: T.xs, letterSpacing: "0.5px", whiteSpace: "nowrap", minHeight: 44 }}>
            {t.l}
          </button>
        ))}
      </div>

      <div style={{ flex: 1, overflow: "auto", padding: T.s4 }}>
        {tab === "core" && (
          <div>
            <div style={{ display: "flex", gap: T.s2, flexWrap: "wrap", justifyContent: "center", marginBottom: T.s4 }}>
              {Object.entries(as).map(([ab, score]) => (
                <AbilityBox key={ab} label={ab.toUpperCase()} score={score} onChange={v => setCharacter(p => ({ ...p, abilityScores: { ...p.abilityScores, [ab]: v } }))} />
              ))}
            </div>
            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: T.s3, marginBottom: T.s3 }}>
              <div style={S.card}>
                <div style={{ ...S.label, marginBottom: T.s3 }}>SAVING THROWS</div>
                {Object.entries(as).map(([ab, score]) => {
                  const isp = cls?.saves?.includes(ab);
                  const mod = DnD.mod(score) + (isp ? pb : 0);
                  return <div key={ab} style={{ display: "flex", justifyContent: "space-between", padding: `3px 0`, borderBottom: `1px solid rgba(180,130,60,0.07)` }}>
                    <span style={{ fontFamily: T.body, fontSize: T.sm, color: isp ? T.textPrimary : T.textSecondary }}>{isp ? "◉" : "○"} {ab.charAt(0).toUpperCase() + ab.slice(1)}</span>
                    <span style={{ fontFamily: T.heading, fontSize: T.sm, color: isp ? T.goldBright : T.textSecondary }}>{DnD.modStr(mod)}</span>
                  </div>;
                })}
              </div>
              <div style={S.card}>
                <div style={{ ...S.label, marginBottom: T.s3 }}>SENSES & MISC</div>
                {[["Passive Perception", 10 + getSkillMod("Perception")], ["Passive Investigation", 10 + getSkillMod("Investigation")], ["Passive Insight", 10 + getSkillMod("Insight")]].map(([l, v]) => (
                  <div key={l} style={{ display: "flex", justifyContent: "space-between", padding: `3px 0`, borderBottom: `1px solid rgba(180,130,60,0.07)` }}>
                    <span style={{ fontFamily: T.body, fontSize: T.sm, color: T.textSecondary }}>{l}</span>
                    <span style={{ fontFamily: T.heading, fontSize: T.sm, color: T.goldBright }}>{v}</span>
                  </div>
                ))}
              </div>
            </div>
          </div>
        )}

        {tab === "combat" && (
          <div>
            <div style={{ ...S.card, marginBottom: T.s4 }}>
              <div style={{ ...S.label, marginBottom: T.s3 }}>ATTACKS & ACTIONS</div>
              <div style={{ display: "grid", gridTemplateColumns: "2fr 1fr 1fr 1fr", gap: T.s2, marginBottom: T.s2 }}>
                {["Weapon/Ability", "ATK Bonus", "Damage", "Type"].map(h => <span key={h} style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted }}>{h}</span>)}
              </div>
              {(character.attacks || [{ name: "Unarmed Strike", bonus: DnD.modStr(DnD.mod(as.str) + pb), dmg: `1+${DnD.mod(as.str)}`, type: "Bludgeoning" }]).map((a, i) => (
                <div key={i} style={{ display: "grid", gridTemplateColumns: "2fr 1fr 1fr 1fr", gap: T.s2, padding: `${T.s2} 0`, borderBottom: `1px solid rgba(180,130,60,0.07)` }}>
                  <span style={{ fontFamily: T.body, fontSize: T.md, color: T.textPrimary }}>{a.name}</span>
                  <span style={{ fontFamily: T.heading, fontSize: T.sm, color: "#70b060" }}>{a.bonus}</span>
                  <span style={{ fontFamily: T.heading, fontSize: T.sm, color: "#c07050" }}>{a.dmg}</span>
                  <span style={{ fontFamily: T.body, fontSize: T.sm, color: T.textSecondary }}>{a.type}</span>
                </div>
              ))}
            </div>
            <div style={{ ...S.card, marginBottom: T.s4 }}>
              <div style={{ ...S.label, marginBottom: T.s3 }}>CONDITIONS</div>
              <div style={{ display: "flex", flexWrap: "wrap", gap: T.s2 }}>
                {["Blinded", "Charmed", "Deafened", "Exhausted", "Frightened", "Grappled", "Incapacitated", "Invisible", "Paralyzed", "Petrified", "Poisoned", "Prone", "Restrained", "Stunned", "Unconscious"].map(cond => {
                  const active = (character.conditions || []).includes(cond);
                  return <button key={cond} onClick={() => setCharacter(p => ({ ...p, conditions: active ? (p.conditions || []).filter(c => c !== cond) : [...(p.conditions || []), cond] }))}
                    style={{ padding: `3px 10px`, borderRadius: T.r4, fontFamily: T.body, fontSize: T.xs, cursor: "pointer", border: `1px solid ${active ? "rgba(180,60,60,0.5)" : T.border}`, background: active ? "rgba(180,60,60,0.15)" : "transparent", color: active ? "#e08080" : T.textSecondary, minHeight: 32 }}>
                    {cond}
                  </button>;
                })}
              </div>
            </div>
          </div>
        )}

        {tab === "spells" && (
          <div>
            {!isSpellcaster ? <div style={{ color: T.textSecondary, textAlign: "center", padding: T.s6, fontFamily: T.body }}>
              {cls?.name || "This class"} doesn't cast spells. Some subclasses grant spellcasting.
            </div> : <div>
              <div style={{ display: "flex", gap: T.s3, marginBottom: T.s4, flexWrap: "wrap" }}>
                <StatBox label="SAVE DC" value={spellSaveDC} color={T.magicBright} />
                <StatBox label="SPELL ATK" value={`+${spellAtkBonus}`} color={T.magicBright} />
                <StatBox label="ABILITY" value={cls?.name === "Wizard" ? "INT" : ["Cleric", "Druid", "Ranger"].includes(cls?.name) ? "WIS" : "CHA"} color={T.magicBright} />
              </div>
              <div style={{ ...S.card, marginBottom: T.s4 }}>
                <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: T.s3 }}>
                  <div style={S.label}>SPELL SLOTS</div>
                  <button onClick={() => setUsedSlots({ 1: 0, 2: 0, 3: 0, 4: 0, 5: 0, 6: 0, 7: 0, 8: 0, 9: 0 })} style={{ ...S.ghostBtn, padding: `2px 8px`, fontSize: T.xs }}>Long Rest</button>
                </div>
                <div style={{ display: "flex", gap: T.s4, flexWrap: "wrap" }}>
                  {slots.map((max, i) => {
                    if (!max) return null;
                    const lvl = i + 1;
                    const used = usedSlots[lvl] || 0;
                    return <div key={lvl}>
                      <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textSecondary, marginBottom: T.s2, textAlign: "center" }}>LVL {lvl}</div>
                      <div style={{ display: "flex", gap: 4, justifyContent: "center", marginBottom: 4 }}>
                        {[...Array(max)].map((_, j) => (
                          <div key={j} onClick={() => setUsedSlots(p => ({ ...p, [lvl]: j < used ? Math.max(0, used - 1) : Math.min(max, used + 1) }))}
                            style={{ width: 16, height: 16, borderRadius: "50%", cursor: "pointer", background: j < used ? "transparent" : T.magic, border: `2px solid ${j < used ? "rgba(112,96,184,0.3)" : T.magic}`, transition: "all 0.2s" }} />
                        ))}
                      </div>
                      <div style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted, textAlign: "center" }}>{max - used}/{max}</div>
                    </div>;
                  })}
                </div>
              </div>
            </div>}
          </div>
        )}

        {tab === "skills" && (
          <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: T.s3 }}>
            <div style={S.card}>
              <div style={{ ...S.label, marginBottom: T.s3 }}>SKILLS</div>
              {DATA.skills.map(skill => {
                const isPb = skillProf.has(skill.name);
                const isExp = skillExp.has(skill.name);
                const mod = getSkillMod(skill.name);
                return <div key={skill.name} style={{ display: "flex", justifyContent: "space-between", alignItems: "center", padding: `3px 0`, borderBottom: `1px solid rgba(180,130,60,0.05)` }}>
                  <div style={{ display: "flex", alignItems: "center", gap: T.s2 }}>
                    <span onClick={() => setCharacter(p => {
                      const sp = new Set(p.skillProficiencies || []); const se = new Set(p.skillExpertise || []);
                      if (se.has(skill.name)) { se.delete(skill.name); sp.delete(skill.name); } else if (sp.has(skill.name)) { se.add(skill.name); } else { sp.add(skill.name); }
                      return { ...p, skillProficiencies: [...sp], skillExpertise: [...se] };
                    })} style={{ color: isExp ? "#ffd700" : isPb ? T.gold : T.textMuted, cursor: "pointer", fontSize: 12, lineHeight: 1 }}>
                      {isExp ? "◉◉" : isPb ? "◉" : "○"}
                    </span>
                    <span style={{ fontFamily: T.body, fontSize: T.sm, color: isPb ? T.textPrimary : T.textSecondary }}>{skill.name}</span>
                    <span style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted }}>({skill.ab.toUpperCase()})</span>
                  </div>
                  <span style={{ fontFamily: T.heading, fontSize: T.sm, color: isPb ? T.goldBright : T.textSecondary }}>{DnD.modStr(mod)}</span>
                </div>;
              })}
            </div>
            <div style={S.card}>
              <div style={S.label}>LANGUAGES</div>
              <div style={{ display: "flex", flexWrap: "wrap", gap: T.s1, marginTop: T.s3 }}>
                {["Common", "Dwarvish", "Elvish", "Giant", "Gnomish", "Goblin", "Halfling", "Orc", "Abyssal", "Celestial", "Draconic", "Deep Speech", "Infernal", "Primordial", "Sylvan", "Undercommon"].map(lang => {
                  const has = (character.languages || ["Common"]).includes(lang);
                  return <button key={lang} onClick={() => setCharacter(p => ({ ...p, languages: has ? (p.languages || ["Common"]).filter(l => l !== lang) : [...(p.languages || ["Common"]), lang] }))}
                    style={{ padding: `2px 8px`, borderRadius: T.r4, fontFamily: T.body, fontSize: T.xs, cursor: "pointer", background: has ? "rgba(180,130,60,0.12)" : "transparent", border: `1px solid ${has ? T.borderHot : T.border}`, color: has ? T.goldBright : T.textSecondary, minHeight: 28 }}>
                    {lang}
                  </button>;
                })}
              </div>
            </div>
          </div>
        )}

        {tab === "gear" && (
          <div>
            <div style={{ ...S.card, marginBottom: T.s4 }}>
              <div style={S.label}>CURRENCY</div>
              <div style={{ display: "flex", gap: T.s3, flexWrap: "wrap", marginTop: T.s3 }}>
                {[["CP", "#c08030"], ["SP", "#c0c0c0"], ["EP", "#a0b8a0"], ["GP", "#ffd700"], ["PP", "#d0d0e0"]].map(([c, col]) => (
                  <div key={c}><div style={{ fontFamily: T.heading, fontSize: T.xs, color: col, textAlign: "center", marginBottom: 4 }}>{c}</div>
                    <input type="number" min={0} value={character[`c_${c}`] || 0} onChange={e => setCharacter(p => ({ ...p, [`c_${c}`]: Number(e.target.value) }))}
                      style={{ width: 72, ...S.input, textAlign: "center", padding: `${T.s2} ${T.s1}`, fontSize: T.md, color: col }} /></div>
                ))}
              </div>
            </div>
            {Object.entries({ ...DATA.weapons, ...DATA.armor }).map(([cat, items]) => (
              <div key={cat} style={{ marginBottom: T.s3 }}>
                <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, letterSpacing: "1px", marginBottom: T.s2 }}>{cat.toUpperCase()}</div>
                <div style={{ display: "flex", flexWrap: "wrap", gap: T.s1 }}>
                  {items.map(item => {
                    const short = item.split(" (")[0];
                    const has = (character.equipment || []).includes(short);
                    return <button key={item} onClick={() => setCharacter(p => ({ ...p, equipment: has ? (p.equipment || []).filter(e => e !== short) : [...(p.equipment || []), short] }))}
                      style={{ padding: `3px 9px`, borderRadius: T.r1, fontFamily: T.body, fontSize: T.xs, cursor: "pointer", background: has ? "rgba(180,130,60,0.1)" : "transparent", border: `1px solid ${has ? T.borderHot : T.border}`, color: has ? T.goldBright : T.textSecondary, minHeight: 30 }}>
                      {short}
                    </button>;
                  })}
                </div>
              </div>
            ))}
          </div>
        )}

        {tab === "features" && (
          <div>
            <div style={{ ...S.card, marginBottom: T.s4 }}>
              <div style={{ ...S.label, marginBottom: T.s3 }}>FEATS</div>
              <div style={{ display: "flex", flexWrap: "wrap", gap: T.s2, marginBottom: T.s3 }}>
                {(character.feats || []).map(f => (
                  <span key={f} style={{ background: "rgba(180,130,60,0.1)", border: `1px solid ${T.border}`, borderRadius: T.r4, padding: `3px 10px`, fontFamily: T.heading, fontSize: T.xs, color: T.gold }}>
                    {f} <button onClick={() => setCharacter(p => ({ ...p, feats: (p.feats || []).filter(ff => ff !== f) }))} style={{ background: "none", border: "none", color: T.hpLow, cursor: "pointer", fontFamily: T.heading, fontSize: T.xs, padding: 0, marginLeft: 4 }}>✕</button>
                  </span>
                ))}
              </div>
              <select onChange={e => { if (e.target.value) setCharacter(p => ({ ...p, feats: [...(p.feats || []), e.target.value] })); e.target.value = ""; }}
                style={{ ...S.input, fontFamily: T.heading }}>
                <option value="" style={{ background: "#150d04" }}>+ Add Feat</option>
                {DATA.feats.filter(f => !(character.feats || []).includes(f.name)).map(f => <option key={f.name} value={f.name} style={{ background: "#150d04" }}>{f.name} ({f.src})</option>)}
              </select>
            </div>
            <div style={S.card}>
              <div style={S.label}>RACIAL TRAITS</div>
              <div style={{ display: "flex", flexWrap: "wrap", gap: T.s2, marginTop: T.s3 }}>
                {DATA.races.find(r => r.name === character.race)?.traits.map(t => (
                  <span key={t} style={{ background: "rgba(80,140,60,0.1)", color: "#80b860", border: "1px solid rgba(80,140,60,0.2)", borderRadius: 4, padding: `2px 8px`, fontFamily: T.body, fontSize: T.xs }}>{t}</span>
                ))}
              </div>
            </div>
          </div>
        )}

        {tab === "notes" && (
          <div style={{ display: "flex", flexDirection: "column", gap: T.s4 }}>
            {[["Backstory", "backstory", 5], ["Adventure Notes", "notes", 4], ["NPC Relationships", "npcNotes", 3], ["Treasure & Loot", "treasure", 3]].map(([l, k, rows]) => (
              <div key={k}>
                <label style={S.label}>{l.toUpperCase()}</label>
                <textarea value={character[k] || ""} onChange={e => setCharacter(p => ({ ...p, [k]: e.target.value }))} placeholder={l + "..."} rows={rows}
                  style={{ ...S.input, height: "auto", resize: "vertical", fontFamily: T.body, fontSize: T.md, lineHeight: 1.7 }} />
              </div>
            ))}
          </div>
        )}
      </div>
    </div>
  );
};

// ─── COMPENDIUM ────────────────────────────────────────────
const Compendium = ({ character, setCharacter }) => {
  const [tab, setTab] = useState("spells");
  const [search, setSearch] = useState("");
  const sl = search.toLowerCase();
  const TABS = ["spells", "weapons", "armor", "races", "classes", "feats", "backgrounds"];

  return (
    <div style={{ height: "100%", display: "flex", flexDirection: "column" }}>
      <div style={{ padding: `${T.s3} ${T.s4}`, borderBottom: `1px solid ${T.border}`, flexShrink: 0 }}>
        <input value={search} onChange={e => setSearch(e.target.value)} placeholder="Search compendium..."
          style={{ ...S.input, marginBottom: T.s3 }} />
        <div style={{ display: "flex", gap: T.s2, flexWrap: "wrap" }}>
          {TABS.map(t => <button key={t} onClick={() => setTab(t)} style={{ padding: `${T.s2} ${T.s3}`, borderRadius: T.r4, fontFamily: T.heading, fontSize: T.xs, cursor: "pointer", background: tab === t ? "linear-gradient(135deg,#8a5c1a,#5c3a0a)" : "transparent", border: `1px solid ${tab === t ? "transparent" : T.border}`, color: tab === t ? T.goldBright : T.textSecondary, minHeight: 32 }}>{t.charAt(0).toUpperCase() + t.slice(1)}</button>)}
        </div>
      </div>
      <div style={{ flex: 1, overflow: "auto", padding: T.s4 }}>
        {tab === "spells" && <div>
          {Object.entries(DATA.spells).map(([lvl, list]) => (
            <div key={lvl} style={{ marginBottom: T.s5 }}>
              <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, letterSpacing: "1px", marginBottom: T.s3, borderBottom: `1px solid ${T.border}`, paddingBottom: T.s2 }}>{lvl === "C" ? "CANTRIPS" : `LEVEL ${lvl} SPELLS`}</div>
              <div style={{ display: "flex", flexWrap: "wrap", gap: T.s1 }}>
                {list.filter(s => s.toLowerCase().includes(sl)).map(s => {
                  const known = character && (character.spells || []).includes(s);
                  return <span key={s} onClick={() => character && setCharacter(p => ({ ...p, spells: known ? (p.spells || []).filter(sp => sp !== s) : [...(p.spells || []), s] }))}
                    style={{ background: known ? "rgba(112,96,184,0.15)" : "rgba(180,130,60,0.06)", color: known ? T.magicBright : T.textSecondary, border: `1px solid ${known ? "rgba(112,96,184,0.3)" : T.border}`, borderRadius: 5, padding: `3px 9px`, fontFamily: T.body, fontSize: T.xs, cursor: character ? "pointer" : "default", transition: "all 0.15s" }}>
                    {s}{known ? " ✓" : ""}
                  </span>;
                })}
              </div>
            </div>
          ))}
        </div>}
        {tab === "weapons" && <div>{Object.entries(DATA.weapons).map(([cat, items]) => (
          <div key={cat} style={{ marginBottom: T.s4 }}>
            <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, letterSpacing: "1px", marginBottom: T.s3, borderBottom: `1px solid ${T.border}`, paddingBottom: T.s2 }}>{cat.toUpperCase()}</div>
            <div style={{ display: "flex", flexWrap: "wrap", gap: T.s2 }}>{items.filter(i => i.toLowerCase().includes(sl)).map(i => <div key={i} style={{ ...S.card, padding: `${T.s2} ${T.s3}`, fontSize: T.xs, fontFamily: T.body, color: T.textSecondary }}>{i}</div>)}</div>
          </div>
        ))}</div>}
        {tab === "armor" && <div>{Object.entries(DATA.armor).map(([cat, items]) => (
          <div key={cat} style={{ marginBottom: T.s4 }}>
            <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, letterSpacing: "1px", marginBottom: T.s3, borderBottom: `1px solid ${T.border}`, paddingBottom: T.s2 }}>{cat.toUpperCase()}</div>
            <div style={{ display: "flex", flexWrap: "wrap", gap: T.s2 }}>{items.filter(i => i.toLowerCase().includes(sl)).map(i => <div key={i} style={{ ...S.card, padding: `${T.s2} ${T.s3}`, fontSize: T.xs, fontFamily: T.body, color: T.textSecondary }}>{i}</div>)}</div>
          </div>
        ))}</div>}
        {tab === "races" && <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill,minmax(200px,1fr))", gap: T.s3 }}>
          {DATA.races.filter(r => r.name.toLowerCase().includes(sl)).map(r => (
            <div key={r.name} style={S.card}>
              <div style={{ display: "flex", justifyContent: "space-between", marginBottom: T.s2 }}><span style={{ fontFamily: T.heading, fontSize: T.sm, color: T.textPrimary, fontWeight: 700 }}>{r.name}</span><span style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted }}>{r.source}</span></div>
              <div style={{ display: "flex", flexWrap: "wrap", gap: 3, marginBottom: T.s2 }}>{Object.entries(r.ab).map(([a, v]) => <span key={a} style={{ fontFamily: T.heading, fontSize: 9, color: "#70a060", background: "rgba(80,160,60,0.1)", padding: "1px 5px", borderRadius: 3 }}>+{v} {a.toUpperCase()}</span>)}</div>
              <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, marginBottom: T.s2 }}>Spd {r.spd}ft · {r.sz}</div>
              {r.traits.map(t => <div key={t} style={{ fontFamily: T.body, fontSize: T.xs, color: T.textSecondary }}>• {t}</div>)}
            </div>
          ))}
        </div>}
        {tab === "classes" && <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill,minmax(240px,1fr))", gap: T.s3 }}>
          {DATA.classes.filter(c => c.name.toLowerCase().includes(sl)).map(c => (
            <div key={c.name} style={S.card}>
              <div style={{ display: "flex", justifyContent: "space-between", marginBottom: T.s2 }}><span style={{ fontFamily: T.heading, fontSize: T.sm, color: T.textPrimary, fontWeight: 700 }}>{c.name}</span><span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.gold, background: "rgba(180,130,60,0.1)", padding: "1px 7px", borderRadius: 3 }}>d{c.hd}</span></div>
              <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textSecondary, marginBottom: T.s2 }}>{c.pri} · {c.saves.map(s => s.toUpperCase()).join(", ")}</div>
              <div style={{ display: "flex", flexWrap: "wrap", gap: 3 }}>{c.subclasses.slice(0, 6).map(s => <span key={s} style={{ fontFamily: T.body, fontSize: 9, color: T.textMuted, background: "rgba(255,255,255,0.03)", padding: "1px 5px", borderRadius: 3 }}>{s}</span>)}{c.subclasses.length > 6 && <span style={{ fontFamily: T.body, fontSize: 9, color: T.textMuted }}>+{c.subclasses.length - 6} more</span>}</div>
            </div>
          ))}
        </div>}
        {tab === "feats" && <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill,minmax(240px,1fr))", gap: T.s3 }}>
          {DATA.feats.filter(f => f.name.toLowerCase().includes(sl) || f.desc.toLowerCase().includes(sl)).map(f => (
            <div key={f.name} style={S.card}>
              <div style={{ display: "flex", justifyContent: "space-between", marginBottom: T.s2 }}><span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textPrimary, fontWeight: 700 }}>{f.name}</span><span style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted }}>{f.src}</span></div>
              {f.pre && <div style={{ fontFamily: T.heading, fontSize: 9, color: "rgba(180,130,60,0.5)", marginBottom: 4 }}>Req: {f.pre}</div>}
              <div style={{ fontFamily: T.body, fontSize: T.xs, color: T.textSecondary, lineHeight: 1.5 }}>{f.desc}</div>
            </div>
          ))}
        </div>}
        {tab === "backgrounds" && <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill,minmax(200px,1fr))", gap: T.s3 }}>
          {DATA.backgrounds.filter(b => b.name.toLowerCase().includes(sl)).map(b => (
            <div key={b.name} style={S.card}>
              <div style={{ display: "flex", justifyContent: "space-between", marginBottom: T.s2 }}><span style={{ fontFamily: T.heading, fontSize: T.sm, color: T.textPrimary, fontWeight: 700 }}>{b.name}</span><span style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted }}>{b.src}</span></div>
              <div style={{ fontFamily: T.body, fontSize: T.xs, color: T.textSecondary, marginBottom: 4 }}>Skills: {b.skills.join(", ")}</div>
              <div style={{ fontFamily: T.body, fontSize: T.xs, color: T.textMuted }}>Feature: {b.feature}</div>
            </div>
          ))}
        </div>}
      </div>
    </div>
  );
};

// ─── STORY ENGINE ──────────────────────────────────────────
const StoryEngine = ({ character, worldState, dispatch, selectedSetting }) => {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState("");
  const [isLoading, setIsLoading] = useState(false);
  const [showDice, setShowDice] = useState(false);
  const endRef = useRef(null);
  const inputRef = useRef(null);

  useEffect(() => { endRef.current?.scrollIntoView({ behavior: "smooth" }); }, [messages]);

  const parseAndDispatch = (text) => {
    try {
      const match = text.match(/```state\n([\s\S]*?)```/);
      if (!match) return;
      const state = JSON.parse(match[1]);
      if (state.hpDelta && state.hpDelta !== 0) dispatch({ type: "UPDATE_CHAR_HP", delta: state.hpDelta });
      if (state.locationChange) dispatch({ type: "UPDATE_LOCATION", location: state.locationChange });
      if (state.worldFact) dispatch({ type: "ADD_WORLD_FACT", fact: state.worldFact });
      if (state.npcsMet?.length) state.npcsMet.forEach(n => dispatch({ type: "ADD_NPC", npc: { name: n, relation: "Met", notes: "Encountered in story", alive: true } }));
      if (state.phaseChange) dispatch({ type: "SET_PHASE", phase: state.phaseChange });
      if (state.questUpdate) dispatch({ type: "ADD_QUEST", quest: { title: state.questUpdate.title, desc: state.questUpdate.desc, status: "active", objectives: [] } });
      if (state.combatStart && state.combatStart.length) {
        const playerInit = { name: worldState.character?.name || "Player", roll: DnD.roll(1, 20, 2), hp: worldState.character?.hp?.current || 20, maxHp: worldState.character?.hp?.max || 20, ac: 14, isPlayer: true, conditions: [] };
        const enemies = state.combatStart.map(e => ({ ...e, roll: DnD.roll(1, 20, 1), conditions: [] }));
        const initiative = [...enemies, playerInit].sort((a, b) => b.roll - a.roll);
        dispatch({ type: "START_COMBAT", initiative });
      }
    } catch (e) { /* silently ignore malformed state JSON */ }
  };

  const sendMessage = async (msg, isSystem = false) => {
    if (!msg?.trim() && !isSystem) return;
    setIsLoading(true);
    if (!isSystem) {
      setMessages(p => [...p, { role: "user", content: msg, id: Date.now() }]);
      setInput("");
    }

    const systemPrompt = buildSystemPrompt(worldState, selectedSetting);
    const historyMsgs = messages.slice(-16).map(m => ({
      role: m.role === "dm" ? "assistant" : "user",
      content: m.role === "system" ? `[System: ${m.content}]` : m.content,
    })).filter(m => m.role === "user" || m.role === "assistant");

    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          model: "claude-sonnet-4-20250514",
          max_tokens: 1200,
          system: systemPrompt,
          messages: [...historyMsgs, { role: "user", content: msg }],
        }),
      });
      const data = await res.json();
      const text = data.content?.map(b => b.text || "").join("") || "*The world holds its breath...*";

      dispatch({ type: "UPDATE_STORY_CONTEXT", context: `${worldState.session.location}: ${msg?.slice(0, 60) || "Story began"}` });
      parseAndDispatch(text);

      setMessages(p => [...p, { role: "dm", content: text, id: Date.now() + 1, isNew: true }]);
    } catch (e) {
      setMessages(p => [...p, { role: "dm", content: "*The arcane connection wavers... Try again.*", id: Date.now() + 1, isNew: true }]);
    }
    setIsLoading(false);
  };

  const startAdventure = () => {
    const hooks = [
      "A dying courier collapses at the party's feet, pressing a wax-sealed letter into trembling hands before going still.",
      "The village elder's urgent summons brings you here — three children vanished from locked rooms last night, leaving only fresh mud and claw marks.",
      "The map was sold to you for a pittance by a terrified merchant who said keeping it any longer would kill him.",
      "You wake in a cart, hands unbound, moving through an unfamiliar forest. Whoever left you here wanted you to find something.",
      "The job seemed simple: escort a merchant's chest to the city. You're three hours out when you realize the chest is breathing.",
      "Someone has been leaving you notes. They know things they shouldn't. The latest one says: 'Tonight. The old mill. Come alone or they all die.'",
    ];
    const hook = hooks[Math.floor(Math.random() * hooks.length)];
    const charDesc = character ? `${character.name}, a ${character.race} ${character.classes ? character.classes.map(c => `${c.name} ${c.level}`).join("/") : character.class}` : "a wandering adventurer of unknown origin";

    const openingPrompt = `[GAME START] Setting: ${selectedSetting.name}. Opening hook: "${hook}". The player character is ${charDesc}.${character?.backstory ? ` Backstory: ${character.backstory.slice(0, 200)}.` : ""}

Begin the adventure with a cinematic opening scene that: (1) establishes vivid atmosphere and setting in 2 paragraphs, (2) introduces the inciting hook naturally — don't just announce it, dramatize it, (3) ends with the character at a clear decision point. Include 4 specific action choices. Use the full state JSON block.`;

    sendMessage(openingPrompt, true);
  };

  const handleDiceResult = useCallback(({ die, raw, total, crit, fail }) => {
    setMessages(p => [...p, { role: "system", content: `🎲 d${die}: **${total}**${crit ? " ★" : fail ? " ☠" : ""}`, id: Date.now() }]);
    if (messages.length > 0) sendMessage(`[DICE RESULT: ${total} on d${die}${crit ? " — Natural 20! Critical!" : fail ? " — Natural 1! Critical failure!" : ""}]`, true);
  }, [messages]);

  const QUICK_ACTIONS = [
    "I look around carefully, taking in all the details.",
    "I speak to anyone nearby.",
    "I check my surroundings for threats or exits.",
    "I examine the most interesting thing here.",
    "I make camp and rest.",
    "I ask about local rumors.",
  ];

  if (messages.length === 0) return (
    <div style={{ flex: 1, overflow: "auto", padding: T.s5, display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center", textAlign: "center" }}>
      <div style={{ fontSize: 64, marginBottom: T.s5 }}>🐉</div>
      <h1 style={{ fontFamily: T.display, color: T.gold, fontSize: 28, margin: `0 0 ${T.s3}`, lineHeight: 1.2 }}>Begin Your Adventure</h1>
      <p style={{ color: T.textSecondary, fontFamily: T.body, fontSize: T.lg, maxWidth: 480, lineHeight: 1.8, marginBottom: T.s5 }}>
        The AI Dungeon Master will guide your story — crafting NPCs, resolving all dice mechanics automatically, and never blocking your choices. You hold agency. The world reacts.
      </p>
      <div style={{ width: "100%", maxWidth: 700, marginBottom: T.s5 }}>
        <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textSecondary, letterSpacing: "1px", marginBottom: T.s3 }}>CHOOSE YOUR WORLD</div>
        <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill,minmax(200px,1fr))", gap: T.s3, textAlign: "left" }}>
          {DATA.settings.map(s => (
            <div key={s.name} onClick={() => dispatch({ type: "SET_SETTING", setting: s })}
              style={{ ...S.card, cursor: "pointer", border: `1px solid ${selectedSetting?.name === s.name ? T.borderHot : T.border}`, background: selectedSetting?.name === s.name ? "rgba(180,130,60,0.1)" : T.surface, transition: "all 0.15s" }}>
              <div style={{ display: "flex", alignItems: "center", gap: T.s2, marginBottom: T.s2 }}>
                <span style={{ fontSize: 20 }}>{s.icon}</span>
                <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textPrimary, fontWeight: 700 }}>{s.name}</span>
              </div>
              <p style={{ fontFamily: T.body, fontSize: T.xs, color: T.textSecondary, margin: 0, lineHeight: 1.5 }}>{s.desc}</p>
              <div style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted, marginTop: T.s2 }}>Tone: {s.tone}</div>
            </div>
          ))}
        </div>
      </div>
      {selectedSetting && (
        <button onClick={startAdventure} style={{ ...S.primaryBtn, fontSize: T.lg, padding: `${T.s4} ${T.s6}`, letterSpacing: "1px" }}>
          ⚔️ Begin Adventure in {selectedSetting.name}
        </button>
      )}
      {!character && selectedSetting && (
        <p style={{ color: T.textMuted, fontFamily: T.body, fontSize: T.sm, marginTop: T.s3 }}>💡 Create a character first for a personalized adventure, or adventure as a mysterious wanderer.</p>
      )}
    </div>
  );

  return (
    <div style={{ display: "flex", flexDirection: "column", height: "100%" }}>
      {worldState.combat.active && (
        <div style={{ padding: `${T.s2} ${T.s4}`, flexShrink: 0 }}>
          <CombatTracker combat={worldState.combat} dispatch={dispatch} characterName={worldState.character?.name} />
        </div>
      )}
      <div style={{ flex: 1, overflow: "auto", padding: T.s4 }}>
        {messages.map((msg) => (
          msg.role === "system"
            ? <div key={msg.id} style={{ textAlign: "center", margin: `${T.s2} auto ${T.s2}`, display: "flex", justifyContent: "center" }}>
              <div style={{ padding: `3px ${T.s4}`, background: "rgba(180,130,60,0.06)", border: `1px solid ${T.border}`, borderRadius: T.r4, fontFamily: T.heading, fontSize: T.xs, color: T.textMuted }}
                dangerouslySetInnerHTML={{ __html: msg.content.replace(/\*\*(.*?)\*\*/g, `<strong style="color:${T.gold}">$1</strong>`) }} />
            </div>
            : <StoryMessage key={msg.id} msg={msg} isNew={!!msg.isNew} />
        ))}
        {isLoading && (
          <div style={{ display: "flex", gap: T.s3, marginBottom: T.s5 }}>
            <div style={{ width: 32, height: 32, borderRadius: "50%", flexShrink: 0, background: "linear-gradient(135deg,#6a3a0a,#3a1a00)", border: `1px solid ${T.border}`, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 14 }}>🎲</div>
            <div style={{ ...S.card, padding: `${T.s3} ${T.s4}`, display: "flex", gap: T.s2, alignItems: "center" }}>
              {[0, 1, 2].map(i => <div key={i} style={{ width: 7, height: 7, borderRadius: "50%", background: T.gold, animation: "pulse 1.4s ease-in-out infinite", animationDelay: `${i * 0.2}s` }} />)}
            </div>
          </div>
        )}
        <div ref={endRef} />
      </div>

      <div style={{ padding: `${T.s2} ${T.s4}`, borderTop: `1px solid rgba(180,130,60,0.1)`, display: "flex", gap: T.s2, flexWrap: "nowrap", overflowX: "auto", flexShrink: 0 }}>
        {QUICK_ACTIONS.map(a => (
          <button key={a} onClick={() => sendMessage(a)}
            style={{ padding: `${T.s1} ${T.s3}`, background: "rgba(180,130,60,0.06)", border: `1px solid ${T.border}`, borderRadius: T.r4, fontFamily: T.body, fontSize: T.xs, color: T.textSecondary, cursor: "pointer", whiteSpace: "nowrap", minHeight: 32, flexShrink: 0 }}>
            {a.split(" ").slice(0, 4).join(" ")}…
          </button>
        ))}
      </div>

      {showDice && (
        <div style={{ padding: T.s4, borderTop: `1px solid ${T.border}`, background: "rgba(8,5,10,0.98)", flexShrink: 0 }}>
          <DiceRoller onRoll={handleDiceResult} />
        </div>
      )}

      <div style={{ padding: `${T.s3} ${T.s4}`, borderTop: `1px solid ${T.border}`, display: "flex", gap: T.s2, alignItems: "flex-end", background: "rgba(6,4,8,0.98)", flexShrink: 0 }}>
        <button onClick={() => setShowDice(p => !p)} style={{ ...S.secondaryBtn, padding: `${T.s2} ${T.s3}`, minHeight: 44, flexShrink: 0, background: showDice ? "rgba(180,130,60,0.15)" : "transparent" }}>🎲</button>
        <textarea ref={inputRef} value={input} onChange={e => setInput(e.target.value)}
          onKeyDown={e => { if (e.key === "Enter" && !e.shiftKey) { e.preventDefault(); if (input.trim() && !isLoading) sendMessage(input); } }}
          placeholder="What do you do? (Enter to send, Shift+Enter for new line)"
          rows={2}
          style={{ flex: 1, ...S.input, height: "auto", resize: "none", lineHeight: 1.5, fontFamily: T.body, fontSize: T.md, padding: T.s3 }} />
        <button onClick={() => { if (input.trim() && !isLoading) sendMessage(input); }} disabled={isLoading || !input.trim()}
          style={{ ...S.primaryBtn, padding: `${T.s3} ${T.s4}`, opacity: isLoading || !input.trim() ? 0.5 : 1, flexShrink: 0, minHeight: 44 }}>
          Send
        </button>
      </div>
    </div>
  );
};

// ─── SPLASH SCREEN ─────────────────────────────────────────
const SplashScreen = ({ onEnter }) => {
  const [visible, setVisible] = useState(false);
  useEffect(() => { setTimeout(() => setVisible(true), 100); }, []);
  return (
    <div style={{ position: "fixed", inset: 0, background: "#03020a", display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center", zIndex: 1000, transition: "opacity 0.4s", opacity: visible ? 1 : 0 }}>
      <div style={{ position: "absolute", inset: 0, overflow: "hidden", pointerEvents: "none" }}>
        {["✦", "⚔", "✦", "🐉", "✦", "⚔", "✦"].map((s, i) => (
          <div key={i} style={{ position: "absolute", left: `${10 + i * 13}%`, top: `${15 + Math.sin(i) * 25}%`, fontSize: i % 2 === 0 ? 12 : 24, color: "rgba(180,130,60,0.08)", animation: `float ${3 + i * 0.5}s ease-in-out infinite alternate`, animationDelay: `${i * 0.3}s` }}>{s}</div>
        ))}
      </div>
      <div style={{ textAlign: "center", padding: T.s5 }}>
        <div style={{ fontSize: 80, marginBottom: T.s4 }}>🐉</div>
        <div style={{ fontFamily: T.display, fontSize: 28, color: T.gold, marginBottom: T.s3, lineHeight: 1.2, letterSpacing: "2px" }}>AI DUNGEON MASTER</div>
        <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, letterSpacing: "3px", marginBottom: T.s6 }}>D&D 5e · 2014–2024 · Collaborative Story Engine</div>
        <div style={{ display: "flex", flexWrap: "wrap", gap: T.s2, justifyContent: "center", marginBottom: T.s6 }}>
          {["Full 5e Rules Engine", "AI Narrative DM", "Battle Map", "60+ Races", "13+ Classes", "Multiclassing", "Spell Slots", "Combat Tracker"].map(f => (
            <span key={f} style={{ fontFamily: T.heading, fontSize: 9, color: T.textMuted, background: "rgba(180,130,60,0.07)", border: `1px solid rgba(180,130,60,0.15)`, borderRadius: T.r4, padding: `3px 10px`, letterSpacing: "0.5px" }}>{f}</span>
          ))}
        </div>
        <button onClick={onEnter} style={{ ...S.primaryBtn, fontSize: T.lg, padding: `${T.s4} ${T.s6}`, letterSpacing: "2px", boxShadow: "0 0 40px rgba(180,130,60,0.2)" }}>
          ENTER THE WORLD
        </button>
        <div style={{ fontFamily: T.heading, fontSize: T.xs, color: T.textMuted, marginTop: T.s4, opacity: 0.5 }}>Requires Anthropic API — Provided by claude.ai</div>
      </div>
      <style>{`@keyframes float{0%{transform:translateY(0) rotate(0)}100%{transform:translateY(-10px) rotate(5deg)}}`}</style>
    </div>
  );
};

// ─── ROOT APP ──────────────────────────────────────────────
export default function App() {
  const [splash, setSplash] = useState(true);
  const [tab, setTab] = useState("story");
  const [charView, setCharView] = useState("create");
  const [character, setCharacter] = useState(null);
  const [selectedSetting, setSelectedSetting] = useState(null);
  const [worldState, dispatch] = useReducer(gameReducer, createWorldState());

  const fullDispatch = useCallback((action) => {
    if (action.type === "SET_SETTING") { setSelectedSetting(action.setting); return; }
    dispatch(action);
  }, []);

  const fullWorldState = {
    ...worldState,
    character: character ? {
      name: character.name,
      race: character.race,
      class: character.classes?.map(c => `${c.name} ${c.level}`).join("/") || character.class,
      level: character.level || 1,
      hp: { current: character.currentHp || character.hp, max: character.hp },
      abilityScores: character.abilityScores,
      conditions: character.conditions || [],
      proficiencyBonus: DnD.prof(character.level || 1),
      equipment: character.equipment || [],
      spells: character.spells || [],
      feats: character.feats || [],
      backstory: character.backstory || "",
    } : null,
  };

  const NAV = [
    { id: "story", icon: "📖", label: "Story" },
    { id: "character", icon: "⚔️", label: character ? "Character" : "Create" },
    { id: "map", icon: "🗺️", label: "Battle Map" },
    { id: "compendium", icon: "📚", label: "Codex" },
  ];

  if (splash) return <SplashScreen onEnter={() => setSplash(false)} />;

  return (
    <div style={{
      height: "100vh", height: "100dvh", display: "flex", flexDirection: "column", background: T.bg, overflow: "hidden",
      backgroundImage: `radial-gradient(ellipse at 15% 85%, rgba(100,50,10,0.2) 0%, transparent 55%), radial-gradient(ellipse at 85% 15%, rgba(60,20,60,0.15) 0%, transparent 55%)`
    }}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;500;600;700&family=Cinzel+Decorative:wght@400;700&family=Crimson+Text:ital,wght@0,400;0,600;1,400&display=swap');
        *{box-sizing:border-box;scrollbar-width:thin;scrollbar-color:rgba(180,130,60,0.25) transparent;-webkit-tap-highlight-color:transparent;}
        *::-webkit-scrollbar{width:3px;height:3px;}
        *::-webkit-scrollbar-thumb{background:rgba(180,130,60,0.25);border-radius:2px;}
        button:active{filter:brightness(0.9);transform:scale(0.98);}
        input,textarea,select{outline:none;appearance:none;-webkit-appearance:none;}
        textarea{font-size:16px;}
        @keyframes pulse{0%,100%{opacity:0.2;transform:scale(0.7)}50%{opacity:1;transform:scale(1)}}
        @keyframes blink{0%,100%{opacity:1}50%{opacity:0}}
      `}</style>

      <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", padding: `0 ${T.s4}`, height: 48, background: "rgba(4,2,8,0.98)", borderBottom: `1px solid ${T.border}`, flexShrink: 0 }}>
        <div style={{ display: "flex", alignItems: "center", gap: T.s3 }}>
          <span style={{ fontSize: 18 }}>🐉</span>
          <div>
            <div style={{ fontFamily: T.display, color: T.gold, fontSize: 12, letterSpacing: "1px" }}>AI Dungeon Master</div>
            <div style={{ fontFamily: T.heading, fontSize: 8, color: T.textMuted, letterSpacing: "1.5px" }}>5e · COLLABORATIVE STORY ENGINE</div>
          </div>
        </div>
        <div style={{ display: "flex", alignItems: "center", gap: T.s2 }}>
          {character && (
            <div style={{ display: "flex", alignItems: "center", gap: T.s2, padding: `3px ${T.s3}`, background: "rgba(180,130,60,0.07)", border: `1px solid ${T.border}`, borderRadius: T.r4 }}>
              <span style={{ fontFamily: T.heading, fontSize: T.xs, color: T.gold }}>{character.name}</span>
              <span style={{ fontFamily: T.heading, fontSize: 9, color: character.currentHp / character.hp > 0.5 ? T.hp : T.hpLow }}>❤ {character.currentHp}/{character.hp}</span>
            </div>
          )}
          {worldState.session.phase !== "exploration" && (
            <span style={{ fontFamily: T.heading, fontSize: 9, color: "#e06060", background: "rgba(180,60,60,0.1)", padding: `2px 8px`, border: "1px solid rgba(180,60,60,0.3)", borderRadius: T.r4, letterSpacing: "1px" }}>
              {worldState.session.phase.toUpperCase()}
            </span>
          )}
        </div>
      </div>

      <div style={{ flex: 1, overflow: "hidden", display: "flex", flexDirection: "column" }}>
        {tab === "story" && (
          <StoryEngine character={character} worldState={fullWorldState} dispatch={fullDispatch} selectedSetting={selectedSetting} />
        )}
        {tab === "character" && (
          <div style={{ height: "100%", display: "flex", flexDirection: "column", overflow: "hidden" }}>
            {character && (
              <div style={{ display: "flex", borderBottom: `1px solid ${T.border}`, background: "rgba(4,2,8,0.97)", flexShrink: 0 }}>
                {[["create", "✏️ Edit"], ["sheet", "📋 Sheet"]].map(([v, l]) => (
                  <button key={v} onClick={() => setCharView(v)} style={{ flex: 1, padding: T.s3, background: "transparent", border: "none", borderBottom: `2px solid ${charView === v ? T.gold : "transparent"}`, color: charView === v ? T.gold : T.textSecondary, cursor: "pointer", fontFamily: T.heading, fontSize: T.xs, letterSpacing: "0.5px", minHeight: 44 }}>{l}</button>
                ))}
              </div>
            )}
            <div style={{ flex: 1, overflow: "hidden" }}>
              {charView === "sheet" && character
                ? <CharSheet character={character} setCharacter={setCharacter} />
                : <CharCreation onComplete={(c) => { setCharacter(c); setCharView("sheet"); setTab("story"); }} />
              }
            </div>
          </div>
        )}
        {tab === "map" && <BattleMap character={character} worldState={fullWorldState} />}
        {tab === "compendium" && <Compendium character={character} setCharacter={setCharacter} />}
      </div>

      <div style={{ display: "flex", background: "rgba(4,2,8,0.97)", borderTop: `1px solid ${T.border}`, flexShrink: 0, height: 60 }}>
        {NAV.map(item => (
          <button key={item.id} onClick={() => setTab(item.id)}
            style={{ flex: 1, background: "transparent", border: "none", borderTop: `2px solid ${tab === item.id ? T.gold : "transparent"}`, display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center", gap: 3, cursor: "pointer", WebkitTapHighlightColor: "transparent", transition: "all 0.15s", padding: `${T.s2} 0` }}>
            <span style={{ fontSize: 20 }}>{item.icon}</span>
            <span style={{ fontFamily: T.heading, fontSize: 9, letterSpacing: "0.5px", color: tab === item.id ? T.gold : T.textSecondary }}>{item.label}</span>
          </button>
        ))}
      </div>
    </div>
  );
}

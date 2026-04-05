## Warlock Leveling Macros (Vanilla 1.12, English Client)

These macros are based on existing patterns used across this repository:
- heavy use of `/run` Lua checks for buffs/debuffs/pet state,
- anti-spam channel protection,
- pet control bundled into offensive buttons.

> Replace spell ranks with your current learned rank while leveling.

---

### 1) Pull opener: Curse of Agony + send pet
```lua
/run local _,_,_,_,isActive=GetPetActionInfo(10) if not isActive and not GetUnitName("pettarget") then PetAttack() end
/cast Curse of Agony
```

Why: starts damage instantly and makes sure your demon engages.

---

### 2) DoT rotator (smart order)
```lua
/run c=CastSpellByName function b(k)for i=1,16 do if strfind(tostring(UnitDebuff("target",i)),k)then return 1 end end end if not b("ionExp")then c("Corruption(Rank 6)")elseif not b("eOfSar")then c("Curse of Agony(Rank 6)")else c("Siphon Life(Rank 4)")end
```

Why: one-button DoT maintenance for grinding when solo.

---

### 3) Simple fixed DoT sequence (easy spam while questing)
```lua
/run s={"Siphon Life","Curse of Agony","Corruption"} if not q then q=1 end CastSpellByName(s[q]) q=q+1 if q>table.getn(s) then q=1 end
```

Why: no debuff scan needed; stable rhythm for fast leveling.

---

### 4) Fear panic (interrupt current cast)
```lua
/script SpellStopCasting()
/cast Fear
```

Why: emergency peel for melee mobs.

---

### 5) Drain Life anti-spam (prevents recasting mid-channel)
```lua
/script if not CastingBarFrame.channeling then CastSpellByName("Drain Life") end
```

Why: prevents wasted GCD from spam clicking.

---

### 6) Drain Soul finisher + pull pet back
```lua
/run CastSpellByName("Drain Soul(Rank 1)") if UnitIsUnit("playertarget","pettarget") then PetPassiveMode() end
```

Why: secures shard generation and avoids pet stealing the last hit.

---

### 7) Wand shooter anti-spam
```lua
/run for i=1,120 do if IsAutoRepeatAction(i) then return end end CastSpellByName("Shoot")
```

Why: classic wand-finisher for mana efficiency.

---

### 8) Life Tap / Demon Armor smart key
```lua
/run local i,x=1,0 while UnitBuff("player",i) do if UnitBuff("player",i)=="Interface\\Icons\\Spell_Shadow_RagingScream" then x=1 end i=i+1 end if x==0 then CastSpellByName("Demon Armor") else CastSpellByName("Life Tap")end
```

Why: if Demon Armor is missing, rebuff; otherwise converts HP to mana.

---

### 9) Pet attack/follow toggle (excellent for mob control)
```lua
/run if not UnitExists("target") then PetFollow(); return; end if not UnitIsUnit("target", "pettarget") then PetAttack(target); else PetFollow(); end
```

Why: tap once to send pet, tap again on same target to recall.

---

### 10) Healthstone or Healing Potion (survival button)
```lua
/run for b=0,4 do for s=1,GetContainerNumSlots(b,s)do local n=GetContainerItemLink(b,s)if n and (strfind(n,"Healthstone") or strfind(n,"Healing Potion"))then UseContainerItem(b,s,1)end end end
```

Why: fastest self-save during bad pulls.

---

## Recommended leveling keybind flow
1. Pull with **Curse+Pet**.
2. Apply DoTs with **Smart DoT** or **Fixed Sequence**.
3. Use **Drain Life** if stable, **Fear** if pressured.
4. Finish with **Wand** or **Drain Soul** when low HP mob.
5. Use **Healthstone/Potion** for emergency.

## Notes for English client
- Keep spell names exactly in English (e.g., `Curse of Agony`, `Drain Soul`, `Shoot`).
- If a macro fails after ranking up, update rank text manually.
- Vanilla macro limit is short; compact Lua style is normal in this repo.

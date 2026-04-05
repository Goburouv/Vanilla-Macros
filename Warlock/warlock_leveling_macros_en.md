## Warlock Leveling Macros (Vanilla 1.12, English client)

Practical leveling setup based on macros already present in this repository.

> Keep spell names in English inside `/cast` lines.

---

## Level 10–20 (early leveling)

### 1) Pull: Curse of Agony + send pet
```lua
/run local _,_,_,_,isActive=GetPetActionInfo(10) if not isActive and not GetUnitName("pettarget") then PetAttack() end
/cast Curse of Agony
```

### 2) Panic Fear (stop current cast first)
```lua
/script SpellStopCasting()
/cast Fear
```

### 3) Drain Soul finisher (keep rank 1)
```lua
/run CastSpellByName("Drain Soul(Rank 1)") if UnitIsUnit("playertarget","pettarget") then PetPassiveMode() end
```

### 4) Wand anti-spam
```lua
/run for i=1,120 do if IsAutoRepeatAction(i) then return end end CastSpellByName("Shoot")
```

---

## Level 20–40 (stable grind)

### 5) Smart DoT rotator (Corruption -> CoA -> Siphon Life)
```lua
/run c=CastSpellByName function b(k)for i=1,16 do if strfind(tostring(UnitDebuff("target",i)),k)then return 1 end end end if not b("ionExp")then c("Corruption(Rank 6)")elseif not b("eOfSar")then c("Curse of Agony(Rank 6)")else c("Siphon Life(Rank 4)")end
```

### 6) Simple DoT sequence (single button)
```lua
/run s={"Siphon Life","Curse of Agony","Corruption"} if not q then q=1 end CastSpellByName(s[q]) q=q+1 if q>table.getn(s) then q=1 end
```

### 7) Drain Life anti-channel-break
```lua
/script if not CastingBarFrame.channeling then CastSpellByName("Drain Life") end
```

### 8) Demon Armor if missing, else Life Tap
```lua
/run local i,x=1,0 while UnitBuff("player",i) do if UnitBuff("player",i)=="Interface\\Icons\\Spell_Shadow_RagingScream" then x=1 end i=i+1 end if x==0 then CastSpellByName("Demon Armor") else CastSpellByName("Life Tap")end
```

---

## Level 40–60 (faster multi-pull rhythm)

### 9) Pet Attack/Follow toggle
```lua
/run if not UnitExists("target") then PetFollow(); return; end if not UnitIsUnit("target", "pettarget") then PetAttack(target); else PetFollow(); end
```

### 10) Emergency: Healthstone or Healing Potion
```lua
/run for b=0,4 do for s=1,GetContainerNumSlots(b) do local n=GetContainerItemLink(b,s) if n and (strfind(n,"Healthstone") or strfind(n,"Healing Potion")) then UseContainerItem(b,s,1) return end end end
```

---

## Suggested action bar order
1. Pull (macro 1)
2. Smart DoT (macro 5) or Simple DoT (macro 6)
3. Fear panic (macro 2)
4. Drain Life (macro 7)
5. Drain Soul finisher (macro 3)
6. Wand (macro 4)
7. Pet toggle (macro 9)
8. Emergency consumable (macro 10)
9. Life Tap / Demon Armor (macro 8)

---

## Notes
- Update `Rank` values as you level.
- If you do not have Siphon Life yet, remove it from macros 5 and 6.
- Rank 1 Drain Soul is intentional for shard efficiency.

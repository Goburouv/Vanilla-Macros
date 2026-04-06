## Priest Leveling Macros (Vanilla 1.12, English client)

Lista práctica de macros para leveo de Sacerdote, priorizando eficiencia de maná, control de casteo y fluidez.

> Usa nombres de hechizos en inglés y ajusta `Rank` según tu nivel.

---

## 1) Self Flash Heal o target Flash Heal (sin perder ritmo)
```lua
/run SpellStopCasting()
/run if UnitIsFriend("player","target") then CastSpellByName("Flash Heal") else CastSpellByName("Flash Heal",1) end; TargetLastEnemy(); UIErrorsFrame:Clear()
```

## 2) Self Heal o target Heal
```lua
/run SpellStopCasting()
/run if UnitIsFriend("player","target") then CastSpellByName("Heal") else CastSpellByName("Heal",1) end; TargetLastEnemy(); UIErrorsFrame:Clear()
```

## 3) Heal downrank dinámico (ahorro de maná)
```lua
/script if(UnitHealth("target")/UnitHealthMax("target")<0.50) then CastSpellByName("Flash Heal(Rank 3)") else CastSpellByName("Flash Heal(Rank 1)") end
```

## 4) Cancelar heal si el target ya está bien
```lua
/script if CastingBarFrame:IsShown() and UnitHealth("target")/UnitHealthMax("target") > 0.8 then SpellStopCasting() elseif not CastingBarFrame:IsShown() then CastSpellByName("Flash Heal(Rank 6)") end
```

## 5) Shoot (wand) anti-spam
```lua
/run for i=1,120 do if IsAutoRepeatAction(i) then return end end CastSpellByName("Shoot")
```

## 6) Lock anti-toggle para wand
```lua
/run GGUseAction=GGUseAction or UseAction;UseAction=function(id,a,b)if not IsCurrentAction(id)and not IsAutoRepeatAction(id)then GGUseAction(id,a,b)end end
/cast Shoot
```

## 7) Renew Stopper (no sobrescribir renew activo)
```lua
/script fred = 0 for i=1,16 do if UnitBuff("target", i) then if string.find(UnitBuff("target", i), "Renew") then fred = 1 end end end if fred == 0 then CastSpellByName("Renew") end
```

## 8) Macro de ahorro de maná para healing continuo
```lua
/script p="player";m=UnitMana(p);mm=UnitManaMax(p);if (m < (mm/10)) then SendChatMessage("I am at 10% of my mana, next healer please take over.", "SAY") else CastSpellByName("FlashHeal(Rank 7)") end
```

## 9) Multi-hechizo por modificadores (1 tecla)
```lua
/script local d=CastSpellByName; if(IsControlKeyDown()) then d("Shadow Word: Pain") elseif (IsAltKeyDown()) then d("Smite") elseif (IsShiftKeyDown()) then d("Mind Blast") else d("Psychic Scream") end
```

## 10) Dismount + cast instantáneo
```lua
/dismount
/cast Power Word: Shield
```

## 11) Dispel Magic rápido
```lua
/run SpellStopCasting()
/cast Dispel Magic
```

## 12) Buff rápido: Inner Fire
```lua
/cast Inner Fire
```

---

## Barra recomendada de leveo (Shadow/Holy)
1. Wand anti-spam (macro 5)
2. Self/target Flash Heal (macro 1)
3. Heal downrank (macro 3)
4. Cancel-heal inteligente (macro 4)
5. Renew Stopper (macro 7)
6. Dispel rápido (macro 11)
7. Modifiers DPS/CC (macro 9)
8. Shield instant (macro 10)

---

## Consejos finales
- Para **Shadow leveando**: prioriza daño + wand + auto-heal de emergencia.
- Para **Holy/Disc leveando en grupo**: prioriza downrank + cancel-heal + control de maná.
- Si un macro deja de funcionar al subir nivel, revisa nombres/ranks exactos de los hechizos.

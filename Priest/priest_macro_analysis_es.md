## Análisis de macros para Priest (Vanilla 1.12)

Este análisis resume cómo están construidos los macros de Priest en este repositorio y qué patrones son más útiles para leveo.

---

## 1) Patrones técnicos que más se repiten

### A. Control de casteo (`SpellStopCasting`)
Se usa para cortar casteos largos y reaccionar más rápido (pánico, cambio de objetivo, auto-heal).

### B. Condiciones de vida/mana
Muchos macros toman decisiones por porcentaje o por HP faltante:
- cambiar rango del heal,
- frenar un heal si el target ya está sano,
- avisar cuando estás OOM.

### C. Protección anti-spam para wand/autoattack
Priest usa mucho wand en Vanilla, así que el repo evita reactivar `Shoot` accidentalmente.

### D. Self-cast sin perder target
Patrón clásico de Vanilla: castear en ti mismo con `CastSpellByName("Spell",1)` o con lógica de target/retorno.

---

## 2) Qué filosofía de juego reflejan estos macros

1. **Eficiencia de maná** (downranking y cancelación inteligente de heals).
2. **Fluidez** (menos clicks inútiles, menos cambio manual de objetivo).
3. **Supervivencia** (panic buttons con stopcast).
4. **Tempo de leveo** (DoT + wand + control de pet/target del grupo cuando aplica).

---

## 3) Macros de Priest más útiles para leveo (según el repo)

### 1) Self Flash Heal o target Heal
```lua
/run SpellStopCasting()
/run if UnitIsFriend ("player", "target") then CastSpellByName("Flash Heal") else CastSpellByName("Flash Heal",1)  end; TargetLastEnemy();UIErrorsFrame:Clear()
```

### 2) Heal por porcentaje (downrank dinámico)
```lua
/script if(UnitHealth("target")/UnitHealthMax("target")<0.50) then CastSpellByName("Flash Heal(Rank 3)")else CastSpellByName("Flash Heal(Rank 1)"); end
```

### 3) Cancelar heal si ya no hace falta
```lua
/script if CastingBarFrame:IsShown() and UnitHealth("target")/UnitHealthMax("target") > 0.8 then SpellStopCasting() elseif not CastingBarFrame:IsShown() then CastSpellByName("Flash Heal(Rank 6)") end
```

### 4) Wand anti-spam
```lua
/run for i=1,120 do if IsAutoRepeatAction(i) then return end end CastSpellByName("Shoot")
```

### 5) Stop toggling de wand/autoattack
```lua
/run GGUseAction=GGUseAction or UseAction;UseAction=function(id,a,b)if not IsCurrentAction(id)and not IsAutoRepeatAction(id)then GGUseAction(id,a,b)end end
/cast Shoot
```

### 6) Macro de ahorro de maná en rotación de healing
```lua
/script p="player";m=UnitMana(p);mm=UnitManaMax(p);if (m < (mm/10)) then SendChatMessage("I am at 10% of my mana, next healer please take over.", "SAY") else CastSpellByName("FlashHeal(Rank 7)"); end
```

---

## 4) Recomendación de barra para Priest leveando

1. Shield / Renew manual
2. Self-or-target Flash Heal
3. Heal de downrank
4. Stop-heal si el target sube de vida
5. Wand anti-spam
6. Wand lock (anti-toggle)

---

## 5) Notas importantes

- Cliente en inglés: usa nombres exactos de hechizos en `/cast` y `CastSpellByName`.
- Ajusta `Rank` conforme subes nivel.
- Si juegas Shadow, prioriza macros de flujo (DoT + wand); si juegas Holy/Disc, prioriza macros de eficiencia de curas.

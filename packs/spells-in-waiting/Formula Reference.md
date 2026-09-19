Aero
Aero II
Aero III
Aero IV
Aera
Aeroga
Abyss
Abyss II


The following YAML files contain partially incorrect damage formulas. It is your job to go through each of these files and replace the values within the formula section under damage with an appropriate formula as follows:
1d6 + @spells.primary.abilityMod + min(@spells.primary.cl.total,5)
3d6 + @spells.primary.abilityMod + min(@spells.primary.cl.total,10)
5d6 + @spells.primary.abilityMod + min(@spells.primary.cl.total,15)
7d6 + @spells.primary.abilityMod + min(@spells.primary.cl.total,20)
(min(@spells.primary.cl.total, 10))d6
(min(@spells.primary.cl.total, 15))d6
(min(@spells.primary.cl.total, 15))d8
(min(@spells.primary.cl.total, 20))d8

The following spell files already have these fomulas applied as examples for you to go over when needed: Aero, Aero II, Aero III, Aero IV, Aera, Aeroga, Abyss, Abyss II.
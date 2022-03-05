<template>
	<v-container>
		<v-row justify="space-between">
			<v-col cols="7">
				<ClassesComponent
					:jsonData="this.jsonData"
					:SelectedClasses="this.SelectedClasses"
					:SelectedTurn="SelectedTurn"
					@spellCasted="addSpellCasted"
					:AllClasses="AllClasses"
					@changeSelectedClasses="updateSelectedClasses"
				/>
			</v-col>
			<v-col cols="5">
				<TimeLine
					:SelectedTurn="SelectedTurn"
					:spells_casted="spells_casted"
					@selectTurn="(n) => (SelectedTurn = n)"
				/>
			</v-col>
		</v-row>
	</v-container>
</template>

<script>
import ClassesComponent from "./ClassesComponent.vue";
import TimeLine from "./TimeLine.vue";

import json from "../../public/classesData.json";

export default {
	name: "Layout",
	components: { ClassesComponent, TimeLine },
	data() {
		return {
			jsonData: json,
			AllClasses: [
				"Cra",
				"Ecaflip",
				"Eliotrope",
				"Eniripsa",
				"Enutrof",
				"Feca",
				"Foggernaut",
				"Huppermage",
				"Iop",
				"Masqueraider",
				"Osamodas",
				"Ouginak",
				"Pandawa",
				"Rogue",
				"Sacrier",
				"Sadida",
				"Sram",
				"Xelor",
			],
			SelectedClasses: ["", "", ""],
			SelectedTurn: 1,
			spells_casted: [],
			spells_last_id: 3,
		};
	},
	methods: {
		addSpellCasted(DofusClass, Turn, SpellName, SpellCooldown) {
			let calculatedTurn = parseInt(Turn) + parseInt(SpellCooldown);
			this.spells_last_id = this.spells_last_id + 1;
			this.spells_casted.push({
				id: this.spells_last_id,
				DofusClass: DofusClass,
				Turn: Turn,
				SpellName: SpellName,
				SpellCooldown: SpellCooldown,
				Spell_up_again: calculatedTurn,
			});
		},
		updateSelectedClasses(newClasses) {
			this.SelectedClasses = newClasses;
			while (this.SelectedClasses.length != 3) {
				this.SelectedClasses.push("");
			}
		},
	},
};
</script>

<style></style>

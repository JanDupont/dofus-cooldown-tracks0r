<template>
	<v-container>
		<v-timeline dense id="my-timeline-container">
			<v-timeline-item color="green lighten-1" dark fill-dot right v-for="turn in maxTurns" :key="turn">
				<v-card @click="selectTurn(turn)">
					<v-card-title
						:class="{ blue: SelectedTurn === turn, green: SelectedTurn !== turn, 'lighten-1': true }"
					>
						<v-icon dark size="42" class="mr-4"> mdi-sword-cross </v-icon>
						<h2 class="text-h4 white--text font-weight-light">Turn: {{ turn }}</h2>
					</v-card-title>
					<v-container>
						<v-row>
							<v-col cols="12" md="12" id="bigContainer">
								<h3>Used Spells this turn:</h3>
								<div v-for="spell in spells_casted" :key="'A' + spell.id" id="midContainer">
									<v-container v-if="spell.Turn == turn">
										<img
											v-if="spell.Turn == turn"
											:src="
												require('../../public/images/' +
													spell.DofusClass +
													'/' +
													spell.SpellName +
													'.png')
											"
										/>
									</v-container>
								</div>

								<h3>Spells on Cooldown:</h3>
								<div v-for="spell in spells_casted" :key="'B' + spell.id" id="midContainer">
									<v-container v-if="turn >= spell.Turn && turn < spell.Spell_up_again">
										<img
											v-if="turn >= spell.Turn && turn <= spell.Spell_up_again"
											:src="
												require('../../public/images/' +
													spell.DofusClass +
													'/' +
													spell.SpellName +
													'.png')
											"
										/>
									</v-container>
								</div>
							</v-col>
						</v-row>
					</v-container>
				</v-card>
			</v-timeline-item>
		</v-timeline>
	</v-container>
</template>

<script>
export default {
	props: {
		SelectedTurn: Number,
		spells_casted: Array,
	},
	data() {
		return {
			maxTurns: 50,
		};
	},
	methods: {
		selectTurn(turn) {
			this.$emit("selectTurn", turn);
		},
	},
};
</script>

<style scoped>
#my-timeline-container {
	max-height: 91vh;
	overflow: scroll;
}
#bigContainer {
	display: table-row;
	box-sizing: border-box;
}
#midContainer {
	display: inline-block;
	box-sizing: border-box;
}
</style>

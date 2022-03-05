<template>
	<v-container>
		<v-row justify="center">
			<v-col cols="9">
				<div v-for="(myClass, index) in this.SelectedClasses" :key="index">
					<v-row>
						<v-container id="bigContainer">
							<v-select
								v-model="InputClasses[index]"
								:items="AllClasses"
								outlined
								dark
								@change="VSelectChanged"
								:placeholder="'Class ' + (index + 1)"
							></v-select>
							<div id="midContainer" v-for="spell in jsonData[myClass]" :key="'C' + spell.id">
								<div id="smallContainer">
									<img
										@click="spellClick(spell.Variant1, myClass)"
										:src="
											require('../../public/images/' +
												myClass +
												'/' +
												spell.Variant1.Name +
												'.png')
										"
										:class="
											activatedSpells.includes(spell.Variant1.Name) ? 'coloredImage' : 'greyImage'
										"
									/>
								</div>
								<div id="smallContainer">
									<img
										@click="spellClick(spell.Variant2, myClass)"
										:src="
											require('../../public/images/' +
												myClass +
												'/' +
												spell.Variant2.Name +
												'.png')
										"
										:class="
											activatedSpells.includes(spell.Variant2.Name) ? 'coloredImage' : 'greyImage'
										"
									/>
								</div>
							</div>
						</v-container>
					</v-row>
				</div>
			</v-col>
			<v-col cols="3">
				<v-container>
					<v-card id="info_card">
						<v-container>
							<div class="text-h6 blue-grey--text text--lighten-5">
								Selected Turn: <span class="blue--text text--lighten-1">{{ SelectedTurn }}</span>
							</div>
							<br />
							<div class="text-h6 orange--text text--lighten-1">Mode:</div>
							<div class="text-h6 orange--text text--lighten-1" v-if="trackCooldown">Track Cooldown</div>
							<div class="text-h6 orange--text text--lighten-1" v-if="!trackCooldown">
								Select Variants
							</div>
							<v-btn class="green lighten-1" block @click="trackCooldown = !trackCooldown"
								>Switch Mode</v-btn
							>
						</v-container>
					</v-card>
				</v-container>
			</v-col>
		</v-row>
	</v-container>
</template>

<script>
export default {
	props: {
		jsonData: Object,
		SelectedClasses: Array,
		SelectedTurn: Number,
		AllClasses: Array,
	},
	data() {
		return {
			activatedSpells: [],
			trackCooldown: false,
			InputClasses: [],
		};
	},
	methods: {
		spellClick(variant, myClass) {
			// Mode: Select Variants
			if (this.trackCooldown === false) {
				// if is selected
				if (this.activatedSpells.includes(variant.Name)) {
					this.activatedSpells = this.activatedSpells.filter(function (spell) {
						return spell !== variant.Name;
					});
				}
				// if is not selected
				else {
					this.activatedSpells.push(variant.Name);
				}
			}
			// Mode: Track Cooldown
			else {
				this.$emit("spellCasted", myClass, this.SelectedTurn, variant.Name, variant.Cooldown);
			}
		},
		VSelectChanged() {
			this.$emit("changeSelectedClasses", this.InputClasses);
		},
	},
	mounted() {},
};
</script>

<style>
#bigContainer {
	display: table-row;
	box-sizing: border-box;
	background-color: #1e1e1e;
	border-radius: 10px;
	max-width: 95%;
}
#midContainer {
	display: inline-block;
	box-sizing: border-box;
}
#smallContainer {
	height: 44px;
	margin: 3px;
	width: 55px;
	text-align: center;
}
#smallContainer:hover {
	cursor: pointer;
}
.greyImage {
	filter: grayscale(100%);
	max-height: 44px;
	max-width: 44px;
}
.coloredImage {
	filter: none;
	max-height: 44px;
	max-width: 44px;
}
#info_card {
	background-color: #1e1e1e;
	border-radius: 6px;
}
</style>

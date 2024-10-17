<template>
	<v-app>
		<v-app-bar app color="primary" dark>
			<div class="d-flex align-center">
				<v-img
					alt="Vuetify Logo"
					class="shrink mr-2"
					contain
					src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
					transition="scale-transition"
					width="40"
				/>

				<v-img
					alt="Vuetify Name"
					class="shrink mt-1 hidden-sm-and-down"
					contain
					min-width="100"
					src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
					width="100"
				/>
			</div>

			<v-spacer></v-spacer>

			<v-btn href="https://github.com/vuetifyjs/vuetify/releases/latest" target="_blank" text>
				<span class="mr-2">Latest Release</span>
				<v-icon>mdi-open-in-new</v-icon>
			</v-btn>
		</v-app-bar>

		<v-main>
			<v-container class="d-flex justify-center align-center">
				<div>
					<h3 class="text-center my-4">Calendar Test</h3>
					<v-date-picker
						v-model="date"
						@contextmenu:year="contextMenu"
						@dblclick:date="dblClick"
						@mouseenter:month="mouseEnter"
						@mouseleave:month="mouseLeave"
					></v-date-picker>
					<!-- <HelloWorld/> -->
				</div>
			</v-container>
		</v-main>
	</v-app>
</template>

<script>
// import HelloWorld from './components/HelloWorld'

export default {
	name: 'App',

	components: {
		// HelloWorld,
	},

	data: () => ({
		date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
			.toISOString()
			.substr(0, 10),
		done: [false, false, false],
		mouseMonth: null,
	}),

	methods: {
		contextMenu(year, event) {
			this.$set(this.done, 2, true)

			event.preventDefault()

			alert(`You have activated context menu for year ${year}`)
		},
		dblClick(date) {
			this.$set(this.done, 0, true)

			alert(`You have just double clicked the following date: ${date}`)
		},
		mouseEnter(month) {
			this.$set(this.done, 1, true)
			this.mouseMonth = month
		},
		mouseLeave() {
			this.mouseMonth = null
		},
	},
}
</script>

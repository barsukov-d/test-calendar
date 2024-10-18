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
						@click:date="openAddEventDialog"
						:events="functionEvents"
					></v-date-picker>
					<!-- <HelloWorld/> -->
				</div>
			</v-container>

			<pre>{{ meetingDays }}</pre>
			<pre>{{ birthdayDays }}</pre>
			<pre>{{ holidayDays }}</pre>
		</v-main>

		<v-dialog v-model="addEventDialog" max-width="500px">
			<v-card>
				<v-card-title>
					<span class="headline">Добавить событие</span>
				</v-card-title>
				<v-card-text>
					<v-form ref="form" @submit.prevent="submitEvent">
						<v-text-field
							label="Название события"
							v-model="formData.eventName"
							required
						></v-text-field>
						<v-textarea label="Описание" v-model="formData.description"></v-textarea>
						<v-select
							:items="eventTypes"
							item-text="text"
							item-value="value"
							label="Тип события"
							v-model="formData.type"
							required
						></v-select>
						<v-menu
							v-model="startTimeMenu"
							:close-on-content-click="false"
							transition="scale-transition"
							offset-y
							min-width="auto"
						>
							<template v-slot:activator="{ on, attrs }">
								<v-text-field
									v-model="formData.startTime"
									label="Время начала"
									prepend-icon="mdi-clock"
									readonly
									v-bind="attrs"
									v-on="on"
									required
								></v-text-field>
							</template>
							<v-time-picker
								v-model="formData.startTime"
								@click:minute="startTimeMenu = false"
							></v-time-picker>
						</v-menu>
						<v-menu
							v-model="endTimeMenu"
							:close-on-content-click="false"
							transition="scale-transition"
							offset-y
							min-width="auto"
						>
							<template v-slot:activator="{ on, attrs }">
								<v-text-field
									v-model="formData.endTime"
									label="Время окончания"
									prepend-icon="mdi-clock"
									readonly
									v-bind="attrs"
									v-on="on"
									required
								></v-text-field>
							</template>
							<v-time-picker
								v-model="formData.endTime"
								@click:minute="endTimeMenu = false"
							></v-time-picker>
						</v-menu>
					</v-form>
				</v-card-text>
				<v-card-actions>
					<v-spacer></v-spacer>
					<v-btn color="blue darken-1" text @click="closeAddEventDialog">Отмена</v-btn>
					<v-btn color="blue darken-1" text @click="submitEvent">Сохранить</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>
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
		addEventDialog: false,
		startTimeMenu: false,
		endTimeMenu: false,
		formData: {
			eventName: '',
			description: '',
			type: '',
			date: '',
			startTime: '',
			endTime: '',
		},
		eventTypes: [
			{ text: 'Встреча', value: 'meeting', color: '#F44336' }, // Красный
			{ text: 'День рождения', value: 'birthday', color: '#2196F3' }, // Синий
			{ text: 'Праздник', value: 'holiday', color: '#4CAF50' }, // Зеленый
			// Добавьте другие типы событий здесь
		],
		arrayEvents: [], // Массив для хранения событий
	}),

	computed: {
		daysByEventType() {
			const types = ['meeting', 'birthday', 'holiday']
			const result = {}

			types.forEach((type) => {
				result[`${type}Days`] = this.arrayEvents
					.filter((event) => event.formData.type === type)
					.map((event) => {
						event.date
						return event.date
					})
			})

			return result
		},

		// Отдельные вычисляемые свойства для каждого типа события
		meetingDays() {
			return this.daysByEventType.meetingDays
		},
		birthdayDays() {
			return this.daysByEventType.birthdayDays
		},
		holidayDays() {
			return this.daysByEventType.holidayDays
		},
	},

	methods: {
		functionEvents(date) {
			// const [, , day] = date.split('-')
			// const dayNum = parseInt(day, 10)
			const colors = []

			if (this.meetingDays.includes(date)) colors.push('green')
			if (this.birthdayDays.includes(date)) colors.push('blue')
			if (this.holidayDays.includes(date)) colors.push('red')

			return colors.length > 0 ? colors : false
		},

		openAddEventDialog(date) {
			this.selectedDate = date
			this.formData.date = date
			this.addEventDialog = true
		},
		closeAddEventDialog() {
			this.addEventDialog = false
			this.resetForm()
		},
		resetForm() {
			this.formData = {
				eventName: '',
				description: '',
				type: '',
				date: this.selectedDate,
				startTime: '',
				endTime: '',
			}
		},
		submitEvent() {
			if (
				this.formData.eventName &&
				this.formData.type &&
				this.formData.startTime &&
				this.formData.endTime
			) {
				this.arrayEvents.push({
					date: this.formData.date,
					// color: this.getEventColor(this.formData.type),
					formData: this.formData,
				})
				this.closeAddEventDialog()
				console.log('this.arrayEvents', this.arrayEvents)
			} else {
				alert('Пожалуйста, заполните все обязательные поля.')
			}
		},
		// getEventColor(eventType) {
		// 	const event = this.eventTypes.find((et) => et.value === eventType)
		// 	return event ? event.color : '#9E9E9E' // Серый по умолчанию
		// },
		// getEventColorForDate(date) {
		// 	const matchingEvents = this.arrayEvents.filter((event) => event.date === date)
		// 	if (matchingEvents.length === 0) return false
		// 	// Возвращает массив цветов для всех событий на дату
		// 	return matchingEvents.map((event) => event.color)
		// },
	},
}
</script>

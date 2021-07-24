<template>
  <div class="content-box">
		<div class="p-15">
			<label class="date-field-label d-block">Date of birth</label>
			<div class="d-block">
				<input class="dateField" type="date" v-model="dob" @change="dobChanged">
			</div>
			<div v-if="!dobValid" class="d-block error_box">
				Please select a date in the past.
			</div>
			<template v-else>
				<div class="buttons-box" v-if="dob != null">
					<button v-if="!isCustom" color="primary" expand="block" @click="calculateDOBYesterday()">
						<b>
							Age Yesterday
						</b>
					</button>
					<button v-if="!isCustom" color="secondary" expand="block" @click="calculateDOBToday()">
						<b>
							Age Today
						</b>
					</button>
					<button v-if="!isCustom" color="warning" expand="block" @click="calculateDOBTomorrow()">
						<b>
							Age Tomorrow
						</b>
					</button>
				</div>
				<div v-if="dob != null">
					<div class="custom-box">
						<input
							type="checkbox"
							v-model="isCustom"
							color="primary" 
						>
						<label>Custom</label>
					</div>
				</div>
				<div v-if="isCustom" class="text-left mt-15">
					<label class="date-field-label d-block">Custom Date</label>
					<input class="dateField" type="date" v-model="customDate">
					<div class="text-center" v-if="customDate != null">
						<button class="block" color="primary" expand="block" @click="calculateCustomDate()">
							<b>
								Calculate
							</b>
						</button>
					</div>
				</div>
				<div class="p-15" v-if="result.day != null">

					<div>
						<div id="result"  class="p-15">
							<div>
								<span id="descriptive_result">{{ result.descriptive }}</span>
							</div>
							<h3 class="flex-between">
								Days
								<b>{{ result.day }}</b>
							</h3> 
							<h3 class="flex-between">
								Weeks
								<b>{{ result.weekDay }}</b>
							</h3> 
							<h3 class="flex-between">
								Months
								<b>{{ result.monthWeekDay }}</b>
							</h3> 
							<h3 class="flex-between">
								Years
								<b>{{ result.yearMonthWeekDay }}</b>
							</h3> 
						</div>
					</div>
					<button color="danger" class="block" expand="block" @click="reset()">
						<i name="trash-outline"></i>
						<b>
							Reset
						</b>
					</button>
				</div>
			</template>
		</div>


  </div>
</template>

<script>
	import moment from 'moment';
	import { intervalToDuration, formatDuration } from 'date-fns'
	export default {
		data() {
			return {
				dob: null,
				customDate: null,
				dobValid: true,
				result: {
					day: null,
					weekDay: null,
					monthWeekDay: null,
					yearMonthWeekDay: null,
					descriptive: null,
				},
				isCustom: false
			}
		},
		methods: {
			dobChanged() {
				const dobMoment = moment(this.dob, 'YYYY-MM-DD');
				const currentMoment = moment();
				if(dobMoment.diff(currentMoment, 'days', true) > 0) {
					this.dobValid = false;
				} else {
					this.dobValid = true;
				}
			},
			calculateDOBToday() {
				this.calculateResult( moment().startOf('day') );
			},
			calculateDOBYesterday() {
				this.calculateResult( moment().subtract(1, 'days').startOf('day') );
			},
			calculateDOBTomorrow() {
				this.calculateResult( moment().add(1, 'days').startOf('day') );
			},
			calculateCustomDate() {
				const customDateMoment = moment(this.customDate, 'YYYY-MM-DD');
				this.calculateResult(customDateMoment);
			},
			calculateResult(dateToCalculateAgainst) {
				
				// const dateToCalculateAgainst = moment().startOf('day'); // doing it this way to lose the time info.
				const dobMoment = moment(this.dob, 'YYYY-MM-DD');
				
				this.result.day = dateToCalculateAgainst.diff(dobMoment, 'days', true).toFixed(2);
				this.result.weekDay = dateToCalculateAgainst.diff(dobMoment, 'weeks', true).toFixed(2);
				this.result.monthWeekDay = dateToCalculateAgainst.diff(dobMoment, 'months', true).toFixed(2);
				this.result.yearMonthWeekDay = dateToCalculateAgainst.diff(dobMoment, 'years', true).toFixed(2);

				const startDate = Date.parse(dobMoment.format('YYYY-MM-DD') + " 00:00:00 UTC");
				const endDate = Date.parse(dateToCalculateAgainst.format('YYYY-MM-DD') + " 00:00:00 UTC");
				const duration = intervalToDuration({start: startDate, end: endDate});
				const durationInWords = formatDuration(duration, {format: ["years", "months", "days"]});

				this.result.descriptive = durationInWords;
			},
			reset() {
				this.dob = null;
				this.customDate = null;
				this.result = {
					day: null,
					weekDay: null,
					monthWeekDay: null,
					yearMonthWeekDay: null,
					descriptive: null,
				};
				this.isCustom = false;
			}
		}
	}
</script>

<style scoped>
	.p-15{ padding: 15px; }
	.m-15{ margin: 15px; }
	.mt-15{ margin-top: 15px; }
	
	.d-block {
		display: block;
	}
	.date-field-label {
		font-size: 16px;
	}
	.error_box {
		background: maroon;
		color: white;
		text-align: center;
		padding: 14px;
	}

	.dateField {
		display: block;
		width: 100%;
		font-size: 20px;
		padding: 10px;
		margin-top: 10px;
		box-sizing: border-box;
	}

	.content-box {
		width: 800px;
		margin: auto;
		min-height: 520px;
	}

	input[type="checkbox"] {
		width: 20px;
    height: 20px;
		margin-right: 20px;
	}

	.custom-box {
		background: rgb(0, 39, 10);
		color: white;
		padding: 15px;
		display: flex;
		font-weight: bold;
		text-transform:none;
		letter-spacing: 3px;
		border-bottom-left-radius: 10px;
		border-bottom-right-radius: 10px;
		justify-content: center;
	}

	.buttons-box {
		display: flex;
		width: 100%;
		/* border-bottom: 10px solid green;
		border-bottom-left-radius: 10px;
		border-bottom-right-radius: 10px; */
	}

	button {
		
		background: black;
		color: white;
		padding: 20px 15px;
		font-size: 16px;
		font-weight:lighter;
		border: none;
		cursor: pointer;
		flex: 1;
		letter-spacing: 4px;
	}

	button.block {
		width: 100%;
		padding: 12px;
		border-bottom-left-radius: 10px;
		border-bottom-right-radius: 10px;
	}

	button[color="primary"] {
		background: blue;
	}
	button[color="danger"] {
		background: maroon;
	}
	button[color="warning"] {
		background: yellow;
		color: black;
	}

	#result {
		border: 2px solid #ebebeb;		
	}

	#descriptive_result {
		font-size: 22px;
		font-weight: bolder;
	}

	.flex-between {
		display: flex;
		justify-content: space-between;
		margin-top: 10px;
	}

	@media only screen and (max-width: 768px) {
    /* For mobile phones: */
    .content-box {
			width: 100%;
		}

		.buttons-box {
			flex-direction: column;
		}

		.buttons-box button {
			flex: none;
			display: block;
			width: 100%;
		}
  }
	
</style>
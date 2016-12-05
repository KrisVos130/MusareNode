<template>
	<div class='modal is-active'>
		<div class='modal-background'></div>
		<div class='modal-card'>
			<header class='modal-card-head'>
				<p class='modal-card-title'>Edit station</p>
				<button class='delete' @click='$parent.toggleModal("editStation")'></button>
			</header>
			<section class='modal-card-body'>
				<label class='label'>Display name</label>
				<div class='control is-grouped'>
					<p class='control is-expanded'>
						<input class='input' type='text' placeholder='Station Display Name' v-model='$parent.station.displayName'>
					</p>
					<p class='control'>
						<a class='button is-info' @click='updateDisplayName()'>Update</a>
					</p>
				</div>
				<label class='label'>Description</label>
				<div class='control is-grouped'>
					<p class='control is-expanded'>
						<input class='input' type='text' placeholder='Station Display Name' v-model='$parent.station.description'>
					</p>
					<p class='control'>
						<a class='button is-info' @click='updateDescription()'>Update</a>
					</p>
				</div>
				<label class='label'>Privacy</label>
				<div class='control is-grouped'>
					<p class='control is-expanded'>
						<span class='select'>
							<select v-model='$parent.station.privacy'>
								<option v-bind:value=''public''>Public</option>
								<option v-bind:value=''unlisted''>Unlisted</option>
								<option v-bind:value=''private''>Private</option>
							</select>
						</span>
					</p>
					<p class='control'>
						<a class='button is-info' @click='updatePrivacy()'>Update</a>
					</p>
				</div>
			</section>
		</div>
	</div>
</template>

<script>
	import { Toast } from 'vue-roaster';

	export default {
		methods: {
			updateDisplayName: function () {
				this.socket.emit('stations.updateDisplayName', this.$parent.stationId, this.$parent.station.displayName, res => {
					if (res.status == 'success') return Toast.methods.addToast(res.message, 4000);
					Toast.methods.addToast(res.message, 8000);
				});
			},
			updateDescription: function () {
				this.socket.emit('stations.updateDescription', this.$parent.stationId, this.$parent.station.description, res => {
					if (res.status == 'success') return Toast.methods.addToast(res.message, 4000);
					Toast.methods.addToast(res.message, 8000);
				});
			},
			updatePrivacy: function () {
				this.socket.emit('stations.updatePrivacy', this.$parent.stationId, this.$parent.station.privacy, res => {
					if (res.status == 'success') return Toast.methods.addToast(res.message, 4000);
					Toast.methods.addToast(res.message, 8000);
				});
			}
		},
		ready: function () {
			let _this = this;
			let socketInterval = setInterval(() => {
				if (!!_this.$parent.$parent.socket) {
					_this.socket = _this.$parent.$parent.socket;
					clearInterval(socketInterval);
				}
			}, 100);
		}
	}
</script>

<style type='scss' scoped>
	.controls {
		display: flex;

		a {
			display: flex;
    		align-items: center;
		}
	}

	.table { margin-bottom: 0; }

	h5 { padding: 20px 0; }
</style>
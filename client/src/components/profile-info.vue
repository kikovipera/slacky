<template>
	<div class="profile-container">
		<img class="profile-pic" :src="getProfilePic($store.state.user)" />
		<h4 class="profile-username">{{ $store.state.user.username }} </h4>

		<a class="logout-button" @click="logout">log out</a>
	</div>
</template>

<script>

import api from '../api';
import profilePicCache from '../misc/profile-pic-cache';

export default {
	name: 'profile-info',

	methods: {
		getProfilePic: user => profilePicCache.get(user),
		async logout() {
			const redirectToLoginPage = async () => {
				await this.$router.redirect('Login');
				this.$store.commit('resetState');
			};

			// log out and redirect at the same time, to reduce latency
			// not super duper great since logout could fail but w/e :P
			// also no need to actually await them even
			await Promise.all([api.logout(), redirectToLoginPage()]);
		}
	}
}
</script>

<style lang="scss">

$container-height: 40px;
.profile-container {
	height: $container-height;
	margin-top: 24px;
}

.profile-username {
	color: white;
	font-size: 24px;
	font-weight: 600;
	display: inline-block;

	vertical-align: middle;
	margin: 0;
	margin-left: 4px;

	max-width: 122px; /* meh, hardcoded. calc didn't work for some reason? */
	overflow: hidden;
	text-overflow: ellipsis;
}

.profile-pic {
	height: 100%;
	display: inline-block;

	border-radius: 20%;
	background-color: white;
}

/* todo: this is not especially well coded atm :P */
$base-color: #898989;
.logout-button {

	float: right;
	margin-right: 16px;
	line-height: $container-height;
	max-width: calc(100% - 120px);
	overflow: hidden;

	font-size: 16px;
	color: lighten($base-color, 5);

	cursor: pointer;
	user-select: none;

	&:hover {
		color: darken($base-color, 5);
		text-decoration: none;
	}

	&:active {
		color: darken($base-color, 10);
		text-decoration: none;
	}
}

</style>